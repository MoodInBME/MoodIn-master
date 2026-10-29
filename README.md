<picture>
  <source media="(prefers-color-scheme: dark)" srcset=".github/readme/banner-dark.png">
  <img alt="MoodIn-master: pusat dokumen proyek MoodIn, Proposal Tugas Akhir (Pra-TA)." src=".github/readme/banner-light.png" width="100%">
</picture>

<p align="center">
  <a href="#gambaran-sistem">Gambaran sistem</a> ·
  <a href="#peta-repositori">Peta repositori</a> ·
  <a href="#prinsip-lintas-repo">Prinsip</a> ·
  <a href="#milestone">Milestone</a> ·
  <a href="#dokumen">Dokumen</a> ·
  <a href="#tim">Tim</a>
</p>

Repositori ini adalah **titik awal** untuk memahami MoodIn, cermin pintar yang memperkirakan kategori suasana hati (_mood_) dan tingkat stres penggunanya tanpa kontak. Isinya hanya dokumen: gambaran sistem, peta repositori, catatan keputusan arsitektur, roadmap, kontrak pesan antar-repo, dan arsip era GEMASTIK. Kode yang dijalankan berada di repositori lain.

MoodIn saat ini dikembangkan sebagai **Proposal Tugas Akhir (Pra-TA)** di Departemen Teknik Biomedik, Fakultas Teknologi Elektro dan Informatika Cerdas (FTEIC), Institut Teknologi Sepuluh Nopember. Rancangannya masih dapat berubah mengikuti hasil bimbingan dengan calon dosen pembimbing.

> [!NOTE]
> Rincian rancangan, yaitu arsitektur model, parameter, target, serta protokol dan instrumen pengambilan data, ada di dokumen Proposal Tugas Akhir dan **belum dipublikasikan**. Judul proposal juga masih dapat berubah.

## Gambaran sistem

Pengguna duduk di depan cermin dua arah selama pemeriksaan singkat. Kamera di balik cermin menangkap video wajah, lalu komputer _edge_ di dalam cermin, NVIDIA Jetson Orin Nano Super, mengolah video itu menjadi dua keluaran yang berjalan berdampingan:

<table>
  <tr>
    <td width="50%" valign="top">
      <img src=".github/readme/ikon/stres.png" width="40" alt=""><br>
      <b>Tingkat stres</b><br>
      Diperkirakan dari sinyal denyut yang diekstraksi dari video wajah (rPPG) dan variabilitas denyut jantung (HRV). Pembagian tingkatnya masih dibahas dalam bimbingan.
    </td>
    <td width="50%" valign="top">
      <img src=".github/readme/ikon/mood.png" width="40" alt=""><br>
      <b>Kategori <i>mood</i></b><br>
      Diperkirakan dari gerak <i>landmark</i> wajah, lalu dipetakan ke lima kategori: Ceria, Rileks, Netral, Cemas, dan Tertekan.<br>
      <img src=".github/readme/mood/mood-ceria.png" width="28" alt="Ceria">
      <img src=".github/readme/mood/mood-rileks.png" width="28" alt="Rileks">
      <img src=".github/readme/mood/mood-netral.png" width="28" alt="Netral">
      <img src=".github/readme/mood/mood-cemas.png" width="28" alt="Cemas">
      <img src=".github/readme/mood/mood-tertekan.png" width="28" alt="Tertekan">
    </td>
  </tr>
</table>

Hasilnya dikirim sebagai angka, bukan gambar, ke tampilan di balik cermin dan ke aplikasi pendamping. Diagram berikut menunjukkan repositori yang menangani setiap bagian:

```mermaid
flowchart LR
    U(["Pengguna di depan cermin"]) --> K
    subgraph J["Di dalam cermin: perangkat dari Hardware-Connections"]
        K["Kamera di balik<br/>cermin dua arah"] --> S["Pemrosesan sinyal dan model<br/>(Software-Architecture)"]
    end
    S -- "MQTT, angka saja" --> C["Tampilan cermin<br/>(Interface-App)"]
    S -- "MQTT, angka saja" --> A["Aplikasi pendamping Android<br/>(Interface-App)"]
```

Format setiap pesan MQTT ditetapkan di MoodIn-master sebagai kontrak antar-repo, sehingga pengirim dan penerima pesan dapat dikembangkan terpisah.

## Peta repositori

