# IPC Contract — Skema MQTT Lintas-Repo (KANONIK)

> **Sumber kebenaran tunggal.** Berkas ini mendefinisikan seluruh nama topik dan skema payload komunikasi antar-modul MoodIn. Repo lain **hanya mereferensikan** berkas ini, tidak mendefinisikan ulang. Setiap perubahan kontrak: perbarui berkas ini dan naikkan `schema_version`, baru ubah implementasi.

> **Status tahap Pra-TA (22 Nov 2026):** versi 2.0.0 menggantikan 1.0.0 era GEMASTIK. Perubahan utama ada di bagian 6. Bagian 3.1 dan 3.2 masih memakai isi 1.0.0 dan belum selaras dengan implementasi; keduanya diputuskan sebelum milestone M3.

- **Versi kontrak:** `2.0.0`
- **Field wajib di setiap payload:** `schema_version` (string), `session_id` (string), `ts` (epoch milidetik UTC, integer).
- **Encoding:** JSON UTF-8.

## 1. Broker dan transport

Mengikuti ADR-10 ([adr.md](adr.md)): broker berada di jaringan lokal perangkat, tanpa TLS.

| Lingkungan | Host | Port | TLS | Auth |
| --- | --- | --- | --- | --- |
| Perangkat dan pengembangan | broker lokal | `1883` | Tidak | _username/password_ wajib di perangkat; opsional saat pengembangan di laptop |

- Isi pesan dienkripsi sesuai ADR-10. Skema enkripsinya belum ditetapkan di kontrak ini; sampai ditetapkan, broker hanya boleh menerima koneksi dari jaringan lokal perangkat.
- Port 8883 dan TLS tidak dipakai.
- `client_id` disarankan: `moodin-<repo>-<peran>` (mis. `moodin-sw-inference`, `moodin-ui-kiosk`).
- Gunakan **Last Will & Testament (LWT)** pada topik `moodin/presence` untuk menandai modul yang terputus.

## 2. Ringkasan Topik

| Topik | Publisher | Subscriber | QoS | Retained |
| --- | --- | --- | --- | --- |
| `moodin/presence` | Hardware / Software | Interface, Software | 1 | Ya |
| `moodin/signal/quality` | Software | Interface | 0 | Tidak |
| `moodin/inference/result` | Software | Interface | 1 | Tidak |
| `moodin/ui/command` | Interface | Software | 1 | Tidak |
| `moodin/gesture/event` | Software | Interface | 0 untuk `pointer`, 1 untuk `select` dan `back`; `hand_lost` memakai 1 | Tidak |

Semua topik memakai prefix `moodin/`. Jangan membuat topik di luar daftar ini tanpa memperbarui berkas ini.

## 3. Skema Payload

### 3.1 `moodin/presence`

Deteksi kehadiran pengguna di depan cermin; memicu/menutup sesi. Retained agar modul yang baru connect langsung tahu status terkini.

```json
{
  "schema_version": "2.0.0",
  "session_id": "2026-07-22T19-40-00_a1b2",
  "present": true,
  "source": "presence-detector",
  "ts": 1753189200000
}
```

| Field | Tipe | Keterangan |
| --- | --- | --- |
| `present` | boolean | `true` saat pengguna terdeteksi, `false` saat pergi |
| `source` | string | asal deteksi (mis. `presence-detector`, `face-detector`) |

> Belum selaras: mesin inferensi saat ini memakai topik ini untuk status proses (`service`, `status`) beserta LWT. Pemisahan status proses dan kehadiran pengguna belum diputuskan.

### 3.2 `moodin/signal/quality`

Kualitas sinyal real-time agar UI bisa memberi umpan balik ("tetap diam", "cahaya kurang"). QoS 0 karena frekuensi tinggi dan boleh ada yang hilang.

