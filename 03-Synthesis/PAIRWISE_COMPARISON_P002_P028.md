---
type: pairwise-comparison
scope: [P002, P028]
updated: 2026-11-23
tags: [mobilenet, literature-review, hardware-aware]
---

# Perbandingan kritis P002 dan P028

[[02-Literature/Paper-Reviews/P002]] memperkenalkan MobileNetV4 dan menguji trade-off akurasi–latensi lintas backend mobile. [[02-Literature/Paper-Reviews/P028]] mengadaptasi Conv-Small untuk inspeksi kemasan dan menguji biaya modifikasi pada Raspberry Pi 4B. Bukti langsung dari dua studi berguna untuk menyusun protokol yang seragam. Kecepatan absolut antarpaper bergantung pada workload, hardware, presisi dan batas timer yang berbeda.

## Klasifikasi perbandingan

| Aspek | P002 | P028 | Validitas dan kegunaan |
| --- | --- | --- | --- |
| Tujuan dan metode | Arsitektur dasar lintas perangkat. UIB, Mobile MQA, NAS dua tahap dan distilasi. | Task inspeksi seal. LA, PIB dan ASML, dengan optimasi loss dan stress test. | **CONDITIONALLY COMPARABLE** sebagai desain respons terhadap constraint hardware. Bandingkan mekanisme dan bukti ablation sesuai tugas. [P002-C001, P002-C008, P028-C001–P028-C004] |
| Bentuk input nominal | Conv-S 224×224×3 (Appendix D). Conv/Hybrid M dan L memakai ukuran lain. | Input 224×224×3 untuk model kemasan. | **DIRECTLY COMPARABLE** untuk dimensi nominal Conv-S dan model kemasan. Resizing, normalisasi, distribusi data, dan kesetaraan graph diperiksa terpisah. [P002-C002, P028-C001, P028-C005] |
| Struktur backbone dan attention | Stem dan UIB, head 1000 kelas untuk klasifikasi ImageNet. Mobile MQA dengan K/V dipakai bersama, opsional DW 3×3 stride 2 dan penataan Einsum, diterapkan pada Hybrid atau setup ablasi. | Turunan Conv-Small dengan head 4 kelas, LA 3×5 avg pooling pada K/V dan PIB. Table 10 menguji Mobile_Mqa pada task kemasan. | **CONDITIONALLY COMPARABLE** untuk pola operator dan stage yang dapat disejajarkan, dengan graph, head dan stride LA perlu diverifikasi. **INSUFFICIENT EVIDENCE** untuk kecepatan LocalAttention vs Mobile MQA pada graph, task dan target yang sama. [P002-C001, P002-C002, P002-C004, P002-C006, P028-C001–P028-C003, P028-C012] |
| Dataset, split, dan kualitas prediksi | Top-1 ImageNet-1K validation dan AP COCO 2017 validation. Ablasi/pelatihan tambahan diisolasi. | Accuracy/P/R/F1 held-out kemasan dan transfer arsitektur pada NEU-CLS. Agregasi per kelas dan split detail belum tuntas. | **NOT DIRECTLY COMPARABLE** untuk persentase akurasi, F1 dan AP lintas task. [P002-C010, P002-C012–P002-C014, P028-C005, P028-C006, P028-C010] |
| Hardware, presisi, runtime | TFLite INT8 CPU/DSP/EdgeTPU, FP16 GPU, iPhone 13 Core ML float16. | RTX 4090 untuk setup eksperimen, Pi 4B untuk benchmark dan sistem. ONNX disebut pada desain deployment. Precision, provider, versi runtime timing NR. | **NOT DIRECTLY COMPARABLE** untuk latensi antarperangkat. **INSUFFICIENT EVIDENCE** untuk efek quantization karena graph dan presisi P028 tidak ditentukan. [P002-C010, P002-C011, P028-C005, P028-C007, P028-C017] |
| Latensi model dan statistik | Model on-device, TFLite umum median dari lima mean, masing-masing sekitar 1.000 run. Batas timer I/O dan detail ANE belum terverifikasi. | Pi 4B 39,58 ms forward satu citra. Statistik latency, repetisi, warm-up model, dan thread NR. | **NOT DIRECTLY COMPARABLE** untuk angka P002 vs P028. **DIRECTLY COMPARABLE** dalam Table 7 P028 untuk Base vs Ours pada kondisi forward yang sama sesuai caption. [P002-C011, P028-C007–P028-C009] |
| Pipeline throughput | Tidak ada metrik pipeline aplikasi yang dipasangkan dengan benchmark Table 6. | Mean 23,35 FPS uji 8 jam termasuk preprocessing dan komunikasi PLC, terpisah dari 25,27 FPS forward. | **INSUFFICIENT EVIDENCE** untuk peringkat throughput pipeline lintas paper. Waktu per tahap P028 belum cukup untuk menurunkan overhead per frame dari kedua FPS. [P002-C010, P002-C011, P028-C015, P028-C017] |
| Kompleksitas dan parameter | Parameter dan GMAC per model atau detektor. | Parameter dan GFLOPs. Angka parameter bergantung tabel. | **CONDITIONALLY COMPARABLE** untuk ukuran structural yang dihitung ulang dengan graph, head dan input disamakan. **INSUFFICIENT EVIDENCE** untuk konversi angka GMAC/GFLOPs antarpenulis yang tidak mendefinisikan aturan operasi seragam. [P002-C002, P002-C010, P002-C012, P028-C006, P028-C007, P028-C009] |
| Memori, ukuran file, energi | Model roofline memakai bytes bobot + aktivasi untuk biaya teoritis. Peak RAM, ukuran file, energi per inferensi NR. | Fig. 16(c) memory usage sepanjang stress test tanpa definisi proses/peak RAM, ukuran artefak dan energi NR. | **INSUFFICIENT EVIDENCE** untuk perbandingan kuantitatif. Kapasitas Pi dan model biaya P002 bukan ukuran peak RAM. [P002-C015, P002-C016, P028-C016] |
| Trade-off dan reprodusibilitas | Within-paper benchmark menunjukkan Conv vs Hybrid dan MQA vs MHSA. Rincian model dan training tersedia, tetapi export dan cakupan target berbeda. | Within-paper ablation mendukung tambahan akurasi dengan tambahan waktu. Graph/stride, split, dan detail runner belum sepenuhnya terjelaskan. | **CONDITIONALLY COMPARABLE** untuk bentuk trade-off dan kualitas pelaporan setelah tiap baseline diselaraskan. **NOT DIRECTLY COMPARABLE** untuk besar perubahan antarpaper. [P002-C003–P002-C008, P002-C010, P028-C005, P028-C007, P028-C011, P028-C017] |

## Implikasi untuk desain eksperimen

1. Gunakan baseline Conv-S yang sama pada tiap backend, dengan input, kepala klasifikasi, split, presisi dan file model dicatat. Gunakan kontrol arsitektur sebelum menambah LA, PIB, atau opsi UIB. Rekam kinerja pada holdout yang sama.
2. Ukur forward pass dan pipeline end-to-end sebagai kondisi berbeda. Rekam durasi warm-up, repetisi, thread, input copy, suhu, versi runtime, dan penyebaran timing. Profil per operator untuk menguji biaya DW, pointwise, PConv dan perubahan layout.
3. Simpan hasil yang dilaporkan per konfigurasi di [[02-Literature/Literature-Matrix/CRITICAL_EVIDENCE_MATRIX]]. Tunda perhitungan dampak antarstudi sampai item di [[02-Literature/VERIFICATION_BACKLOG]] terselesaikan. Implikasi terhadap G01–G06 ada di [[03-Synthesis/Research-Gaps/GAP_EVIDENCE_UPDATE_P002_P028]].