<table>
  <tr>
    <th width="64"></th>
    <th align="left">Repositori dan perannya</th>
    <th align="left" width="150">Status</th>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/repo-master.png" width="36" alt=""></td>
    <td><b>MoodIn-master</b> (repositori ini)<br>Dokumen: gambaran sistem, peta repositori, ADR, roadmap, kontrak pesan antar-repo, dan arsip GEMASTIK.</td>
    <td>Aktif</td>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/repo-software.png" width="36" alt=""></td>
    <td><a href="https://github.com/MoodInBME/Software-Architecture"><b>Software-Architecture</b></a><br>Pemrosesan sinyal dan model: jalur fisiologis untuk tingkat stres dan jalur wajah untuk kategori <i>mood</i>.</td>
    <td>Aktif (M1)</td>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/repo-hardware.png" width="36" alt=""></td>
    <td><a href="https://github.com/MoodInBME/Hardware-Connections"><b>Hardware-Connections</b></a><br>Perangkat keras, serta integrasi dan <i>deployment</i> sistem di Jetson.</td>
    <td>Aktif (M2)</td>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/repo-app.png" width="36" alt=""></td>
    <td><a href="https://github.com/MoodInBME/Interface-App"><b>Interface-App</b></a><br>Aplikasi Flutter: tampilan di balik cermin dan aplikasi pendamping Android.</td>
    <td>Persiapan (M3)</td>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/repo-display.png" width="36" alt=""></td>
    <td><a href="https://github.com/MoodInBME/Interface-Display"><b>Interface-Display</b></a><br>Antarmuka era GEMASTIK (PySide6 + QML dan kiosk web).</td>
    <td>Arsip</td>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/repo-github.png" width="36" alt=""></td>
    <td><a href="https://github.com/MoodInBME/.github"><b>.github</b></a><br>Profil organisasi dan berkas komunitas bawaan: panduan kontribusi, kebijakan keamanan, dan templat <i>issue</i>.</td>
    <td>Aktif</td>
  </tr>
</table>

Antar-repositori tidak saling `import`. Modul berkomunikasi lewat MQTT di jaringan lokal sesuai kontrak pesan di [docs/ipc-contract.md](docs/ipc-contract.md). Kontrak itu masih versi era GEMASTIK; kontrak pesan v2 disusun setelah bimbingan.

## Prinsip lintas repo

<table>
  <tr>
    <td width="33%" valign="top">
      <img src=".github/readme/ikon/prinsip-lokal.png" width="36" alt=""><br>
      <b>Diproses di perangkat</b><br>
      Video wajah diolah di NVIDIA Jetson Orin Nano Super di dalam cermin.
    </td>
    <td width="33%" valign="top">
      <img src=".github/readme/ikon/prinsip-tanpa-citra.png" width="36" alt=""><br>
      <b>Tanpa citra wajah</b><br>
      Citra wajah tidak disimpan dan tidak dikirim ke luar perangkat.
    </td>
    <td width="33%" valign="top">
      <img src=".github/readme/ikon/prinsip-angka-saja.png" width="36" alt=""><br>
      <b>Angka saja</b><br>
      Antarmodul hanya bertukar nilai numerik lewat MQTT.
    </td>
  </tr>
  <tr>
    <td width="33%" valign="top">
      <img src=".github/readme/ikon/prinsip-bukan-diagnosis.png" width="36" alt=""><br>
      <b>Bukan alat diagnosis</b><br>
      MoodIn adalah prototipe riset, bukan alat diagnosis medis.
    </td>
    <td width="33%" valign="top">
      <img src=".github/readme/ikon/prinsip-tanpa-dataset.png" width="36" alt=""><br>
      <b>Tanpa dataset di repo</b><br>
      Berkas dataset, data partisipan, turunannya, dan data pribadi tidak di-commit.
    </td>
    <td width="33%" valign="top">
      <img src=".github/readme/ikon/prinsip-persetujuan.png" width="36" alt=""><br>
      <b>Satu penulis commit</b><br>
      Sejak tag GEMASTIK, hanya mahasiswa pengusul yang membuat commit (ADR-13).
    </td>
  </tr>
</table>

Prinsip di atas berlaku untuk cara kerja cermin dan isi repositori. Pengambilan data penelitian dengan partisipan diatur oleh protokol yang disetujui Komite Etik Penelitian (KEP) ITS; lihat [docs/etika-dan-data.md](docs/etika-dan-data.md).

## Milestone

Kode milestone yang sama dipakai di semua repositori. Milestone mencakup tahap Pra-TA dan berlanjut ke pelaksanaan Tugas Akhir.

