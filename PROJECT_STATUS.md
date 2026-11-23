---
type: status
phase: eksplorasi-literatur
updated: 2026-11-23
tags: [mobilenet, thesis-progress]
---

# Status proyek

**Pembaruan terakhir** 2026-11-23 (Asia/Jakarta)

**Fase** Eksplorasi literatur. Batch Phase 3 mencakup dua full review selesai oleh agent, dengan bukti lokal dan validasi manusia menunggu.

**Tujuan** Memetakan pendekatan optimasi MobileNet dan bukti pengaruhnya terhadap akurasi, latensi, memori, ukuran model, serta kompleksitas komputasi pada perangkat mobile dan edge.

| Indikator | Jumlah |
| --- | ---: |
| PDF ditemukan di `Paper/` | 29 |
| Paper diinventarisasi, dengan Paper ID | 29 |
| Paper selesai screening awal | 29 |
| INCLUDE / MAYBE / EXCLUDE | 19 / 9 / 1 |
| Paper menunggu screening awal | 0 |
| Paper di shortlist full review | 15 |
| Full review selesai oleh agent | 2 |
| Full review selesai divalidasi manusia | 0 |
| Paper masuk perbandingan pairwise | 2 |
| Item bukti terdaftar, lokal / parsial | 36 (25 / 11) |
| Kandidat gap (hypothesized gap) | 6 |
| Kandidat gap ditinjau ulang terhadap P002/P028 | 6 |
| Gap literatur yang dikonfirmasi | 0 |
| Pertanyaan riset dirumuskan | 0 |

Review lengkap tersedia untuk [[02-Literature/Paper-Reviews/P002]] dan [[02-Literature/Paper-Reviews/P028]]. [[02-Literature/Literature-Matrix/CRITICAL_EVIDENCE_MATRIX]] memuat 131 konfigurasi/rekaman benchmark. Hasil sintesis awal batch ada di [[03-Synthesis/PAIRWISE_COMPARISON_P002_P028]] dan [[03-Synthesis/Research-Gaps/GAP_EVIDENCE_UPDATE_P002_P028]]. Triase seluruh korpus tetap di [[02-Literature/Literature-Matrix/SCREENING_MATRIX]].

## Pertanyaan terbuka dan hambatan

- P002 merupakan sumber dasar MobileNetV4 dalam versi preprint lokal. Sumber primer `Searching for MobileNetV3` tercantum sebagai referensi di P002, dengan PDF primer belum tersedia dalam inventaris.
- Teks P022 dapat dibaca. Preflight struktur ulang tetap `UNAVAILABLE`. Gunakan locator berbasis bagian. Detail timing P006/P007 serta model GPU P025 memerlukan pemeriksaan saat full review.
- Kelayakan final, transfer hasil FPGA/domain lain dan validasi manusia tetap terbuka. Untuk P002/P028, prioritas verifikasi meliputi pasangan checkpoint 87% dan timing P002, perbedaan angka serta graph LA P028, dan protokol runtime/memori. Lihat [[02-Literature/VERIFICATION_BACKLOG]].
- Verifikasi eksternal metadata dan DOI berada di luar cakupan lokal. Lihat versi sumber di [[02-Literature/PAPER_INVENTORY]].

## Status Git

Pre-flight Phase 3 menemukan branch `master` di `9e42368`, working tree bersih, dan ref tracking `origin/master` tetap pada `fd0111d`. Keadaan ini konsisten dengan hasil akhir Phase 2. Perubahan Phase 3 dicatat melalui Git lokal.

## Prioritas berikutnya

1. Validasi manual locator, angka dan konflik utama P002/P028 dari [[02-Literature/VERIFICATION_BACKLOG]].
2. Jadwalkan full review P001 dan P029 untuk mengevaluasi pruning pada MobileNetV4/V3 serta definisi timing dan baseline. P007 berikutnya untuk pencarian UIB dan perangkat benchmark.
3. Periksa sumber primer V3 bila salinan lokal tersedia dan perluas matriks dengan konfigurasi yang dapat dilacak.
