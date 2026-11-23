---
type: status
phase: eksplorasi-literatur
updated: 2026-11-23
tags: [mobilenet, thesis-progress]
---

# Status proyek

**Pembaruan terakhir** 2026-11-23 (Asia/Jakarta)

**Fase** Eksplorasi literatur. Screening awal dan pemetaan korpus lokal selesai. Tahap berikutnya adalah full review.

**Tujuan** Memetakan pendekatan optimasi MobileNet dan bukti pengaruhnya terhadap akurasi, latensi, memori, ukuran model, serta kompleksitas komputasi pada perangkat mobile dan edge.

| Indikator | Jumlah |
| --- | ---: |
| PDF ditemukan di `Paper/` | 29 |
| Paper diinventarisasi, dengan Paper ID | 29 |
| Paper selesai screening awal | 29 |
| INCLUDE / MAYBE / EXCLUDE | 19 / 9 / 1 |
| Paper menunggu screening awal | 0 |
| Paper di shortlist full review | 15 |
| Full review selesai | 0 |
| Paper masuk sintesis berbasis full review | 0 |
| Kandidat gap (hypothesized gap) | 6 |
| Gap yang didukung bukti full review | 0 |
| Pertanyaan riset dirumuskan | 0 |

Keputusan triase satu reviewer ada di [[02-Literature/Literature-Matrix/SCREENING_MATRIX]]. Pemetaan awal ada di [[03-Synthesis/PRELIMINARY_LITERATURE_MAP]] dan [[03-Synthesis/RESEARCH_TAXONOMY]]. Observasi kandidat ada di [[03-Synthesis/Research-Gaps/PRELIMINARY_GAPS]].

## Pertanyaan terbuka dan hambatan

- P002 merupakan sumber dasar MobileNetV4 dalam versi preprint lokal. Sumber primer `Searching for MobileNetV3` tercantum sebagai referensi di P002, dengan PDF primer belum tersedia dalam inventaris.
- Teks P022 dapat dibaca. Preflight struktur ulang tetap `UNAVAILABLE`. Gunakan locator berbasis bagian. Detail timing P006/P007 serta model GPU P025 memerlukan pemeriksaan saat full review.
- Status kelayakan final, mutu eksperimen, kesetaraan runtime, dan transfer hasil FPGA atau domain lain masih perlu dinilai. Validasi manusia atas keputusan triase belum dilakukan.
- Verifikasi eksternal metadata dan DOI berada di luar cakupan lokal. Lihat versi sumber di [[02-Literature/PAPER_INVENTORY]].

## Status Git

Branch `master` awalnya berada di `fd0111d` tanpa remote terdaftar. Saat recheck Phase 2, `origin/master` tercatat pada commit dasar yang sama. Seluruh pekerjaan Phase 2 memakai Git lokal.

## Prioritas berikutnya

1. Mulai full review dengan P002 dan P028 untuk mengekstrak konfigurasi baseline, backend, dan protokol timing. Lanjut dari [[02-Literature/Reading-Queue/PRIORITY_READING_LIST]].
2. Periksa ketersediaan salinan lokal sumber dasar V3 dan telaah kembali keputusan MAYBE bila arah review berubah.
3. Isi [[02-Literature/Literature-Matrix/LITERATURE_MATRIX]] dari review lengkap dan gunakan untuk menguji kandidat gap.
