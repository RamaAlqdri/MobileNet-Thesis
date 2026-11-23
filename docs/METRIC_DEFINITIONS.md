---
type: metric-definitions
scope: [P002, P028]
updated: 2026-11-23
tags: [mobilenet, hardware-aware, literature-review]
---

# Definisi operasional metrik

Gunakan definisi umum untuk rancangan tesis dan definisi paper untuk transkripsi. Bukti per konfigurasi ada di [[02-Literature/Literature-Matrix/CRITICAL_EVIDENCE_MATRIX]]. `NR` berarti Not reported dan `NV` berarti Not verified.

| Metrik | Definisi umum untuk pencatatan | P002 | P028 |
| --- | --- | --- | --- |
| Accuracy, Top-1 | Prediksi kelas tertinggi yang benar dibagi jumlah sampel pada split tertentu. Selisih dua persentase dilaporkan dalam poin persentase. | ImageNet-1K validation Top-1 untuk klasifikasi. Nilai training muncul hanya di beberapa ablation. P002-C003/P002-C009/P002-C010/P002-C013. | Persentase klasifikasi held-out kemasan/NEU-CLS. Table 14 menyebut Top-1. Pemisahan validation/test harus diklarifikasi. P028-C005/P028-C006/P028-C010/P028-C014. |
| Precision, recall, F1 | Rasio TP terhadap semua prediksi positif, rasio TP terhadap semua positif aktual, dan harmonic mean. Untuk multi-kelas, rekam macro/micro/weighted/per-class dan support. | NR pada benchmark klasifikasi utama. | Eqs. (11)–(13) mendefinisikan bentuk confusion metrics. Cara agregasi tabel lintas empat kelas NR. P028-C005–P028-C007. |
| AP deteksi | Ringkasan precision–recall pada aturan IoU/split dataset yang ditetapkan. Catat dataset dan implementasi evaluator. | COCO 2017 val AP untuk RetinaNet, input 384 px. P002-C012. | Paper menilai klasifikasi kemasan. |
| Parameter count | Jumlah elemen bobot model menurut aturan yang dinyatakan, termasuk penjelasan trainable/buffer. Tidak mengukur byte atau RAM. | Table 6 untuk classifier, Table 7 seluruh detector. P002-C010/P002-C012. | Pakai hitungan per tabel: Conv-S 2,30 M di Table 5 dan Base 2,36 M di Table 7, Ours 3,70 M vs 3,76 M. P028-C006/P028-C007/P028-C009. |
| MACs dan FLOPs | MAC ialah satu multiply–accumulate. FLOP adalah operasi floating-point menurut konvensi penghitung. Nyatakan bagaimana multiply dan add dihitung, operator apa yang dikecualikan, serta batch/resolusi. Hindari konversi universal. | GMAC model/detector, dengan operational intensity MAC/byte dalam roofline. P002-C010/P002-C012/P002-C015. | GFLOPs model (jumlah operasi, tidak ditafsirkan sebagai throughput FLOP/s). Konvensi counter NV. P028-C006/P028-C007/P028-C009. |
| Latency | Lama interval dari awal hingga akhir unit kerja relatif terhadap batas timer. Rekam unit ms, statistik, repetisi, warm-up, batch, threads, presisi, runtime dan perangkat. | Median dari lima mean, masing-masing sekitar 1.000 run untuk metode umum TFLite. Timer I/O/pre/post tidak rinci dan ANE perlu audit terpisah. Selisih incremental attention berbeda dari latensi model. P002-C004/P002-C005/P002-C010/P002-C011. | 39,58 ms adalah forward single-image Ours pada Pi 4B. Protokol statistik/repetisi NR. 39 ms di uraian deployment memiliki scope NV. P028-C007/P028-C008/P028-C017. |
| Throughput | Jumlah unit selesai per waktu, misalnya frame/s, image/s, atau item/s. Pipeline async dapat overlap, sehingga throughput tidak menetapkan latency per item. | FPS pipeline NR untuk konfigurasi utama. | FPS 25,27 merupakan laju forward single-image. Mean 23,35 FPS adalah pipeline stress test, dengan preprocessing dan komunikasi PLC. 40 bags/min adalah kecepatan lini. P028-C008/P028-C015/P028-C017. |
| Model size | Ukuran artefak serialisasi dalam byte, dengan format, precision, quantization metadata, compression dan ukuran disk/transfer disebutkan. | Ukuran artefak NR untuk konfigurasi yang dipilih. | Ukuran artefak NR. 3,70/3,76 M adalah parameter. |
| Peak RAM | Maksimum penggunaan memori dalam interval dan scope jelas (RSS proses, allocator runtime, atau sistem) dengan metode sampling/peak counter. Capacity RAM, VRAM dan bobot tidak dapat dipertukarkan. | Tidak ada peak RAM pada Table 6/7. Bytes teoritis roofline menggambarkan biaya perpindahan data. P002-C015. | Fig. 16(c) menunjukkan memory usage stress test. Definisi proses/sistem dan peak RAM model NV. Kit 8 GB di Table 17 adalah kapasitas. P028-C016/P028-C017. |
| Energi per inferensi | Integral daya terhadap waktu untuk interval yang ditentukan, dalam joule, dibagi jumlah inferensi. Tetapkan scope perangkat, apakah idle dikurangkan, dan batas batching. | NR untuk konfigurasi utama. | NR. Suhu CPU Fig. 16(b) merupakan data termal, tanpa pengukuran daya atau joule per inferensi. P028-C015. |

## Kategori batas timer

- **Model forward-pass only.** Graph model dari tensor input siap hingga tensor keluaran. Catat sinkronisasi dan transfer yang benar-benar masuk. Table 7/9 P028 menyediakan contoh single-image forward.
- **End-to-end model inference.** Masuknya satu contoh ke runner hingga prediksi akhir, termasuk pre/postprocessing model jika timer memang mencakupnya. Gunakan kategori ini ketika batas itu ditegaskan oleh sumber atau protokol.
- **Full application pipeline.** Siklus aplikasi seperti trigger/akuisisi, preprocessing, model, hasil, komunikasi atau aktuasi. Catat subset yang termasuk. P028 menghitung throughput pada sistem yang mencakup preprocessing dan PLC.
- **Operator-level latency.** Pengukuran langsung satu kernel atau blok dengan bentuk tensor dan runtime tetap. Prediksi roofline dan selisih dua timing model dicatat sebagai estimasi biaya atau delta, dengan label berbeda dari profil operator langsung.
- **Unclear / Not reported.** Sumber tidak mendefinisikan batas dengan cukup rinci. Gunakan preset ini untuk timer Table 6 P002 pada komponen I/O dan 39 ms deployment P028. Jangan imputasi scope dengan melihat satuan atau kebalikan FPS.

Benchmark mencatat jenis runtime (misalnya TFLite/XNNPACK, Core ML, atau engine ONNX tertentu), target CPU/GPU/NPU, serta fallback operator jika diketahui. Untuk pengukuran throughput, cantumkan unit item dan perbedaan antara persentil throughput dan persentil latency per item. Lihat [[03-Synthesis/PAIRWISE_COMPARISON_P002_P028]].
