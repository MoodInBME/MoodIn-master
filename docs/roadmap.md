# Roadmap Tugas Akhir — MoodIn

Status per 27 Oktober 2026. Kode milestone yang sama dipakai di semua repo. Rincian metode dan target ada di proposal TA (belum dipublikasikan).

Legenda: [x] selesai · [~] berjalan · [ ] belum mulai

## M0 — Arsip GEMASTIK dan tata kelola repo (berjalan)

- [x] Tag `gemastik-xix-2026` di 5 repo era GEMASTIK
- [x] Branch default diseragamkan menjadi `main`; ruleset branch dan tag aktif
- [x] Repo Interface-App dibuat untuk antarmuka Flutter
- [~] Dokumentasi semua repo diselaraskan dengan TA
- [ ] Interface-Display diarsipkan (_read-only_)
- [ ] Label, milestone, dan _issue_ awal di setiap repo
- [ ] Kontrak pesan antar-repo versi TA, setelah bimbingan

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
- [ ] Klien kontrak pesan versi TA

## M4 — Jalur wajah (setelah keputusan bimbingan)

- [ ] Pemilihan pustaka _landmark_ wajah
- [ ] Model jalur wajah dan evaluasinya

## M5 — Integrasi dan uji sistem

- [ ] Sistem utuh berjalan di Jetson
- [ ] Pengujian sistem sesuai proposal TA

## Risiko yang dipantau

| Risiko                                            | Mitigasi                                                                                    |
| ------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Keputusan bimbingan mengubah rancangan            | Pekerjaan yang bergantung pada keputusan itu ditunda; keputusan dicatat di [adr.md](adr.md) |
| Kinerja waktu nyata di Jetson tidak tercapai      | Uji latensi per tahap sejak M2, sebelum integrasi                                           |
| Flutter belum teruji di Jetson dengan JetPack 7.2 | Uji kelayakan lebih dulu di M3, sebelum semua layar dipindahkan                             |
| Akses dataset tertunda                            | Mulai dari dataset yang sudah tersedia                                                      |
| Klaim berlebihan                                  | MoodIn ditegaskan sebagai prototipe riset, bukan alat diagnosis medis                       |
