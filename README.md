# MoodIn: Smart Mirror untuk Klasifikasi Mood

> Berbasis Graph Convolutional Network + remote Photoplethysmography sebagai Personal Wellness Coach
>
> GEMASTIK XIX — Divisi 9 (Piranti Cerdas dan IoT) · Tim VisionaryX · Institut Teknologi Sepuluh Nopember

![Status](https://img.shields.io/badge/status-on--going-yellow)
![Kompetisi](https://img.shields.io/badge/GEMASTIK%20XIX-Divisi%209-red)
![Lisensi](https://img.shields.io/badge/license-Apache--2.0-brightgreen)

---

## Ringkasan

Kesehatan mental merupakan salah satu tantangan pembangunan manusia yang paling mendesak di Indonesia saat ini. Survei Kesehatan Indonesia (SKI) 2023 mencatat prevalensi depresi nasional dengan kelompok usia 15–24 tahun (Generasi Z) menunjukkan prevalensi tertinggi. Sistem kami mengatasi ini dengan menyediakan intervensi proaktif tanpa gesekan (frictionless).

## Masalah yang Diselesaikan

1. Bagaimana merancang sistem smart mirror yang mengklasifikasikan mood harian pengguna secara non-intrusive dan real-time?
2. Bagaimana merepresentasikan mood ke dalam kerangka dimensional (valence–arousal) menggunakan Graph Convolutional Network?
3. Bagaimana merancang mekanisme fusi multimodal yang menggabungkan fitur wajah (dinamika ekspresi) dan fisiologis (HRV dari rPPG)?
4. Bagaimana merancang arsitektur edge AI yang menjaga privasi biometrik wajah (UU PDP No. 27/2022)?

## Cara Kerja Singkat

![Arsitektur Sistem](docs/assets/system-architecture.png)
*(Arsitektur edge-to-cloud dengan fusi VA-Bridge)*

1. **Autentikasi & personalisasi** — aplikasi pendamping menghubungkan pengguna.
2. **Akuisisi contactless** — video wajah direkam kamera Arducam IMX477 (RGB) di balik two-way mirror.
3. **Ekstraksi fitur ganda** — ST-GCN mengestimasi valence–arousal secara paralel dengan STC-rPPG.
4. **Fusi multimodal & klasifikasi (VA-bridge)** — satu argmax kuadran menghasilkan 5 kelas mood.
5. **Personal wellness coach** — micro-intervention + visualisasi tren mood.

## Batas Tanggung Jawab 4 Repositori

| Repo | Fungsi Inti | TIDAK Memiliki |
| --- | --- | --- |
| **MoodIn-master** (repo ini) | Hub: dokumen, ADR, konvensi, kontrak IPC | Kode runtime |
| [Software-Architecture](https://github.com/MoodInBME/Software-Architecture.git) | Model DL, fusi, pipeline data, evaluasi | Kode UI, skrip hardware |
| [Hardware-Connections](https://github.com/MoodInBME/Hardware-Connections.git) | Wiring, kamera, protokol fisik, skrip bring-up | Model, UI |
| [Interface-Display](https://github.com/MoodInBME/Interface-Display.git) | UI kiosk (PySide6+QML), aplikasi pendamping | Model, inferensi |

Aturan utama: **komunikasi antar-repo hanya lewat MQTT sesuai `ipc_contract_v1.json`.** Tidak ada `import` lintas repo.

## Keputusan Arsitektural (ADR)

| ID | Keputusan | Alasan |
|---|---|---|
| ADR-01 | Edge AI = **NVIDIA Jetson Orin Nano Super 8GB**, JetPack 7.2 | 67 TOPS INT8 untuk menjalankan 2 model neural simultan 30 fps. |
| ADR-02 | Landmark iBUG-68, bukan MediaPipe 468 penuh | 68 simpul mengefisiensikan biaya adjacency graf. |
| ADR-03 | Head regresi VA, bukan klasifikasi langsung | Memungkinkan fusi dengan arousal fisiologis pada ruang circumplex. |
| ADR-04 | Late fusion, bukan early fusion | Memungkinkan degradasi anggun saat cahaya memburuk. |
| ADR-05 | Autentikasi di aplikasi, personalisasi di cermin | Cermin tetap berfungsi mode tamu tanpa akun. |
| ADR-06 | Protokol MQTT | UI butuh push (publish/subscribe); fitur Last Will and Testament (LWT). |
| ADR-07 | Validasi subject-independent wajib | Mencegah kebocoran identitas subjek pelatihan. |

## Kontrak Antar-Repo (MQTT v1.0.0)

Komunikasi menggunakan satu broker MQTT (TLS, port 8883). 
Sumber kebenaran `ipc_contract_v1.json`.

| Topik | Arah | Fungsi |
|---|---|---|
| `moodin/presence` | engine -> UI | (LWT) Status koneksi (online/offline) |
| `moodin/signal/quality` | engine -> UI | Kualitas sinyal (face_detected, SNR) |
| `moodin/inference/result` | engine -> UI | Mood_id, valence, arousal, HR, HRV |
| `moodin/ui/command` | UI -> engine | Start, stop, recalibrate, logout |

**Invarian Privasi:** Tidak ada citra mentah di topik mana pun. Hanya skalar numerik.

## Status Integrasi & Pengerjaan

| Komponen / Antarmuka | Repositori | Status |
| --- | --- | --- |
| Perakitan hardware & kamera | Hardware-Connections | _On-going_ |
| Model STC-rPPG | Software-Architecture | _Iterasi Lokal_ |
| Model ST-GCN / Graph-Tran | Software-Architecture | _Pelatihan CK+_ |
| Fusi multimodal (VA-bridge) | Software-Architecture | _Kalibrasi bobot_ |
| UI kiosk (PySide6 + QML) | Interface-Display | _Desain Figma_ |
| `stgcn_va.onnx` | Software-Architecture | _Tersedia_ |
| `mock_inference_publisher.py` | Software-Architecture | _Tersedia (UI bisa jalan)_ |

## Cara Berkontribusi

1. Baca `ipc_contract_v1.json` sebelum menulis kode integrasi.
2. Jika mengerjakan UI, gunakan `mock_inference_publisher.py`. Jangan menunggu finalisasi model DL.
3. Branch utama (`main`) selalu stabil, branch `dev` untuk iterasi mingguan, `feat/*` untuk fitur individual.
4. Sertakan penanda bahwa repo Anda adalah bagian dari "MoodIn" di README masing-masing.

## Tim

| Nama | NRP | Peran |
| --- | --- | --- |
| Jeremia Christ Immanuel Manalu | 5023231017 | Ketua, Arsitektur Perangkat Lunak |
| Tanuhita Setya Dhanurendra | 5023231026 | Anggota, Hardware & Sinyal |
| Nozomi Crystalle Padilla Malle | 5023241046 | Anggota, Antarmuka |

**Dosen Pembimbing:** Nada Fitrieyatul Hikmah, S.T., M.T.

## Referensi & Lisensi
Repo hub `MoodIn-master` dirilis di bawah **Apache License 2.0**.
- [Proposal PDF](docs/proposal/Proposal_MoodIn_VisionaryX.pdf)
- SDG 3, 4, 9 — lihat [docs/sdgs.md](docs/sdgs.md).
