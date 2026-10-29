# Roadmap MoodIn: dari Pra-TA ke Tugas Akhir

Status per 29 Oktober 2026. Kode milestone yang sama dipakai di semua repo. Milestone dimulai pada tahap Proposal Tugas Akhir (Pra-TA) dan berlanjut ke pelaksanaan Tugas Akhir. Rincian metode dan target ada di dokumen proposal (belum dipublikasikan).

Legenda: [x] selesai · [~] berjalan · [ ] belum mulai

## M0 — Arsip GEMASTIK dan tata kelola repo (berjalan)

- [x] Tag `gemastik-xix-2026` di 5 repo era GEMASTIK
- [x] Branch default diseragamkan menjadi `main`; ruleset branch dan tag aktif
- [x] Repo Interface-App dibuat untuk antarmuka Flutter
- [x] Dokumentasi semua repo diselaraskan dengan rancangan proposal (Fase B)
- [x] Interface-Display diarsipkan (_read-only_)
- [x] Tampilan README dan istilah Pra-TA diseragamkan (Fase B2)
- [x] Aturan data partisipan dan halaman etika penelitian (ADR-14)
- [~] Label, milestone, dan _issue_ awal di setiap repo (Fase C)
- [ ] Kontrak pesan antar-repo v2, setelah bimbingan

## M1 — Jalur fisiologis pada dataset sekunder (berikutnya)

- [ ] Pemuat data untuk dataset sekunder
- [ ] Estimasi sinyal denyut dari video, beserta metode pembanding
- [ ] Turunan denyut dan HRV dari sinyal hasil estimasi
- [ ] Klasifikasi tingkat stres dan evaluasinya

## M2 — Perangkat keras dan pengujian di Jetson

- [ ] Keputusan bimbingan: kamera, layar pengganti, susunan cermin, dan pencahayaan
- [ ] Integrasi kamera dan uji latensi per tahap di Jetson
- [ ] Skrip integrasi dan _deployment_ ditata di Hardware-Connections

## M3 — Antarmuka Flutter

- [ ] Uji kelayakan Flutter di Jetson: _build_, layar penuh, orientasi layar, beban CPU dan RAM
- [ ] Kerangka aplikasi cermin dan aplikasi pendamping Android
- [ ] Klien kontrak pesan v2

## M4 — Jalur wajah (setelah keputusan bimbingan)

- [ ] Pemilihan pustaka _landmark_ wajah
- [ ] Model jalur wajah dan evaluasinya

## M5 — Integrasi dan uji sistem

- [ ] Sistem utuh berjalan di Jetson
- [ ] Pengambilan data primer, setelah persetujuan etik terbit
- [ ] Pengujian sistem sesuai Proposal Tugas Akhir

## Persetujuan etik (berjalan paralel)

- [~] Protokol penelitian disiapkan untuk diajukan ke Komite Etik Penelitian (KEP) ITS
- [ ] Persetujuan etik terbit; statusnya dicatat di [etika-dan-data.md](etika-dan-data.md)

## Risiko yang dipantau

| Risiko                                            | Mitigasi                                                                                    |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Keputusan bimbingan mengubah rancangan            | Pekerjaan yang bergantung pada keputusan itu ditunda; keputusan dicatat di [adr.md](adr.md) |
| Kinerja waktu nyata di Jetson tidak tercapai      | Uji latensi per tahap sejak M2, sebelum integrasi                                           |
| Flutter belum teruji di Jetson dengan JetPack 7.2 | Uji kelayakan lebih dulu di M3, sebelum semua layar dipindahkan                             |
| Akses dataset tertunda                            | Mulai dari dataset yang sudah tersedia                                                      |
| Klaim berlebihan                                  | MoodIn ditegaskan sebagai prototipe riset, bukan alat diagnosis medis                       |
| Persetujuan etik terbit terlambat                 | Protokol disiapkan sedini mungkin; data primer tidak diambil sebelum persetujuan terbit     |
