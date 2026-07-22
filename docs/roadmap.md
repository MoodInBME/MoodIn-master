# Roadmap — MoodIn

Status per **22 Juli 2026**. Roadmap ini mengikat progres ke tahapan GEMASTIK XIX dan tabel Bab 3.3 proposal. Perbarui saat sinkronisasi mingguan.

Legenda: [x] selesai · [~] berjalan · [ ] belum mulai

## Status Ringkas

- **Board edge:** Raspberry Pi 5 (16 GB, Debian Trixie) = **fix** untuk Cross-Eval 2. Jetson Orin Nano Super = **rencana peningkatan** setelah pendampingan teknis tersedia.
- **Kamera:** Arducam IMX477 (RGB), base M12, FoV kerja 65 deg = **fix**.
- **Python:** 3.12 (seragam lintas repo) = **fix**.
- **Dataset ekspresi:** Aff-Wild2 = target utama (menunggu persetujuan EULA); CK+/Oulu-CASIA = baseline sementara.
- **Kontrak MQTT:** v1.0.0 terdefinisi di `docs/ipc-contract.md`.

## Fase 1 — Fondasi & Proposal (selesai)

- [x] Riset masalah, rumusan masalah, dan proposal GEMASTIK
- [x] Inisialisasi organisasi `MoodInBME` + 4 repo
- [x] Arsitektur sistem 6 tahap + diagram
- [x] Penetapan stack: ST-GCN/Graph-Transformer + STC-rPPG + fusi VA-bridge

## Fase 2 — Menuju Cross-Eval 2 (berjalan)

### Hardware
- [x] Finalisasi kamera Arducam IMX477 + perhitungan FoV (65 deg)
- [x] Penetapan Raspberry Pi 5 sebagai board edge utama CE2
- [~] Perakitan enclosure + two-way mirror + integrasi Waveshare 10.1"
- [~] Uji akuisisi & kualitas sinyal di balik cermin

### Software
- [x] Pipeline preprocessing (CLAHE) + ekstraksi 468->68 landmark
- [x] Komponen model: FaceGraph, ST-GCN block, Temporal Transformer, kepala VA
- [~] Training model ekspresi (baseline CK+/Oulu; alih ke Aff-Wild2 setelah EULA)
- [~] STC-rPPG (estimasi HR/HRV)
- [~] Fusi multimodal VA-bridge + klasifikasi 5 kelas
- [ ] Deployment edge Raspberry Pi 5 (ONNX Runtime) end-to-end

### Interface
- [~] UI kiosk PySide6 + QML (alur idle -> capture -> result -> intervention)
- [ ] Overlay landmark real-time pada layar akuisisi
- [ ] Visualisasi tren mood 7 hari

### Integrasi
- [x] Kontrak MQTT kanonik v1.0.0
- [ ] Integrasi end-to-end lintas repo via MQTT (dev broker :1883)
- [ ] Web demo (webcam laptop) untuk peragaan juri/dosen

## Fase 3 — Peningkatan & Final (rencana)

- [ ] Migrasi board ke Jetson Orin Nano Super (JetPack 6.2.1, NVMe) + TensorRT/INT8
- [ ] Kalibrasi bobot fusi (w1, w2) pada dataset fisiologis (MAHNOB-HCI/UBFC-Phys)
- [ ] Evaluasi menyeluruh + validasi lapangan
- [ ] Materi final: demo produk, narasi komersialisasi (model B2B), disclaimer non-medis

## Risiko yang Dipantau

| Risiko | Mitigasi |
| --- | --- |
| EULA Aff-Wild2 lama disetujui | jalan paralel dengan baseline CK+/Oulu |
| Beban komputasi real-time di Pi 5 (>= 30 fps) | optimasi ONNX; Jetson sebagai peningkatan |
| HRV pada 30 fps kurang presisi | fokus RMSSD, sesi >= 30 dtk, komunikasikan keterbatasan |
| Overreach klaim klinis | disclaimer "bukan alat medis" di UI & materi |
