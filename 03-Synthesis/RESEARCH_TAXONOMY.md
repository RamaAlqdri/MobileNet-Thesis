---
type: literature-taxonomy
scope: korpus-lokal
updated: 2026-11-23
tags: [mobilenet, architecture-optimization, hardware-aware, pruning, quantization, literature-review]
---

# Taksonomi penelitian MobileNet

Satu tema utama diberikan berdasarkan kontribusi dominan tiap paper. Tema sekunder mencatat metode tambahan atau bentuk evaluasi yang penting. Hitungan utama mencakup 29 paper. Kolom semua peran menghitung satu paper sekali per tema, sehingga totalnya boleh melebihi 29. Distribusi mencakup seluruh korpus screening, dengan keputusan per paper di [[02-Literature/Literature-Matrix/SCREENING_MATRIX]].

| Tema | Utama | Semua peran | Ciri pembeda dan contoh |
| --- | ---: | ---: | --- |
| Architecture Optimization | 5 | 12 | Mengubah blok, head, atau alur komputasi. P001, P017 dan P028 memodifikasi MobileNetV4 untuk kebutuhan aplikasi yang berbeda. |
| Hardware-Aware NAS | 6 | 7 | Memakai pengukuran, prediksi, atau model biaya hardware saat mencari arsitektur. P007 mencari konfigurasi UIB, P020 memakai loss latensi, P023 menguji prediktor untuk NAS. |
| Structured Pruning | 3 | 5 | Menghapus blok atau kanal, atau memaksakan pola sparsity. P006 menggunakan norma L1, P010 menyeragamkan pola lintas filter, dan P029 mencari blok/kanal dengan bisection. |
| Quantization | 4 | 10 | Mengurangi bit-width, seragam atau campuran. P012 memakai INT8, P008 dan P022 memilih presisi berdasarkan sensitivitas lapisan. |
| Attention / Bottleneck Optimization | 1 | 7 | Mengubah atau menguji mekanisme attention dan komponen bottleneck. P025 menguji CBAM, sementara P002 mendefinisikan UIB dan Mobile MQA. |
| Model Compression | 2 | 14 | Mengurangi penyimpanan atau beban komputasi melalui pruning, distilasi, quantization, atau kombinasinya. P015 dan P026 menilai kombinasi sebelum atau untuk deployment. |
| On-Device Benchmarking | 0 | 12 | Memuat evaluasi inference atau profiling yang dilaporkan pada perangkat mobile/edge atau FPGA target. Pelaporan emulator, desktop, simulasi, dan FPGA dibedakan ketika menilai transfer hasil. |
| Multi-Objective Optimization | 0 | 11 | Menggunakan lebih dari satu sasaran atau kendala untuk memilih konfigurasi. P007 menggabungkan akurasi, latensi dan parameter, P020 mempertimbangkan akurasi dan latensi, P029 membatasi FLOPs dan latensi sambil mempertahankan akurasi. |
| Foundational Architecture | 1 | 1 | Sumber primer yang memperkenalkan generasi MobileNet. P002 memperkenalkan MobileNetV4. Paper dasar MobileNetV3 belum tersedia sebagai PDF lokal. |
| Supporting Methods | 6 | 15 | Survei, profiling, akselerator, atau pendekatan di domain lain yang memberi metode dan batas pembanding. P003 menguji offloading, P004 menyediakan benchmark arsitektur lintas task, dan P021 mencontohkan estimasi memori GNN. |
| Other | 1 | 1 | Aplikasi model dengan evaluasi yang berfokus pada task. P011 menggunakan MobileNetV4-Large untuk klasifikasi histopatologi. |

## Hubungan antartema

- **Arsitektur dan pencarian.** Desain dasar memberi kandidat blok dan baseline. P007 mengubah UIB dalam search space, sedangkan P017 menambahkan atau mengganti komponen secara eksplisit melalui ablation. Lihat [[02-Literature/Paper-Reviews/P002]], [[02-Literature/Paper-Reviews/P007]], dan [[02-Literature/Paper-Reviews/P017]].
- **Kompresi dan dukungan operator.** Structured pruning dapat menghapus kanal atau mengatur pola bobot, sedangkan quantization mengganti representasi numerik. Kedua teknik dapat berinteraksi, seperti urutan quantization lalu pruning N:M pada P014 dan kompresi hybrid pada P029. P009 dan P022 menunjukkan ketergantungan hasil akselerator pada implementasi depthwise/pointwise. Lihat [[02-Literature/Paper-Reviews/P014]], [[02-Literature/Paper-Reviews/P029]], [[02-Literature/Paper-Reviews/P009]], dan [[02-Literature/Paper-Reviews/P022]].
- **Objektif dan pengukuran.** Benchmark memberi label atau tolok ukur untuk search. P020 memakai prediktor latensi, P019 memakai tabel lookup dari pengukuran, dan P002 membandingkan model biaya dengan beberapa backend mobile. Evaluasi device dan batas transfer dibahas di [[03-Synthesis/PRELIMINARY_LITERATURE_MAP]]. Lihat [[02-Literature/Paper-Reviews/P020]], [[02-Literature/Paper-Reviews/P019]], dan [[02-Literature/Paper-Reviews/P002]].