```json
{
  "schema_version": "2.0.0",
  "session_id": "2026-07-22T19-40-00_a1b2",
  "face_detected": true,
  "landmark_confidence": 0.94,
  "rppg_snr_db": 6.8,
  "illumination_ok": true,
  "signal_ok": true,
  "ts": 1753189203000
}
```

| Field | Tipe | Keterangan |
| --- | --- | --- |
| `face_detected` | boolean | wajah terdeteksi pada frame |
| `landmark_confidence` | number [0..1] | keyakinan ekstraksi landmark |
| `rppg_snr_db` | number | SNR sinyal rPPG (dB); ambang layak mis. >= 3 dB |
| `illumination_ok` | boolean | pencahayaan memadai |
| `signal_ok` | boolean | agregat kelayakan sinyal untuk inferensi |

> Belum selaras: mesin inferensi saat ini mengirim `face_detected`, `snr_db`, `illumination`, dan `roi_stability`.

### 3.3 `moodin/inference/result`

Hasil kategori mood. Mood ditentukan dari jalur wajah saja; HR dan HRV ikut dikirim sebagai informasi dan tidak mengubah kategori mood.

```json
{
  "schema_version": "2.0.0",
  "session_id": "2026-07-22T19-40-00_a1b2",
  "mood_id": 3,
  "mood_label": "Rileks",
  "decision_margin": 0.62,
  "valence": 0.42,
  "arousal": -0.35,
  "heart_rate_bpm": 72.5,
  "hrv_ms": 48.3,
  "physio_used": true,
  "simulated": false,
  "ts": 1753189230000
}
```

| Field | Tipe | Keterangan |
| --- | --- | --- |
| `mood_id` | integer 0..4 | indeks kategori (tabel di bawah) |
| `mood_label` | string | label kategori mood |
| `decision_margin` | number [0..1] | margin keputusan: heuristik jarak titik (valensi, arousal) ke batas kategori terdekat, dikali keyakinan deteksi wajah. **Bukan peluang** dan tidak terkalibrasi; UI tidak boleh menampilkannya sebagai persen |
| `valence` | number [-1..1] | valensi dari jalur wajah |
| `arousal` | number [-1..1] | arousal dari jalur wajah saja |
| `heart_rate_bpm` | number \| null | denyut jantung estimasi rPPG |
| `hrv_ms` | number \| null | HRV (RMSSD, ms); `null` bila sesi terlalu pendek |
| `physio_used` | boolean | `true` bila HR/HRV di pesan ini berasal dari pengukuran yang masih berlaku |
| `simulated` | boolean | `true` pada mode demo/simulasi; UI wajib menampilkan penanda simulasi |

**Pemetaan kategori mood (kanonik):**

| `mood_id` | `mood_label` | Daerah valensi-arousal |
| --- | --- | --- |
| 0 | Ceria | valensi positif, arousal positif |
| 1 | Cemas | valensi negatif, arousal positif |
| 2 | Tertekan | valensi negatif, arousal negatif |
| 3 | Rileks | valensi positif, arousal negatif |
| 4 | Netral | pita persegi di sekitar pusat, mengikuti Tabel 3.3 naskah Pra-TA (bukan lingkaran radius 0,2 seperti versi 1.0.0) |

### 3.4 `moodin/ui/command`

Perintah dari antarmuka ke mesin inferensi (mis. mulai sesi, akhiri sesi).

```json
{
  "schema_version": "2.0.0",
  "session_id": "2026-07-22T19-40-00_a1b2",
  "command": "show_result",
  "params": { "screen": "result" },
  "ts": 1753189231000
}
```

| `command` | Arti | `params` contoh |
| --- | --- | --- |
| `start_session` | mulai sesi pengukuran | `{}` |
| `show_capture` | tampilkan layar akuisisi + overlay landmark | `{}` |
| `show_result` | tampilkan hasil mood | `{ "screen": "result" }` |
| `show_intervention` | tampilkan micro-intervention | `{ "type": "breathing" }` |
| `end_session` | akhiri & kembali ke idle | `{}` |

