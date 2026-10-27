# MoodIn

Repo dokumen untuk Tugas Akhir **MoodIn**: sistem cermin berbasis kamera untuk memperkirakan tingkat stres dan kategori _mood_ penggunanya tanpa kontak. Repo ini tidak memuat kode yang dijalankan.

![Tahap](https://img.shields.io/badge/tahap-preliminary%20research-yellow)
![Isi](https://img.shields.io/badge/isi-dokumen%20saja-lightgrey)

> **Penulis:** Jeremia Christ Immanuel Manalu\
> **Pembimbing:** Nada Fitrieyatul Hikmah, S.T., M.T. dan Nabila Puspita Firdi, S.T., M.Si.\
> Departemen Teknik Biomedik, Institut Teknologi Sepuluh Nopember.

---

## Ringkasan

Kamera di balik cermin dua arah merekam wajah pengguna. Sistem menghasilkan dua keluaran:

| Keluaran        | Sumber informasi                                                            |
| --------------- | --------------------------------------------------------------------------- |
| Tingkat stres   | Sinyal denyut dari video wajah (rPPG) dan variabilitas denyut jantung (HRV) |
| Kategori _mood_ | Gerak _landmark_ wajah                                                      |

Prinsip yang berlaku di semua repo:

- pemrosesan berjalan di perangkat, yaitu NVIDIA Jetson Orin Nano Super;
- citra wajah tidak disimpan dan tidak dikirim ke luar perangkat; antarmodul hanya bertukar nilai numerik;
- MoodIn adalah prototipe riset, **bukan alat diagnosis medis**.

Rincian rancangan (arsitektur, parameter, target, dan protokol pengambilan data) ada di proposal TA dan belum dipublikasikan. Judul TA masih dapat berubah mengikuti bimbingan.

## Peta repositori

| Repo                                                                        | Peran pada fase TA                                                                                     | Status         |
| --------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ | -------------- |
| **MoodIn-master** (repo ini)                                                | Dokumen: peta repo, ADR, roadmap, kontrak antar-repo, arsip GEMASTIK                                   | Aktif          |
| [Software-Architecture](https://github.com/MoodInBME/Software-Architecture) | Pemrosesan sinyal dan model: jalur fisiologis dan jalur wajah                                          | Aktif (M1)     |
| [Hardware-Connections](https://github.com/MoodInBME/Hardware-Connections)   | Perangkat keras, serta integrasi dan _deployment_ di Jetson                                            | Aktif (M2)     |
| [Interface-App](https://github.com/MoodInBME/Interface-App)                 | Aplikasi Flutter: tampilan cermin dan aplikasi pendamping Android                                      | Persiapan (M3) |
| [Interface-Display](https://github.com/MoodInBME/Interface-Display)         | UI era GEMASTIK (PySide6 + QML, kiosk web)                                                             | Arsip          |
| [.github](https://github.com/MoodInBME/.github)                             | Profil organisasi dan berkas komunitas bawaan: panduan kontribusi, kebijakan keamanan, templat _issue_ | Aktif          |

Antar-repo tidak saling `import`. Modul berkomunikasi lewat MQTT di jaringan lokal sesuai kontrak pesan di [docs/ipc-contract.md](docs/ipc-contract.md).

## Milestone

| Kode | Cakupan                                 | Status             |
| ---- | --------------------------------------- | ------------------ |
| M0   | Arsip GEMASTIK dan tata kelola repo     | Berjalan           |
| M1   | Jalur fisiologis pada dataset sekunder  | Berikutnya         |
| M2   | Perangkat keras dan pengujian di Jetson | Menunggu bimbingan |
| M3   | Antarmuka Flutter                       | Belum mulai        |
| M4   | Jalur wajah                             | Menunggu bimbingan |
| M5   | Integrasi dan uji sistem                | Belum mulai        |

Rincian di [docs/roadmap.md](docs/roadmap.md).

## Dokumen

| Berkas                                       | Isi                                                               |
| -------------------------------------------- | ----------------------------------------------------------------- |
| [docs/adr.md](docs/adr.md)                   | Catatan keputusan arsitektur (ADR) beserta statusnya pada fase TA |
| [docs/roadmap.md](docs/roadmap.md)           | Milestone TA M0–M5 dan risiko yang dipantau                       |
| [docs/team.md](docs/team.md)                 | Penulis, pembimbing, dan riwayat tim GEMASTIK                     |
| [docs/ipc-contract.md](docs/ipc-contract.md) | Kontrak pesan MQTT v1.0.0 era GEMASTIK; versi TA menyusul         |
| [CHANGELOG.md](CHANGELOG.md)                 | Riwayat perubahan repo ini dan ringkasan perubahan lintas repo    |
| [docs/arsip-gemastik/](docs/arsip-gemastik/) | Proposal, slide, diagram, dan dokumen era GEMASTIK                |

## Riwayat: GEMASTIK XIX 2026

MoodIn berawal dari proyek tim VisionaryX pada GEMASTIK XIX 2026, Divisi 9 (Piranti Cerdas, Sistem Benam, dan IoT). Versi itu menghasilkan 5 kategori _mood_ dari fusi dua jalur, memakai UI PySide6 + QML, dan MQTT dengan TLS. Isi era itu dibekukan dengan tag [`gemastik-xix-2026`](https://github.com/MoodInBME/MoodIn-master/tree/gemastik-xix-2026) di setiap repo. Dokumennya ada di [docs/arsip-gemastik/](docs/arsip-gemastik/).

## Lisensi

Berkas [LICENSE](LICENSE) (Apache-2.0) ditetapkan pada era GEMASTIK. Lisensi untuk luaran TA sedang ditinjau bersama pembimbing, termasuk ketentuan HKI ITS.
