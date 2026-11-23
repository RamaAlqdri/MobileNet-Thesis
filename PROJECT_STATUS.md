---
type: status
phase: eksplorasi-literatur
updated: 2026-11-23
tags: [mobilenet, thesis-progress]
---

# Status proyek

**Pembaruan terakhir** 2026-11-23 (Asia/Jakarta)

**Fase** Eksplorasi literatur. Phase 4 menyelesaikan batch structured pruning P001 dan P029. Review lain, sintesis lintas korpus dan validasi manusia masih berjalan.

**Tujuan** Memetakan pendekatan optimasi MobileNet dan bukti pengaruhnya terhadap akurasi, latensi, memori, ukuran model, serta kompleksitas komputasi pada perangkat mobile dan edge.

| Indikator | Jumlah |
| --- | ---: |
| PDF ditemukan di `Paper/` | 29 |
| Paper diinventarisasi, dengan Paper ID | 29 |
| Paper selesai screening awal | 29 |
| INCLUDE / MAYBE / EXCLUDE | 19 / 9 / 1 |
| Paper menunggu screening awal | 0 |
| Paper di shortlist full review | 15 |
| Full review selesai oleh agent | 4 |
| Full review selesai divalidasi manusia | 0 |
| Paper masuk sintesis kritis | 4 |
| Item bukti terdaftar, lokal / parsial | 65 (42 / 23) |
| Konfigurasi di matriks bukti kritis | 150 |
| Pasangan baseline–pruned | 13 |
| Kandidat gap (hypothesized gap) | 6 |
| Dukungan dalam korpus empat paper, parsial / bukti tidak cukup | 4 / 2 |
| Gap literatur yang dikonfirmasi | 0 |
| Pertanyaan riset dirumuskan | 0 |

Review lengkap tersedia untuk [[02-Literature/Paper-Reviews/P001]], [[02-Literature/Paper-Reviews/P002]], [[02-Literature/Paper-Reviews/P028]] dan [[02-Literature/Paper-Reviews/P029]]. Phase 4 menambah 19 konfigurasi ke [[02-Literature/Literature-Matrix/CRITICAL_EVIDENCE_MATRIX]] dan 13 pasangan baseline–pruned di [[02-Literature/Literature-Matrix/PRUNING_EVIDENCE_MATRIX]]. [[03-Synthesis/PRUNING_ARCHITECTURE_SYNTHESIS]] menjawab RQ-A–RQ-F sesi ini. [[03-Synthesis/Research-Gaps/GAP_EVIDENCE_UPDATE_P001_P029]] mencatat dukungan korpus untuk enam kandidat. Triase ada di [[02-Literature/Literature-Matrix/SCREENING_MATRIX]].

## Pertanyaan terbuka dan hambatan

- P002 merupakan sumber dasar MobileNetV4 dalam versi preprint lokal. Sumber primer `Searching for MobileNetV3` tercantum sebagai referensi di P002, dengan PDF primer belum tersedia dalam inventaris.
- Teks P022 dapat dibaca. Preflight struktur ulang tetap `UNAVAILABLE`. Gunakan locator berbasis bagian. Detail timing P006/P007 serta model GPU P025 memerlukan pemeriksaan saat full review.
- P001 tidak melaporkan timing perangkat. P029 melaporkan penurunan waktu paket hybrid, dengan identitas board, precision per checkpoint dan batas timer belum terperinci. Peak RAM dan energi untuk pasangan pruning belum tersedia. Konflik accuracy/F1 P029 dan scoring/split P001 perlu validasi manusia.
- Kelayakan final, transfer hasil FPGA/domain lain dan validasi manusia tetap terbuka. Untuk P002/P028, prioritas verifikasi meliputi pasangan checkpoint 87% dan timing P002, perbedaan angka serta graph LA P028, dan protokol runtime/memori. Lihat [[02-Literature/VERIFICATION_BACKLOG]].
- Verifikasi eksternal metadata dan DOI berada di luar cakupan lokal. Lihat versi sumber di [[02-Literature/PAPER_INVENTORY]].

## Status Git

Pre-flight Phase 4 menemukan branch `master` di `67ec634`, working tree bersih, dan ref tracking lokal `origin/master` di `fd0111d`. Keadaan konsisten dengan laporan Phase 3. Rincian sesi ada di [[logs/2026-11-23#Sesi 4]].

## Prioritas berikutnya

1. Validasi manusia atas scoring/split P001, konflik T4/Fig. 9(a) dan provenance timing T6 P029, serta item terdahulu di [[02-Literature/VERIFICATION_BACKLOG]].
2. Full review [[02-Literature/Paper-Reviews/P007]] untuk ruang pencarian UIB dan pemakaian latensi aktual sebagai reward. Lanjutkan [[02-Literature/Paper-Reviews/P020]] untuk cara menguji transfer prediktor, lalu [[02-Literature/Paper-Reviews/P006]] untuk pembanding structured pruning yang lebih terkontrol.
3. Setelah bukti cukup, periksa ketersediaan perangkat, graph dan dataset lokal untuk menguji kelayakan [[03-Synthesis/Research-Directions/INITIAL_PRUNING_RESEARCH_DIRECTIONS]]. Periksa sumber primer V3 apabila salinan lokal tersedia.
