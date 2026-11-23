---
type: research-directions
status: proposed
scope: [P001, P002, P028, P029]
updated: 2026-11-23
tags: [mobilenet, pruning, hardware-aware, research-gap]
---

# Arah penelitian awal untuk structured pruning

Dua desain berikut merupakan usulan studi, belum dipilih sebagai topik. Bukti pendukung ditelusuri di [[03-Synthesis/PRUNING_ARCHITECTURE_SYNTHESIS]] dan [[03-Synthesis/Research-Gaps/GAP_EVIDENCE_UPDATE_P001_P029]]. Pemilihan dapat dilakukan setelah pilot, verifikasi manusia dan pemeriksaan literatur pembanding. Prasyarat bersama adalah salinan lokal checkpoint/definisi graph, dataset dengan split tetap, izin menggunakan perangkat, dan runtime yang sudah tersedia.

## D1 — Channel pruning MobileNetV4 dengan pengukuran latency perangkat

| Aspek | Rencana |
| --- | --- |
| Masalah | Seberapa besar keuntungan timing dari pengurangan lebar MobileNetV4 setelah mengontrol recovery, precision dan format export? |
| Dasar bukti | [[02-Literature/Paper-Reviews/P001]] melaporkan 20.00% reduksi parameter tanpa timing (P001-C007–P001-C009). [[02-Literature/Paper-Reviews/P002]] menunjukkan variasi biaya operator dan backend (P002-C007, P002-C015–P002-C016). P029-C013 menunjukkan manfaat paket hybrid dengan atribusi belum terpisah. |
| Ide metode | Uji satu Conv-Small dengan mask kanal terkoordinasi di jalur DW/PW/residual dan pilih lebar kandidat yang didukung kernel. Bandingkan pemeringkatan berbasis magnitudo dengan ranking berbasis profil latency, dengan anggaran kandidat dan recovery yang sama. |
| Potensi kontribusi | Estimasi gain timing terukur per budget accuracy, termasuk kasus parameter berkurang tanpa gain waktu. Publikasikan protokol ekspor dan evidence dari beberapa konfigurasi panjang kanal. |
| Baseline dan hardware | Dense + recovery dengan prosedur sama, magnitude-based channel pruning dan width scaling sederhana. Pilih satu Android/CPU atau Pi/CPU yang tersedia untuk pilot definitif, tetapkan runtime dan thread; perangkat kedua untuk pengujian transfer. Uji precision pertama tetap untuk semua pasangan sebelum menambah INT8 sebagai strata. |
| Metrik | Holdout Top-1, F1 bila kelas tidak seimbang, params, MACs dengan konvensi tetap, byte model, peak RSS/allocator, p50/p90 latency model forward, dan latency aplikasi jika benar-benar diuji. |
| Kesulitan teknis | Propagasi indeks kanal residual/DW, ekspor valid, pembulatan alignment, aliasing akibat sistem/kondisi termal, perbedaan artefak checkpoint. |
| Asumsi terbuka | Profil operator memprediksi ranking model cukup baik. Subset bobot dapat dipangkas tanpa perubahan graph ekspor di luar intervensi. Perangkat dan backend tersedia serta bisa memisahkan timer model dan I/O. |
| Verifikasi novelty | Periksa [[02-Literature/Paper-Reviews/P010]], [[02-Literature/Paper-Reviews/P020]] dan [[02-Literature/Paper-Reviews/P007]] untuk pemilihan kanal, model biaya, target perangkat, serta apakah evaluasi sudah menguji MobileNetV4 dengan kontrol dan granularity yang sama. Catat literatur tambahan yang belum tersedia lokal. |
| Kelayakan | **Sedang–tinggi** untuk klasifikasi satu perangkat jika model dapat diekspor. Pilot menguji validitas graph dan variansi waktu sebelum mempertimbangkan dua perangkat. |

## D2 — Pemilihan blok UIB berdasarkan profil hardware

| Aspek | Rencana |
| --- | --- |
| Masalah | Blok UIB mana yang layak dilepas atau disederhanakan pada batas accuracy tertentu? Apakah urutan pilihan bertahan pada perangkat lain? |
| Dasar bukti | [[02-Literature/Paper-Reviews/P029]] memberi kurva sensitivitas terhadap jumlah blok serta bisection atas urutan tetap (P029-C003–P029-C005, P029-C009). P002-C003–P002-C007 memperlihatkan perbedaan biaya pilihan blok/layout. [[02-Literature/Paper-Reviews/P028]] memberi ablation modul dan pemisahan forward serta pipeline (P028-C007, P028-C015). |
| Ide metode | Buat kandidat untuk UIB ber-stride 1 dan shape kompatibel. Ukur latency dan quality pada beberapa penghilangan tunggal dengan prosedur sama. Pada search terbatas, bandingkan suffix/depth order dengan ranking marginal accuracy–latency, lalu uji kombinasi terbaik pada kedua perangkat. Periksa interaksi blok dengan kombinasi kecil sebelum mengandalkan additive cost. |
| Potensi kontribusi | Peta sensitivitas UIB dengan komparasi search ber-budget sama dan batas transfer yang diukur. Validasi apakah retained count cukup untuk memandu pemilihan. |
| Baseline dan hardware | MobileNetV4 Conv-Small padat, suffix bisection bergaya P029, random matched-budget dan sweep terbatas sebagai acuan. Uji satu CPU mobile dahulu dan satu SBC CPU dengan export yang sama serta versi backend tercatat bila tersedia. |
| Metrik | Top-1 dan pp, p50/p90 latency, export size, MACs, peak memory, jumlah kandidat, total search compute, tingkat fallback dan rank per perangkat. |
| Kesulitan teknis | UIB downsampling dan channel transitions tidak dapat dilepas bebas; repeated fine-tuning mahal; profiling individual tidak menangkap fusion, interaksi memori dan perubahan kernel. |
| Asumsi terbuka | Ada cukup UIB yang kompatibel untuk ablation; timing antar-run stabil; domain klasifikasi cukup sensitif untuk membedakan kandidat. |
| Verifikasi novelty | Full review [[02-Literature/Paper-Reviews/P007]] untuk prior art UIB, [[02-Literature/Paper-Reviews/P017]] untuk pengubahan V4-Small dan [[02-Literature/Paper-Reviews/P020]] untuk pencarian berbasis reward. Cocokkan ruang pencarian, mekanisme pruning, hardware dan kontrol ablation sebelum mengusulkan kontribusi. |
| Kelayakan | **Sedang** untuk satu perangkat dengan jumlah kandidat kecil. Perlu waktu training dan akses perangkat kedua untuk menyimpulkan transfer. |
