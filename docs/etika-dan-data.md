# Etika penelitian dan pengelolaan data

MoodIn mengolah video wajah dan memperkirakan kondisi emosional seseorang. Halaman ini menjelaskan cara proyek ini memisahkan **cara kerja cermin** dari **pengambilan data penelitian**, beserta aturan data yang berlaku di semua repositori organisasi MoodInBME.

Rincian protokol penelitian, instrumen, dan rancangan pengujian ada di dokumen Proposal Tugas Akhir dan dokumen pengajuan etik, yang **belum dipublikasikan**. Keputusan terkait dicatat sebagai ADR-14 di [adr.md](adr.md).

## Status persetujuan etik

| Tanggal     | Status                                                                                                              |
| ----------- | ------------------------------------------------------------------------------------------------------------------- |
| 29 Okt 2026 | Protokol penelitian sedang disiapkan untuk diajukan ke Komite Etik Penelitian (KEP) ITS. Data primer belum diambil. |

Tabel ini diperbarui setiap kali statusnya berubah, misalnya saat protokol diajukan, diminta revisi, atau disetujui.

## Dua konteks data

|               | Cara kerja cermin                                                       | Pengambilan data penelitian (data primer)                                         |
| ------------- | ----------------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| Berlaku untuk | Setiap pemakaian cermin dalam mode normal                               | Sesi penelitian dengan partisipan yang sudah menyatakan setuju                    |
| Video wajah   | Diolah langsung di perangkat, tidak disimpan, dan tidak dikirim ke luar | Boleh direkam hanya bila diperlukan dan diizinkan protokol yang disetujui KEP ITS |
| Yang disimpan | Hasil pemeriksaan berupa angka                                          | Sesuai protokol: jenis data, lokasi, pengamanan, hak akses, dan masa simpan       |
| Dasar         | Prinsip di README dan persetujuan pengguna di layar                     | Persetujuan partisipan setelah penjelasan dan protokol yang disetujui KEP ITS     |

Partisipan boleh mengundurkan diri kapan saja tanpa konsekuensi. Perlakuan terhadap data yang sudah terkumpul mengikuti lembar persetujuan.

Fitur yang memakai layanan pihak ketiga, misalnya layanan suara berbasis cloud, dijelaskan lebih dulu kepada pengguna dan partisipan. Selama pengambilan data penelitian, fitur seperti itu hanya dipakai bila protokol mengizinkannya.

## Aturan untuk semua repositori

1. **Data partisipan tidak pernah masuk repositori**, dalam bentuk apa pun: rekaman video atau suara, sinyal fisiologis, jawaban kuesioner, hasil skrining, data identitas, lembar persetujuan, serta kode partisipan beserta kunci penghubungnya. Aturan ini juga berlaku untuk turunan yang masih bisa dikaitkan dengan seseorang, misalnya _landmark_ atau fitur per partisipan.
2. **Data partisipan disimpan di luar GitHub**, dengan akses terbatas, sesuai protokol.
3. **Data partisipan tidak diunggah ke layanan pihak ketiga**, seperti GPU Kaggle atau Colab, penyimpanan cloud, atau alat AI, kecuali protokol yang disetujui KEP ITS mengizinkannya.
4. **Publikasi hanya memuat hasil agregat tanpa identitas.** Ini berlaku untuk laporan, makalah, presentasi, dan README. Wajah partisipan hanya ditampilkan dengan izin tertulis yang tercakup dalam protokol.
5. **Data primer tidak dibagikan secara terbuka.** Pemakaian ulang untuk penelitian lain hanya dilakukan bila persetujuan partisipan dan KEP ITS mencakupnya.
6. **Bobot model yang dilatih atau disesuaikan dengan data partisipan diperlakukan seperti data partisipan:** tidak di-commit dan tidak dirilis tanpa dasar yang sama.
7. **Dataset publik (sekunder) dipakai sesuai lisensi atau perjanjian (EULA) masing-masing.** Berkasnya tidak diunggah ulang, dan cuplikan _frame_ hanya ditampilkan bila diizinkan.
8. **Pengaman teknis hanya lapis kedua.** Pola `.gitignore` di setiap repo dan _pre-push hook_ di mesin penulis menolak folder data partisipan, rekaman uji, berkas media dan sinyal, bobot model, serta berkas besar. Aturan utamanya tetap: data disimpan di luar folder repo.

## Bila data terpapar

Laporkan secara tertutup sesuai [SECURITY.md](https://github.com/MoodInBME/.github/blob/main/SECURITY.md), bukan lewat _issue_ publik. Penulis menghapus berkasnya, menulis ulang riwayat git bila perlu, meminta GitHub Support menghapus salinan yang masih tersimpan, dan melaporkan kejadiannya sesuai protokol yang disetujui KEP ITS.

## Hubungan dengan lisensi

Lisensi di setiap repositori, atau ketiadaan lisensi, hanya berlaku untuk kode dan dokumen. Data partisipan tidak pernah termasuk dalam lisensi repositori; pemakaiannya hanya mengikuti persetujuan partisipan dan protokol yang disetujui KEP ITS.
