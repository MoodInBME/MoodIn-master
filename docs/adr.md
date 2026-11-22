# Catatan Keputusan Arsitektur (ADR)

Status: **Diterima** = berlaku · **Diubah** = berlaku dengan perubahan · **Ditinjau** = menunggu keputusan bimbingan · **Diusulkan** = draf usulan, belum diputuskan.

ADR-01 s.d. ADR-07 dibuat pada era GEMASTIK dan sebelumnya ada di README repo ini. ADR-08 dan seterusnya dibuat pada tahap Proposal Tugas Akhir (Pra-TA). Keputusan metode yang belum boleh dipublikasikan tidak dicatat di sini.

## Ringkasan

| ID     | Keputusan                                                                      | Status Pra-TA      |
| ------ | ------------------------------------------------------------------------------ | ------------------ |
| ADR-01 | Perangkat edge: NVIDIA Jetson Orin Nano Super 8 GB, JetPack 7.2                | Diterima           |
| ADR-02 | 68 _landmark_ wajah, bukan 468 titik MediaPipe penuh                           | Diubah             |
| ADR-03 | Kepala regresi valensi-_arousal_, bukan klasifikasi langsung                   | Ditinjau           |
| ADR-04 | _Late fusion_ dua jalur menjadi satu keluaran _mood_                           | Ditinjau           |
| ADR-05 | Autentikasi di aplikasi pendamping, personalisasi di cermin                    | Ditinjau           |
| ADR-06 | Komunikasi antarmodul lewat MQTT                                               | Diubah oleh ADR-10 |
| ADR-07 | Validasi _subject-independent_ wajib                                           | Diterima           |
| ADR-08 | Arsip GEMASTIK lewat tag; Pra-TA lanjut di repo yang sama                      | Diterima           |
| ADR-09 | Semua repo publik; branch default dan tag arsip dilindungi ruleset             | Diterima           |
| ADR-10 | MQTT di jaringan lokal tanpa TLS, dengan autentikasi broker dan enkripsi pesan | Diterima           |
| ADR-11 | Antarmuka baru dengan Flutter di repo baru Interface-App                       | Diterima           |
| ADR-12 | MoodIn-master hanya dokumen; integrasi Jetson di Hardware-Connections          | Diterima           |
| ADR-13 | Hanya mahasiswa pengusul yang membuat commit; riwayat GEMASTIK tetap utuh      | Diterima           |
| ADR-14 | Data partisipan tidak disimpan di repo; dikelola sesuai protokol KEP ITS       | Diterima           |
| ADR-15 | Kamera UVC (Logitech C920) sebagai kamera utama                                | Diusulkan          |
| ADR-16 | Interaksi gestur tangan                                                        | Diusulkan          |

## Rincian

### ADR-01 — Jetson Orin Nano Super sebagai perangkat edge

- **Konteks (GEMASTIK):** dua model saraf harus berjalan bersamaan secara waktu nyata, dan Raspberry Pi 5 tidak memadai untuk itu.
- **Keputusan:** NVIDIA Jetson Orin Nano Super 8 GB dengan Ubuntu 24.04 dan JetPack 7.2.
- **Tahap Pra-TA:** tetap. Pelatihan model dilakukan di GPU Kaggle atau Colab, bukan di Jetson.

### ADR-02 — 68 _landmark_ wajah

- **Keputusan awal:** 68 titik skema iBUG supaya graf wajah ringan. Titik diambil dari 468 titik MediaPipe lalu dipetakan.
- **Tahap Pra-TA:** jumlah 68 titik tetap. Pustaka ekstraksinya dipilih ulang lewat uji perbandingan; pemetaan dari 468 titik MediaPipe tidak lagi menjadi acuan.

### ADR-03 — Kepala regresi valensi-_arousal_

- **Keputusan awal:** model wajah memprediksi valensi dan _arousal_, lalu kategori _mood_ ditentukan di ruang circumplex.
- **Tahap Pra-TA:** menunggu keputusan bimbingan tentang luaran jalur wajah.

### ADR-04 — _Late fusion_

