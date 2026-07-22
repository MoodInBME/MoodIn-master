# Changelog

Semua perubahan penting pada proyek **MoodIn** (repo hub `MoodIn-master`) dicatat di berkas ini.

Format mengikuti [Keep a Changelog](https://keepachangelog.com/id/1.1.0/), dan proyek ini memakai [Semantic Versioning](https://semver.org/lang/id/).

> Cakupan berkas ini: dokumentasi, aset, dan kontrak lintas-repo di `MoodIn-master`. Perubahan kode teknis dicatat di `CHANGELOG.md` masing-masing repo (bila ada).

## [Unreleased]

### Added
- Berkas tata kelola repo: `CONTRIBUTING.md`, `CHANGELOG.md`, template issue (`bug_report.md`, `task.md`), dan workflow `link-check.yml`.
- Dokumen kanonik `docs/ipc-contract.md` (skema topik & payload MQTT lintas-repo).
- Dokumen `docs/roadmap.md`, `docs/team.md`, `docs/sdgs.md`.

### Changed
- **Keputusan hardware edge**: Raspberry Pi 5 ditetapkan sebagai board edge **utama** untuk Cross-Eval 2. Migrasi ke Jetson Orin Nano Super diposisikan sebagai **rencana peningkatan** (setelah pendampingan teknis tersedia). README keempat repo diselaraskan dengan keputusan ini.
- Kamera difinalkan ke **Arducam IMX477 (RGB)** dengan base M12, FoV kerja **65 deg** (menggantikan asumsi NoIR awal).
- Target Python diseragamkan ke **3.12** lintas repo.

### Pending
- Akses dataset **Aff-Wild2**: permohonan EULA telah dikirim ke pengelola (QMUL), menunggu balasan. Baseline sementara memakai CK+/Oulu-CASIA.
- Finalisasi berkas `LICENSE` sebelum submission.

## [0.1.0] - 2026-07-13

### Added
- Inisialisasi organisasi GitHub `MoodInBME` dan empat repositori: `MoodIn-master`, `Hardware-Connections`, `Software-Architecture`, `Interface-Display`.
- README hub `MoodIn-master` (latar belakang, rumusan masalah, alur kerja 6 tahap, struktur repo, tim, SDGs).
- Proposal & slide GEMASTIK dimasukkan ke `docs/proposal/`; diagram sistem ke `docs/assets/`.

[Unreleased]: https://github.com/MoodInBME/MoodIn-master/compare/v0.1.0...HEAD
[0.1.0]: https://github.com/MoodInBME/MoodIn-master/releases/tag/v0.1.0
