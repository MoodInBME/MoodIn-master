# Changelog — MoodIn-master

Perubahan penting pada repo ini dicatat di sini. Perubahan yang berdampak ke repo lain juga diringkas di bagian **Lintas repo**.

Format mengikuti [Keep a Changelog](https://keepachangelog.com/id/1.1.0/). Repo ini berisi dokumen, jadi penanda versinya berupa tag git, bukan nomor rilis perangkat lunak.

## [Unreleased] — tahap Pra-TA

### Diubah

- `README.md` ditulis ulang sebagai pintu masuk TA: peta 6 repo, milestone M0–M5, prinsip privasi, dan riwayat GEMASTIK.
- `docs/roadmap.md` diganti dengan milestone TA M0–M5.
- `docs/team.md` memuat penulis TA, kedua pembimbing, dan riwayat tim GEMASTIK, tanpa NRP dan email.
- `docs/ipc-contract.md` diberi catatan status: kontrak v1.0.0 adalah versi era GEMASTIK; kontrak versi TA menyusul setelah bimbingan.
- `.github/workflows/link-check.yml` kini terpicu oleh push dan _pull request_ ke `main`, dan hanya memeriksa dokumen aktif.
- `README.md` ditata ulang (Fase B2): banner terang/gelap, ikon, gambaran sistem dengan diagram Mermaid, peta repositori, dan prinsip lintas repo dalam bentuk kartu. Istilah diselaraskan dengan tahap Proposal Tugas Akhir (Pra-TA).
- `docs/team.md`: peran dosen menjadi **Calon Dosen Pembimbing 1** dan **2**, gelar Nabila Puspita Firdi dikoreksi menjadi M.Sc., dan nama fakultas (FTEIC) dicantumkan.
- `docs/roadmap.md`, `docs/adr.md`, `docs/ipc-contract.md`, dan `docs/arsip-gemastik/README.md`: istilah "fase TA" diganti "tahap Pra-TA", "kontrak versi TA" diganti "kontrak pesan v2", dan status M0 diperbarui.
- `README.md` (Fase C1): prinsip lintas repo dinyatakan berlaku untuk cara kerja cermin dan isi repositori, sedangkan pengambilan data penelitian dirujuk ke halaman etika. Bagian Milestone menyebut syarat persetujuan etik, dan bagian Lisensi menegaskan bahwa lisensi tidak mencakup data partisipan.
- `docs/roadmap.md` (Fase C1): status per 29 Oktober 2026, jalur persetujuan etik, dan risiko keterlambatan persetujuan etik.
- `.gitignore` (Fase C1): pola folder data partisipan dan rekaman uji (ADR-14).

### Ditambahkan

- `docs/adr.md`: ADR-01 s.d. ADR-07 dipindah dari README beserta statusnya pada fase TA, ditambah ADR-08 s.d. ADR-13.
- `docs/arsip-gemastik/README.md` sebagai indeks arsip.
- `.gitignore` untuk data pribadi dan kredensial.
- `.github/readme/`: gambar README (banner, ikon, wajah suasana hati) beserta `KREDIT.md`.
- `docs/etika-dan-data.md` (Fase C1): etika penelitian, status persetujuan etik, dan aturan data partisipan untuk semua repo.
- `docs/adr.md` (Fase C1): ADR-14, data partisipan tidak disimpan di repositori.
- `.github/readme/ikon/etik.png` (Fase C1) untuk tabel dokumen di README.

### Dipindah

- `docs/assets/`, `docs/proposal/`, `docs/sdgs.md`, dan `moodin_system_workflow.html` ke `docs/arsip-gemastik/`.

### Dihapus

- `CONTRIBUTING.md` dan `.github/ISSUE_TEMPLATE/` (`bug_report.md`, `task.md`). Penggantinya adalah berkas komunitas bawaan organisasi di repo [.github](https://github.com/MoodInBME/.github), yang berlaku untuk semua repo.
- NRP dan email anggota dari dokumen.

### Lintas repo

- **27 Okt 2026, tata kelola (Fase A):**
  - branch default semua repo diseragamkan menjadi `main`;
  - ruleset aktif di semua repo: branch default tidak bisa dihapus atau di-_force push_, dan tag `gemastik-*` tidak bisa dihapus atau dipindah;
  - tag `gemastik-xix-2026` dibuat di 5 repo era GEMASTIK;
  - repo baru Interface-App dibuat untuk antarmuka Flutter, dan Interface-Display ditetapkan menjadi arsip.
- **Fase B, dokumentasi:** README dan dokumen semua repo diselaraskan dengan TA. Rinciannya ada di `CHANGELOG.md` setiap repo.
- **28 Okt 2026, tampilan dan istilah (Fase B2):** README semua repo ditata ulang dengan banner terang/gelap, ikon, dan narasi yang lebih deskriptif; istilah diselaraskan menjadi Proposal Tugas Akhir (Pra-TA) dan Calon Dosen Pembimbing; fakultas dicantumkan sebagai FTEIC. Gambar README disimpan di `.github/readme/` setiap repo (di repo .github: `profile/assets/`). Interface-Display dibuka sementara dari arsip untuk pembaruan README, lalu diarsipkan lagi.
- **29 Okt 2026, kesiapan etik (Fase C1):** aturan data partisipan dicatat sebagai ADR-14 dan dijelaskan di `docs/etika-dan-data.md`. Pola folder data partisipan dan rekaman uji ditambahkan ke `.gitignore` lima repo aktif. Di repo .github, profil organisasi, `CONTRIBUTING.md`, `SECURITY.md`, dan templat _issue_ serta _pull request_ menyebut data partisipan, dan templat _issue_ memakai label berawalan `jenis:`. README Software-Architecture dan Hardware-Connections ikut disesuaikan.

## [gemastik-xix-2026] — 2026-10-27

Arsip akhir era GEMASTIK XIX 2026 (tim VisionaryX). Tag menunjuk commit `04f0544`.

### Ditambahkan (Juli–Agustus 2026)

- Berkas tata kelola: `CONTRIBUTING.md`, `CHANGELOG.md`, templat _issue_, dan workflow `link-check.yml`.
- Kontrak MQTT v1.0.0 di `docs/ipc-contract.md`.
- `docs/roadmap.md`, `docs/team.md`, `docs/sdgs.md`, proposal dan slide GEMASTIK, serta diagram sistem.

### Diubah

- Perangkat edge: NVIDIA Jetson Orin Nano Super (8 GB, JetPack 7.2) menggantikan rencana awal Raspberry Pi 5.
- Kamera: Arducam IMX477 (RGB) dengan lensa M12 dan FoV kerja 55°, menggantikan asumsi kamera NoIR awal.
- Target Python diseragamkan ke 3.12.

### Dihapus

- Salinan PDF slide GEMASTIK yang duplikat (nama berspasi).

## [0.1.0] — 2026-07-13

Catatan lama tanpa tag git.

### Ditambahkan

- Organisasi GitHub `MoodInBME` dan empat repo awal: MoodIn-master, Hardware-Connections, Software-Architecture, dan Interface-Display.
- README hub, proposal dan slide GEMASTIK, serta diagram sistem.

[Unreleased]: https://github.com/MoodInBME/MoodIn-master/compare/gemastik-xix-2026...HEAD
[gemastik-xix-2026]: https://github.com/MoodInBME/MoodIn-master/tree/gemastik-xix-2026
