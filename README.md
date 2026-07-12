
# MoodIn: Smart Mirror untuk Klasifikasi Mood

> GCN + remote Photoplethysmography sebagai Personal Wellness Coach
> GEMASTIK XIX — Divisi 9 (Piranti Cerdas dan IoT)
> Tim VisionaryX — Institut Teknologi Sepuluh Nopember

![https://img.shields.io/badge/github-repo-blue?logo=github](https://img.shields.io/badge/github-repo-blue?logo=github)
![https://img.shields.io/badge/On--going-yellow](https://img.shields.io/badge/On--going-yellow)
![https://img.shields.io/badge/Competition-red](https://img.shields.io/badge/Competition-red)
![https://img.shields.io/badge/License-brightgreen](https://img.shields.io/badge/License-brightgreen)

[Badge: Status] [Badge: Kompetisi] [Badge: Lisensi]

## Ringkasan

Kesehatan mental merupakan salah satu tantangan pembangunan manusia yang paling mendesak di Indonesia saat ini. Survei Kesehatan Indonesia (SKI) 2023 yang dilaksanakan Kementerian Kesehatan RI terhadap sekitar 345 ribu rumah tangga di 38 provinsi mencatat prevalensi depresi nasional sebesar 1,4%, dengan kelompok usia 15–24 tahun (Generasi Z) menunjukkan prevalensi tertinggi dibanding kelompok usia lain, yaitu 2% [1]. Yang lebih mengkhawatirkan, pada kelompok anak muda pengidap depresi ini, survei yang sama mencatat bahwa 61% di antaranya pernah berpikir untuk mengakhiri hidup dalam satu bulan terakhir saat survei dilakukan, namun hanya 10,4% dari kelompok berisiko tinggi ini yang benar-benar mencari pengobatan formal [1], [2].

Riskesdas 2018 memakai SRQ-20 atau self-report screening umum, sedangkan SKI 2023 memakai MINI yang merupakan wawancara terstruktur klinis untuk depresi spesifik. Studi re-analisis oleh Zhafirah & Hastono (Universitas Indonesia) bahkan menunjukkan bahwa saat SKI 2023 dianalisis ulang memakai instrumen SRQ-20 yang sama dengan Riskesdas 2018, prevalensi masalah kesehatan jiwa turun menjadi 2,0%, jauh dari 9,8%, bukti bahwa perubahan metodologis, bukan semata perbaikan kondisi riil, banyak berkontribusi pada disparitas angka ini [3].

Terlepas dari perbedaan angka absolut antarsurvei, satu temuan konsisten pada keduanya adalah: kelompok usia produktif-muda menanggung beban gangguan mood tertinggi namun memiliki tingkat pencarian bantuan (help-seeking) yang paling rendah. Kesenjangan ini berbanding terbalik dengan meningkatnya keterbukaan wacana kesehatan mental di ruang digital. Data internal Google Indonesia yang dipublikasikan awal 2021 mencatat kenaikan volume pencarian kata kunci terkait kesehatan mental sebesar 70% dan self-care sebesar 45% pada masa pandemi dibandingkan periode sebelumnya [4], dan studi akademik lain menunjukkan tren pencarian daring terkait gejala gangguan jiwa berkorelasi dengan kondisi psikologis populasi secara agregat, menjadikan minat pemantauan mandiri ini sebagai sinyal yang terukur, bukan sekadar anekdot [5]. Meningkatnya keterbukaan ini sayangnya belum diikuti oleh akses layanan yang memadai, jumlah psikolog klinis yang terbatas, stigma sosial, dan biaya terapi tetap menjadi penghalang struktural utama.


## Masalah yang Diselesaikan

1.	Bagaimana merancang sistem smart mirror yang mampu mengklasifikasikan mood harian pengguna secara non-intrusive dan real-time, dengan menggabungkan analisis ekspresi wajah dan sinyal fisiologis tanpa kontak?
2.	Bagaimana merepresentasikan mood ke dalam kerangka dimensional (valence–arousal) menggunakan Graph Convolutional Network, sehingga keluaran klasifikasi lebih kaya informasi dan lebih tahan terhadap keterbatasan model kategori emosi diskrit yang kaku?
3.	Bagaimana merancang mekanisme fusi multimodal yang menggabungkan fitur geometris wajah (Action Units) dan fitur fisiologis (HRV/BRV dari rPPG) ke dalam satu ruang representasi mood yang konsisten?
4.	Bagaimana merancang arsitektur IoT (edge-to-cloud) yang memungkinkan pelacakan tren mood mingguan dan pemberian rekomendasi intervensi personal, tanpa mengorbankan privasi data biometrik wajah pengguna?


## Cara Kerja Singkat

![Arsitektur Sistem](docs/assets/system-architecture.png)
(Ringkas alur 6 tahap dari Gambar 1 dalam poin-poin)

## Fitur Utama

1. Autentikasi & personalisasi
2. Akuisisi & pra-pemrosesan contactless
3. Ekstraksi fitur ganda (ST-GCN + STC-rPPG)
4. Fusi multimodal & klasifikasi 5-kelas
5. Personal wellness coach (micro-intervention + tren 7 hari)

## Struktur Proyek

| Repo                                                | Fungsi                                         |
| --------------------------------------------------- | ---------------------------------------------- |
| [Hardware-Connections](../../Hardware-Connections)   | Konfigurasi fisik, wiring, protokol komunikasi |
| [Software-Architecture](../../Software-Architecture) | Model deep learning, training, evaluasi        |

## Struktur Repository Ini
``` bash
MoodIn-master/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── .github/
│   ├── ISSUE_TEMPLATE/
│   │   ├── bug_report.md
│   │   └── task.md
│   └── workflows/
│       └── link-check.yml        # optional: cek link mati di README
├── docs/
│   ├── proposal/
│   │   ├── Proposal_MoodIn_VisionaryX.pdf
│   │   └── PPT_Gemastik_MoodIn_VisionaryX.pdf
│   ├── assets/
│   │   ├── system-architecture.png     # Gambar 1
│   │   ├── circumplex-model.png        # Gambar 2
│   │   ├── hardware-3d-design.png      # Gambar 3
│   │   └── workflow-diagram.png        # Gambar 4
│   ├── roadmap.md
│   ├── team.md
│   └── sdgs.md
└── CHANGELOG.md
```
## Status Pengerjaan

(Tarik dari tabel Bab 3.3 — persentase progres per komponen)

## Tim

| Nama                           | NRP        | Peran   |
| ------------------------------ | ---------- | -----   |
| Jeremia Christ Immanuel Manalu | 5023231017 | Ketua   |
| Tanuhita Setya Dhanurendra     | 5023231026 | Anggota |
| Nozomi Crystalle Padilla Malle | 5023241046 | Anggota |

**Dosen Pembimbing:** Nada Fitrieyatul Hikmah, S.T., M.T.

## Dampak SDGs

SDG 3, 4, 9 — lihat [docs/sdgs.md](docs/sdgs.md)

## Konvensi Kerja Lintas Repo
1. Branching Sederhana
```text
main          → selalu stabil, ini yang dilihat juri
dev           → integrasi kerja mingguan
feat/nama-fitur → branch kerja individual, merge ke dev via PR
```
2. Konvensi Commit Message
```text
feat: tambah modul ekstraksi landmark MediaPipe
fix: perbaiki bug dimensi output STC-rPPG
docs: update BOM dengan harga terbaru
test: tambah unit test preprocessing CLAHE
```
3. Cross-Linking wajib, di setiap README repo teknis, selalu ada baris "Bagian dari proyek MoodIn" supaya siapa pun yang nyasar ke satu repo tetap tahu konteks besarnya.

## Dokumen Lengkap

- [Proposal PDF](docs/proposal/Proposal_MoodIn_VisionaryX.pdf)
- [Slide Presentasi](docs/proposal/PPT_Gemastik_MoodIn_VisionaryX.pdf)

## Referensi
[1]	Kementerian Kesehatan RI, Badan Kebijakan Pembangunan Kesehatan, "Depresi pada Anak Muda di Indonesia," Factsheet Survei Kesehatan Indonesia (SKI) 2023. [Online]. Tersedia: https://repository.badankebijakan.kemkes.go.id/id/eprint/5532/1/03%20factsheet%20Keswa_bahasa.pdf.

[2]	Badan Kebijakan Pembangunan Kesehatan (BKPK) Kemenkes RI, "Potret Sehat Indonesia dari Kacamata SKI 2023," 2024. [Online]. Tersedia: https://www.badankebijakan.kemkes.go.id/potret-sehat-indonesia-dari-kacamata-ski-2023/.

[3]	A. Z. Zhafirah dan S. P. Hastono, "Analisis Disparitas Geospasial dan Determinan Sosio-Demografis Masalah Kesehatan Jiwa di Indonesia: Analisis Data Survei Kesehatan Indonesia (SKI) 2023," ScholarHub UI, BIKFOKES, vol. 6, no. 2, 2025. [Online]. Tersedia: https://scholarhub.ui.ac.id/bikfokes/vol6/iss2/1/. 

[4]	ANTARA News, "Pencarian Google Indonesia soal kesehatan meningkat selama pandemi," 23 Feb. 2021. [Online]. Tersedia: https://www.antaranews.com/berita/2014332/pencarian-google-indonesia-soal-kesehatan-meningkat-selama-pandemi.

[5]	U. K. Hisan, M. M. Amri, D. Sulisworo, I. Rahmawati, T. A. Pramesti, "Google Trends Potential for Detecting a Large-scale Mental Health Issue: A Study during Pandemic in Indonesia," Jurnal Aisyah: Jurnal Ilmu Kesehatan, 2023.