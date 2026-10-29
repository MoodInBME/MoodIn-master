# IPC Contract — Skema MQTT Lintas-Repo (KANONIK)

> **Status pada tahap Pra-TA (28 Okt 2026):** berkas ini adalah kontrak **v1.0.0 era GEMASTIK** dan belum mengikuti rancangan proposal. Yang sudah diputuskan: broker MQTT di jaringan lokal **tanpa TLS** (port 8883 tidak dipakai), setiap klien memakai _username/password_, dan isi pesan dienkripsi (ADR-10 di [adr.md](adr.md)). Keluaran sistem juga mencakup tingkat stres. Kontrak pesan v2 akan menggantikan berkas ini setelah bimbingan; sampai saat itu, jangan membuat implementasi baru berdasarkan berkas ini.
>
> Kekeliruan yang diketahui: arah `moodin/ui/command` di tabel bagian 2 terbalik. Yang benar, antarmuka (UI) menerbitkan perintah dan mesin inferensi berlangganan.

> **Sumber kebenaran tunggal.** Berkas ini mendefinisikan seluruh nama topik dan skema payload komunikasi antar-modul MoodIn. Repo lain **hanya mereferensikan** berkas ini, tidak mendefinisikan ulang. Setiap perubahan kontrak: update berkas ini + naikkan `schema_version`, baru ubah implementasi.

- **Versi kontrak:** `1.0.0`
- **Field wajib di setiap payload:** `schema_version` (string), `session_id` (string), `ts` (epoch milidetik, integer).
- **Encoding:** JSON UTF-8.

## 1. Broker

| Lingkungan | Host | Port | TLS | Auth |
| --- | --- | --- | --- | --- |
| Dev (LAN/lokal) | broker lokal | `1883` | Tidak | opsional (username/password) |
| Produksi | broker MoodIn | `8883` | Ya (TLS) | username/password + CA |

- `client_id` disarankan: `moodin-<repo>-<peran>` (mis. `moodin-sw-inference`, `moodin-ui-kiosk`).
- Gunakan **Last Will & Testament (LWT)** pada topik `moodin/presence` untuk menandai modul yang terputus.

## 2. Ringkasan Topik

| Topik | Publisher | Subscriber | QoS | Retained |
| --- | --- | --- | --- | --- |
| `moodin/presence` | Hardware / Software | Interface, Software | 1 | Ya |
| `moodin/signal/quality` | Software | Interface | 0 | Tidak |
| `moodin/inference/result` | Software | Interface | 1 | Tidak |
| `moodin/ui/command` | Software / sistem | Interface | 1 | Tidak |

Semua topik memakai prefix `moodin/`. Jangan membuat topik di luar daftar ini tanpa memperbarui berkas ini.

## 3. Skema Payload

### 3.1 `moodin/presence`

Deteksi kehadiran pengguna di depan cermin; memicu/menutup sesi. Retained agar modul yang baru connect langsung tahu status terkini.

```json
{
  "schema_version": "1.0.0",
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

### 3.2 `moodin/signal/quality`

Kualitas sinyal real-time agar UI bisa memberi umpan balik ("tetap diam", "cahaya kurang"). QoS 0 karena frekuensi tinggi dan boleh ada yang hilang.

```json
{
  "schema_version": "1.0.0",
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

### 3.3 `moodin/inference/result`

Hasil klasifikasi mood akhir (keluaran VA-bridge). Ini payload inti yang ditampilkan UI dan disimpan untuk tren.

```json
{
  "schema_version": "1.0.0",
  "session_id": "2026-07-22T19-40-00_a1b2",
  "mood_id": 3,
  "mood_label": "Rileks",
  "confidence": 0.81,
  "valence": 0.42,
  "arousal": -0.15,
  "heart_rate_bpm": 72.5,
  "hrv_ms": 48.3,
  "ts": 1753189230000
}
```

| Field | Tipe | Keterangan |
| --- | --- | --- |
| `mood_id` | integer 0..4 | indeks kelas (lihat tabel di bawah) |
| `mood_label` | string | label kelas mood |
| `confidence` | number [0..1] | keyakinan klasifikasi |
| `valence` | number [-1..1] | dimensi valence akhir (dari ekspresi) |
| `arousal` | number [-1..1] | dimensi arousal akhir (fusi ekspresi + fisiologis) |
| `heart_rate_bpm` | number \| null | detak jantung estimasi rPPG |
| `hrv_ms` | number \| null | HRV (RMSSD, ms); `null` bila sesi terlalu pendek |

**Pemetaan kelas mood (kanonik):**

| `mood_id` | `mood_label` | Kuadran valence-arousal (indikatif) |
| --- | --- | --- |
| 0 | Ceria | valence+, arousal+ |
| 1 | Cemas | valence-, arousal+ |
| 2 | Tertekan | valence-, arousal- |
| 3 | Rileks | valence+, arousal- |
| 4 | Netral | dekat pusat (radius netral 0.2) |

### 3.4 `moodin/ui/command`

Perintah dari logika inti ke UI kiosk (mis. pindah layar, tampilkan intervensi).

```json
{
  "schema_version": "1.0.0",
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

## 4. Aturan & Konvensi

1. **`session_id`** dibuat oleh modul yang memicu sesi (dari `presence`), dipakai konsisten di seluruh pesan sesi tersebut. Format anjuran: `<ISO8601-detik>_<4 hex acak>`.
2. **`ts`** selalu epoch milidetik UTC.
3. **Kompatibilitas:** perubahan yang menambah field opsional -> naikkan versi *minor*; perubahan yang menghapus/mengubah arti field -> versi *major* dan wajib koordinasi lintas repo.
4. **Timeout sesi:** bila tak ada `signal/quality` selama > 5 detik, UI menganggap sesi terputus.
5. Subscriber harus **toleran** terhadap field tak dikenal (abaikan, jangan crash).
6. Jangan menaruh data biometrik mentah (frame wajah, sinyal PPG mentah) di MQTT. Hanya kirim fitur/hasil teragregasi (sesuai prinsip privasi & UU PDP No. 27/2022).

## 5. Contoh Uji Cepat

```bash
# subscribe semua topik MoodIn (dev, tanpa TLS)
mosquitto_sub -h localhost -p 1883 -t 'moodin/#' -v

# publish contoh hasil inferensi
mosquitto_pub -h localhost -p 1883 -t moodin/inference/result \
  -m '{"schema_version":"1.0.0","session_id":"demo_0001","mood_id":3,"mood_label":"Rileks","confidence":0.81,"valence":0.42,"arousal":-0.15,"heart_rate_bpm":72.5,"hrv_ms":48.3,"ts":1753189230000}'
```