### 3.5 `moodin/gesture/event`

Peristiwa gestur tangan untuk kursor dan pemilihan di layar. Landmark tangan **tidak** dikirim; yang dikirim hanya posisi kursor dan jenis peristiwa. Gestur tidak diterbitkan selama fase pengukuran.

```json
{
  "schema_version": "2.0.0",
  "session_id": "2026-07-22T19-40-00_a1b2",
  "seq": 1042,
  "gesture": "pointer",
  "x": 0.412,
  "y": 0.637,
  "score": 0.93,
  "ts": 1753189232000
}
```

| Field | Tipe | Keterangan |
| --- | --- | --- |
| `seq` | integer >= 0 | nomor urut yang naik 1 setiap pesan dari publisher yang sama; subscriber memakainya untuk membuang pesan ganda (QoS 1 boleh terkirim ulang) dan pesan yang datang terlambat |
| `gesture` | string | `pointer`, `select`, `back`, atau `hand_lost` |
| `x`, `y` | number [0..1] \| null | posisi kursor terhadap layar, asal di kiri atas, sudah dicerminkan; `null` untuk `back` dan `hand_lost` |
| `score` | number [0..1] | keyakinan deteksi tangan |

- `pointer` paling banyak 30 pesan per detik, QoS 0.
- `select`, `back`, dan `hand_lost` memakai QoS 1.
- Tidak ada pesan retained.

## 4. Aturan & Konvensi

1. **`session_id`** dibuat oleh modul yang memicu sesi, dipakai konsisten di seluruh pesan sesi tersebut. Format anjuran: `<ISO8601-detik>_<4 hex acak>`. Sampai aturan sesi bagian 3.1 diputuskan, mesin inferensi membuat `session_id` saat mulai berjalan.
2. **`ts`** selalu epoch milidetik UTC (integer).
3. **Kompatibilitas:** perubahan yang menambah field opsional -> naikkan versi *minor*; perubahan yang menghapus/mengubah arti field -> versi *major* dan wajib koordinasi lintas repo.
4. **Timeout sesi:** bila tak ada `signal/quality` selama > 5 detik, UI menganggap sesi terputus.
5. Subscriber harus **toleran** terhadap field tak dikenal (abaikan, jangan crash).
6. Jangan menaruh data biometrik mentah (frame wajah atau tangan, landmark, sinyal PPG mentah) di MQTT. Hanya kirim fitur/hasil teragregasi (sesuai prinsip privasi & UU PDP No. 27/2022).

## 5. Contoh Uji Cepat

```bash
# subscribe semua topik MoodIn (broker lokal, tanpa TLS)
mosquitto_sub -h localhost -p 1883 -t 'moodin/#' -v

# publish contoh hasil inferensi
mosquitto_pub -h localhost -p 1883 -t moodin/inference/result \
  -m '{"schema_version":"2.0.0","session_id":"demo_0001","mood_id":3,"mood_label":"Rileks","decision_margin":0.62,"valence":0.42,"arousal":-0.35,"heart_rate_bpm":72.5,"hrv_ms":48.3,"physio_used":true,"simulated":true,"ts":1753189230000}'
```

## 6. Riwayat versi

| Versi | Tanggal | Perubahan |
| --- | --- | --- |
| 2.0.0 | 22 Nov 2026 | `confidence` diganti `decision_margin` (margin keputusan, bukan peluang); `arousal` hanya dari jalur wajah; Netral mengikuti pita Tabel 3.3; transport mengikuti ADR-10 (port 1883 tanpa TLS, port 8883 dihapus); topik baru `moodin/gesture/event`; arah `moodin/ui/command` diperbaiki (Interface -> Software); field opsional `physio_used` dan `simulated` di `inference/result`; `ts` ditegaskan sebagai integer milidetik |
| 1.0.0 | Jul 2026 | Kontrak era GEMASTIK |