| Kode | Cakupan                                                 | Status                                                                                             |
| ---- | ------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| M0   | Arsip GEMASTIK, tata kelola, dan dokumentasi repositori | ![Berjalan](https://img.shields.io/badge/-berjalan-48C2FF?style=flat-square)                       |
| M1   | Jalur fisiologis pada dataset sekunder                  | ![Berikutnya](https://img.shields.io/badge/-berikutnya-7A5AF8?style=flat-square)                   |
| M2   | Perangkat keras dan pengujian di Jetson                 | ![Menunggu bimbingan](https://img.shields.io/badge/-menunggu%20bimbingan-F29A0B?style=flat-square) |
| M3   | Antarmuka Flutter                                       | ![Belum mulai](https://img.shields.io/badge/-belum%20mulai-6B7A8C?style=flat-square)               |
| M4   | Jalur wajah                                             | ![Menunggu bimbingan](https://img.shields.io/badge/-menunggu%20bimbingan-F29A0B?style=flat-square) |
| M5   | Integrasi dan uji sistem                                | ![Belum mulai](https://img.shields.io/badge/-belum%20mulai-6B7A8C?style=flat-square)               |

Pengambilan data primer, bagian dari M5, baru dimulai setelah persetujuan etik dari KEP ITS terbit. Rincian setiap milestone dan risiko yang dipantau ada di [docs/roadmap.md](docs/roadmap.md).

## Dokumen

<table>
  <tr>
    <td align="center" width="64"><img src=".github/readme/ikon/peta.png" width="36" alt=""></td>
    <td><a href="docs/adr.md"><b>docs/adr.md</b></a><br>Catatan keputusan arsitektur (ADR) beserta statusnya pada tahap Pra-TA.</td>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/milestone.png" width="36" alt=""></td>
    <td><a href="docs/roadmap.md"><b>docs/roadmap.md</b></a><br>Milestone M0–M5 dan risiko yang dipantau.</td>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/etik.png" width="36" alt=""></td>
    <td><a href="docs/etika-dan-data.md"><b>docs/etika-dan-data.md</b></a><br>Etika penelitian, status persetujuan etik, dan aturan data partisipan.</td>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/tim.png" width="36" alt=""></td>
    <td><a href="docs/team.md"><b>docs/team.md</b></a><br>Mahasiswa pengusul, calon dosen pembimbing, dan riwayat tim GEMASTIK.</td>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/prinsip-angka-saja.png" width="36" alt=""></td>
    <td><a href="docs/ipc-contract.md"><b>docs/ipc-contract.md</b></a><br>Kontrak pesan MQTT v1.0.0 era GEMASTIK; kontrak pesan v2 menyusul.</td>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/dokumen.png" width="36" alt=""></td>
    <td><a href="CHANGELOG.md"><b>CHANGELOG.md</b></a><br>Riwayat perubahan repositori ini dan ringkasan perubahan lintas repositori.</td>
  </tr>
  <tr>
    <td align="center"><img src=".github/readme/ikon/riwayat.png" width="36" alt=""></td>
    <td><a href="docs/arsip-gemastik/"><b>docs/arsip-gemastik/</b></a><br>Proposal, slide, diagram, dan dokumen era GEMASTIK.</td>
  </tr>
</table>

## Tim

Proyek ini dikerjakan oleh **Jeremia Christ Immanuel Manalu** sebagai mahasiswa pengusul Pra-TA, dengan **Nada Fitrieyatul Hikmah, S.T., M.T.** sebagai Calon Dosen Pembimbing 1 dan **Nabila Puspita Firdi, S.T., M.Sc.** sebagai Calon Dosen Pembimbing 2. Dosen pembimbing Tugas Akhir belum ditetapkan secara resmi selama tahap Pra-TA. Rincian peran dan riwayat tim ada di [docs/team.md](docs/team.md).

## Riwayat: GEMASTIK XIX 2026

MoodIn berawal dari proyek tim VisionaryX pada GEMASTIK XIX 2026, Divisi 9 (Piranti Cerdas, Sistem Benam, dan IoT). Versi itu menghasilkan 5 kategori _mood_ dari fusi dua jalur, memakai UI PySide6 + QML, dan MQTT dengan TLS. Isi era itu dibekukan dengan tag [`gemastik-xix-2026`](https://github.com/MoodInBME/MoodIn-master/tree/gemastik-xix-2026) di setiap repositori, dan dokumennya disimpan di [docs/arsip-gemastik/](docs/arsip-gemastik/).

## Lisensi

Berkas [LICENSE](LICENSE) (Apache-2.0) ditetapkan pada era GEMASTIK dan tetap berlaku untuk repositori ini. Lisensi untuk hasil pengembangan berikutnya sedang ditinjau bersama calon dosen pembimbing, termasuk ketentuan HKI ITS. Lisensi hanya berlaku untuk kode dan dokumen di repositori. Data partisipan penelitian tidak termasuk; pemakaiannya hanya mengikuti persetujuan partisipan dan protokol yang disetujui KEP ITS. Dataset publik yang dipakai tetap tunduk pada lisensi atau EULA masing-masing.

---

<div align="center">
<sub>
Departemen Teknik Biomedik · Fakultas Teknologi Elektro dan Informatika Cerdas (FTEIC)<br>
Institut Teknologi Sepuluh Nopember · Surabaya, Indonesia
</sub>
<br><br>
<sub>Ikon antarmuka: <a href="https://tabler.io/icons">Tabler Icons</a> (MIT), lihat <a href=".github/readme/KREDIT.md">kredit aset</a>. Logo, maskot, dan ikon suasana hati: desain tim MoodIn.</sub>
</div>
