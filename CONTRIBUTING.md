# Panduan Kontribusi — MoodIn

Terima kasih sudah ikut membangun **MoodIn** (Tim VisionaryX, GEMASTIK XIX Divisi 9). Dokumen ini adalah aturan kerja **lintas-repo** untuk keempat repositori organisasi `MoodInBME`. Baca sekali, patuhi di semua repo.

> Repo ini (`MoodIn-master`) adalah **hub**. Kontrak bersama (mis. skema MQTT) didefinisikan di sini sebagai sumber kebenaran. Jangan menduplikasi definisi kanonik di repo lain.

---

## 1. Struktur 4 Repo

| Repo | Ruang lingkup kontribusi |
| --- | --- |
| `MoodIn-master` | Dokumentasi, proposal, aset, kontrak lintas-repo, roadmap |
| `Hardware-Connections` | Wiring, enclosure, kamera, daya, protokol fisik |
| `Software-Architecture` | Model DL, fusi, training, evaluasi, deployment edge |
| `Interface-Display` | UI kiosk (PySide6 + QML) |

Kontribusikan perubahan ke repo yang sesuai lingkupnya. Perubahan yang menyentuh kontrak antar-modul **harus** diawali update `docs/ipc-contract.md` di repo ini.

## 2. Alur Branching

```text
main            -> selalu stabil; ini yang dilihat juri. Jangan push langsung.
dev             -> integrasi kerja mingguan
feat/nama-fitur -> branch kerja individual; merge ke dev via Pull Request
fix/nama-bug    -> perbaikan bug
docs/nama-dok   -> perubahan dokumentasi
```

Aturan:

1. Selalu bercabang dari `dev` (kecuali hotfix kritis dari `main`).
2. Satu branch = satu fitur/bug yang jelas. Hindari branch "campur aduk".
3. `main` di-merge dari `dev` hanya saat rilis/checkpoint (mis. sebelum Cross-Eval).

## 3. Konvensi Commit (Conventional Commits)

```text
feat: tambah modul ekstraksi landmark MediaPipe
fix: perbaiki bug dimensi output STC-rPPG
docs: update BOM dengan harga terbaru
test: tambah unit test preprocessing CLAHE
chore: rapikan struktur folder deployment
refactor: pisahkan graph builder dari model
```

Format: `<tipe>: <ringkasan singkat, huruf kecil, tanpa titik>`. Tipe yang dipakai: `feat`, `fix`, `docs`, `test`, `chore`, `refactor`, `perf`.

## 4. Alur Pull Request

1. Push branch, buka PR ke `dev` (bukan `main`).
2. Judul PR memakai gaya Conventional Commit yang sama.
3. Isi deskripsi: **apa** yang diubah, **kenapa**, dan **cara uji**.
4. Kaitkan issue terkait dengan `Closes #<nomor>`.
5. Minimal 1 review anggota tim sebelum merge. Merge memakai **Squash and merge** agar histori `dev` rapi.
6. Pastikan CI (`link-check`) hijau sebelum merge.

## 5. Standar Kode

- **Python 3.12** sebagai target seragam lintas repo (hindari 3.8/3.9 dan 3.14; beberapa dependency belum stabil di 3.14).
- Formatter: `black`; linter: `ruff`. Jalankan sebelum commit.
- Dependency dikunci di `requirements.txt` (dan `requirements-jetson.txt` untuk target Jetson bila sudah relevan).
- Jangan commit berkas besar (dataset, bobot model, video). Gunakan `.gitignore`; untuk artefak besar pertimbangkan Git LFS/DVC.
- Jangan pernah commit rahasia (token, kunci API, kredensi broker). Bila terlanjur, **rotasi** token tersebut dan bersihkan histori.

## 6. Dokumentasi & Cross-Linking

- Setiap README repo teknis wajib memuat baris **"Bagian dari proyek MoodIn"** + tautan ke `MoodIn-master`.
- Perubahan yang memengaruhi kontrak antar-modul: update `docs/ipc-contract.md` **lebih dulu**, lalu implementasi di repo terkait.
- Perubahan penting dicatat di `CHANGELOG.md`.

## 7. Issue

Gunakan template yang tersedia di `.github/ISSUE_TEMPLATE/`:

- **Bug report** untuk cacat/perilaku salah.
- **Task** untuk pekerjaan terencana (fitur, dokumentasi, riset).

## 8. Lisensi Kontribusi

Dengan berkontribusi, Anda setuju kontribusi Anda dirilis di bawah lisensi yang tercantum pada berkas `LICENSE` repo terkait. Catatan: `Interface-Display` memuat PySide6 (LGPL-3.0), sehingga repo tersebut praktis mengikuti LGPL-3.0.