- **Keputusan awal:** jalur wajah dan jalur fisiologis digabung di akhir menjadi satu keluaran berupa 5 kategori _mood_. Bila satu jalur gagal, sistem tetap memberi keluaran dari jalur lain.
- **Tahap Pra-TA:** sistem menghasilkan dua keluaran, yaitu tingkat stres dan kategori _mood_. Peran fusi menunggu keputusan bimbingan.

### ADR-05 — Autentikasi di aplikasi, personalisasi di cermin

- **Keputusan awal:** cermin tetap bisa dipakai dalam mode tamu tanpa akun; akun dan riwayat dikelola di aplikasi pendamping.
- **Tahap Pra-TA:** ditinjau ulang saat merancang aplikasi Flutter (M3).

### ADR-06 — MQTT

- **Keputusan awal:** MQTT dipilih karena antarmuka butuh pola _publish/subscribe_ dan fitur _Last Will and Testament_. Broker produksi memakai TLS di port 8883.
- **Tahap Pra-TA:** MQTT tetap dipakai; bagian TLS diganti oleh ADR-10.

### ADR-07 — Validasi _subject-independent_

- **Keputusan:** data dari subjek yang sama tidak boleh muncul di data latih dan data uji sekaligus, agar model tidak sekadar mengenali orangnya.
- **Tahap Pra-TA:** tetap.

### ADR-08 — Arsip GEMASTIK lewat tag

- **Tanggal:** 27 Oktober 2026.
- **Keputusan:** tag `gemastik-xix-2026` dibuat di setiap repo era GEMASTIK, lalu pekerjaan tahap Pra-TA dilanjutkan di repo yang sama. Nama repo tetap "MoodIn".
- **Alasan:** riwayat dan atribusi tetap utuh tanpa menggandakan repo.

### ADR-09 — Repo publik dengan ruleset

- **Tanggal:** 27 Oktober 2026.
- **Keputusan:** semua repo publik. Ruleset membuat branch default tidak bisa dihapus atau di-_force push_, dan tag `gemastik-*` tidak bisa dihapus atau dipindah. Tidak ada pengecualian (_bypass_).
- **Konsekuensi:** isi repo terlihat oleh umum, sehingga rincian metode yang belum boleh dipublikasikan tidak ditulis di repo.

### ADR-10 — MQTT lokal tanpa TLS

- **Asal:** keputusan bimbingan bersama calon dosen pembimbing.
- **Keputusan:** broker berada di jaringan lokal. Setiap klien wajib memakai _username/password_, isi pesan dienkripsi, dan koneksi tidak memakai TLS.
- **Konsekuensi:** `Hardware-Connections/network/mqtt-tls-config.md` menjadi arsip, dan kontrak pesan ditulis ulang sebagai versi 2.0.0 di `ipc-contract.md` (22 Nov 2026) tanpa port 8883 dan TLS.

### ADR-11 — Flutter di Interface-App

- **Tanggal:** 27 Oktober 2026.
- **Keputusan:** antarmuka baru, yaitu tampilan cermin di Jetson dan aplikasi pendamping Android, dibangun dengan Flutter di repo baru Interface-App. Desainnya dibuat di Figma. Interface-Display (PySide6 + QML dan kiosk web) diarsipkan.
- **Alasan:** satu basis kode untuk cermin dan aplikasi pendamping, serta pemisahan yang jelas dari tumpukan UI lama.
- **Risiko:** dukungan Flutter di Jetson dengan JetPack 7.2 belum teruji, sehingga diuji lebih dulu di M3.

### ADR-12 — MoodIn-master hanya dokumen

- **Tanggal:** 27 Oktober 2026.
- **Keputusan:** MoodIn-master tidak memuat kode yang dijalankan. Skrip integrasi dan _deployment_ di Jetson ditempatkan di Hardware-Connections.
- **Status:** berlaku sampai ada keputusan bimbingan yang mengubahnya.

### ADR-13 — Kontributor sejak tahap Pra-TA

- **Tanggal:** 27 Oktober 2026.
- **Keputusan:** sejak tag `gemastik-xix-2026`, hanya mahasiswa pengusul (Jeremia) yang membuat commit dan push. Commit era GEMASTIK beserta penulisnya tetap di riwayat dan tidak ditulis ulang.
- **Alasan:** karya Pra-TA dan Tugas Akhir dapat dipertanggungjawabkan penulisnya tanpa menghapus atribusi karya anggota tim sebelumnya.

### ADR-14 — Data partisipan di luar repositori

- **Tanggal:** 29 Oktober 2026.
- **Konteks:** penelitian ini akan mengambil data primer dari partisipan setelah persetujuan etik dari Komite Etik Penelitian (KEP) ITS terbit. Semua repo MoodInBME publik, dan berkas yang pernah di-commit tetap tersimpan di riwayat git.
- **Keputusan:** data partisipan dalam bentuk apa pun tidak disimpan di repositori mana pun. Data dikelola di luar GitHub sesuai protokol yang disetujui KEP ITS. Repo hanya memuat kode, dokumen, dan hasil agregat tanpa identitas.
- **Konsekuensi:** pola data partisipan dan rekaman uji ditambahkan ke `.gitignore` setiap repo aktif; _pre-push hook_ di mesin penulis menolak folder data partisipan, berkas media dan sinyal, bobot model, dan berkas besar; aturannya dijelaskan di [etika-dan-data.md](etika-dan-data.md).

### ADR-15 — Kamera UVC sebagai kamera utama (usulan)

- **Tanggal:** 20 November 2026. **Status:** diusulkan, menunggu keputusan bimbingan.
- **Asal:** usulan Calon Dosen Pembimbing 1 untuk mengganti Raspberry Pi HQ Camera (IMX477) dengan Logitech C920.
- **Alasan:**
  - C920 adalah kamera yang dipakai untuk merekam dataset publik UBFC-rPPG, sehingga setelan akuisisi di cermin dapat disamakan dengan data latih.
  - Pada 640 × 480 piksel, C920 dapat mengirim video tanpa kompresi (YUYV) lewat USB.
  - Dukungan driver IMX477 bergantung pada versi JetPack yang terpasang dan sudah beberapa kali menjadi kendala; kamera UVC memakai driver standar Linux.
- **Syarat:** eksposur, _white balance_, dan fokus dikunci manual, dan frekuensi jala-jala diatur 50 Hz sebelum akuisisi.
- **Konsekuensi bila diterima:** pilihan backend kamera di perangkat lunak berpindah dari CSI ke UVC, dan perbandingan kedua kamera dicatat dengan alat uji banding yang hanya menyimpan angka. Tabel spesifikasi perangkat keras baru diubah setelah keputusan.

### ADR-16 — Interaksi gestur tangan (usulan)

- **Tanggal:** 22 November 2026. **Status:** diusulkan, menunggu keputusan bimbingan.
- **Konteks:** layar di balik cermin dua arah tidak dapat disentuh, sehingga pengguna butuh cara memilih menu dan mengisi SAM sembilan posisi tanpa sentuhan.
- **Usulan:** gestur satu tangan dari kamera yang sama. Kursor mengikuti pangkal telunjuk; cubit ibu jari ke telunjuk untuk memilih; telapak terbuka ditahan 1 detik untuk kembali. Gestur nonaktif selama fase pengukuran agar tangan tidak menutupi wajah dan tidak menambah beban komputasi saat sinyal diukur.
- **Privasi:** citra dan titik sendi tangan hanya diproses di memori; yang dikirim lewat MQTT hanya jenis peristiwa dan posisi kursor (`moodin/gesture/event`, kontrak IPC 2.0.0).
- **Implementasi:** modul `interaction/` di Software-Architecture (MediaPipe Hands 0.10.14) dan lapisan kursor di Interface-App. Implementasi ditulis sendiri; dua repositori Apache-2.0 disebut sebagai inspirasi di README modul.
- **Belum diketahui:** beban CPU di Jetson saat gestur dan pelacak wajah berjalan bersamaan, serta ambang cubit untuk pengguna yang berbeda. Keduanya diukur sebelum usulan diputuskan.
