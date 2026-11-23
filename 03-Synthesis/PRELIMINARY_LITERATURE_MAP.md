---
type: literature-map
status: preliminary-screening
updated: 2026-11-23
tags: [mobilenet, architecture-optimization, hardware-aware, literature-review]
---

# Peta literatur awal MobileNet

Peta ini merangkum isi yang diperiksa pada screening 29 PDF lokal. Penelitian aplikasi, survei, eksperimen GPU desktop, dan benchmark pada SoC mobile memberi jenis bukti yang berbeda. Status per paper ada di [[02-Literature/Literature-Matrix/SCREENING_MATRIX]] dan pengelompokan lengkap ada di [[03-Synthesis/RESEARCH_TAXONOMY]].

## Perkembangan arsitektur dan sumber dasar

| Tahap | Yang dilaporkan sumber | Implikasi bagi review berikutnya |
| --- | --- | --- |
| MobileNetV1/V2 | Survei P004 menjelaskan depthwise separable convolution, lalu proposal inverted residual dan linear bottleneck pada V2. P009 mengevaluasi beban depthwise/pointwise di FPGA, sedangkan P006 dan P012 memakai V2 untuk pruning atau quantization. [[02-Literature/Paper-Reviews/P004]] · [[02-Literature/Paper-Reviews/P009]] · [[02-Literature/Paper-Reviews/P006]] · [[02-Literature/Paper-Reviews/P012]] | Pelajari operator yang tetap digunakan dalam generasi berikutnya dan pisahkan temuan tiap runtime. |
| MobileNetV3 | P004 merangkum bottleneck dengan squeeze-and-excitation dan aktivasi hard-swish. P029 menggunakan V3 sebagai baseline pruning pada radar. `Searching for MobileNetV3` (Howard dkk., 2019, arXiv:1905.02244) tercantum pada referensi [19] P002. PDF sumber dasar itu belum ada di inventaris 29 berkas. [[02-Literature/Paper-Reviews/P004]] · [[02-Literature/Paper-Reviews/P029]] · [[02-Literature/Paper-Reviews/P002]] | Gunakan P004 sebagai orientasi sekunder dan P029 untuk metode pruning. Periksa sumber primer V3 bila salinan lokal tersedia. |
| MobileNetV4 | P002 dalam versi preprint v2 memperkenalkan Universal Inverted Bottleneck (UIB), Mobile MQA, NAS dan distilasi, serta membandingkan backend mobile. P007 memakai UIB V4-Conv-Large sebagai ruang pencarian lokal. [[02-Literature/Paper-Reviews/P002]] · [[02-Literature/Paper-Reviews/P007]] | Mulai dari P002 untuk konfigurasi baseline, resolusi input, jenis presisi, dan target benchmark. |
| Adaptasi V4 | P001 menggabungkan DSA, Fused ConvBlock dan channel pruning. P017 menguji SimAM, Ghost convolution dan Mish pada V4-Small. P028 menguji LocalAttention dan PConv, termasuk pengujian Raspberry Pi 4B. P025 membandingkan strategi training dan CBAM lintas empat backbone ringan. [[02-Literature/Paper-Reviews/P001]] · [[02-Literature/Paper-Reviews/P017]] · [[02-Literature/Paper-Reviews/P028]] · [[02-Literature/Paper-Reviews/P025]] | Bandingkan perubahan blok yang setara. Tugas dan konfigurasi tiap paper menentukan sejauh mana hasilnya dapat dipindahkan. |

## Hubungan metode optimasi

- **Pruning.** P001 mengurangi kanal menggunakan sensitivitas yang dikaitkan dengan layer dan bobot BatchNorm. P029 menggunakan NAS bisection untuk pencarian struktur lalu menambahkan kompresi hybrid. P010 dan P009 mengaitkan sparsity dengan pemanfaatan processing elements pada akselerator. Struktur yang hilang dari tensor, pola bobot nol, dan dukungan kernel perlu dicatat dalam review. [[02-Literature/Paper-Reviews/P001]] · [[02-Literature/Paper-Reviews/P029]] · [[02-Literature/Paper-Reviews/P010]] · [[02-Literature/Paper-Reviews/P009]]
- **Quantization.** P012 mengevaluasi INT8 pada SSD MobileNetV2. P008 mencari bit-width menurut sensitivitas layer, sedangkan P022 menggabungkan INT4/INT8 dengan desain buffer dan DSP. P014 melakukan quantization sebelum memaksakan sparsity N:M. Urutan perubahan, export graph, dan operator yang didukung berpengaruh pada cara membandingkan konfigurasi ini. [[02-Literature/Paper-Reviews/P012]] · [[02-Literature/Paper-Reviews/P008]] · [[02-Literature/Paper-Reviews/P022]] · [[02-Literature/Paper-Reviews/P014]]
- **NAS dan hardware-aware optimization.** P007 menambahkan latensi dan parameter ke reward ruang pencarian V4. HL-DARTS P020 memasukkan prediksi latensi terdiferensiasi. P019 membangun lookup table dari pengukuran operator Raspberry Pi pada ConvNet 1D. P023 memvalidasi prediktor terhadap data NAS-Bench-201 dari GPU desktop. P002 menyediakan contoh pembandingan biaya dan latensi lintas backend pada keluarga MobileNet. [[02-Literature/Paper-Reviews/P007]] · [[02-Literature/Paper-Reviews/P020]] · [[02-Literature/Paper-Reviews/P019]] · [[02-Literature/Paper-Reviews/P023]] · [[02-Literature/Paper-Reviews/P002]]

## Metrik dan batas perbandingan

| Bukti | Perbedaan yang terlihat saat screening | Data yang perlu diambil pada full review |
| --- | --- | --- |
| Akurasi atau kualitas model | P002 memakai ImageNet-1K top-1. P028 melaporkan akurasi dan F1 untuk inspeksi. P026 menggunakan mAP pada deteksi. [[02-Literature/Paper-Reviews/P002]] · [[02-Literature/Paper-Reviews/P028]] · [[02-Literature/Paper-Reviews/P026]] | Task, dataset, split, preprocessing, varian, performa baseline, dan ketidakpastian. |
| Operasi dan latensi | Appendix F P002 menghitung korelasi MACs dan latensi terukur per backend. Table 17 P004 memperlihatkan urutan FLOPs yang berbeda dari urutan latensi di RTX 4090. P029 melaporkan hubungan yang mendekati linear pada setup tetap. [[02-Literature/Paper-Reviews/P002]] · [[02-Literature/Paper-Reviews/P004]] · [[02-Literature/Paper-Reviews/P029]] | Konvensi FLOPs/MACs, operator, presisi, backend, batch, warm-up, pengulangan, dan sebaran latensi. |
| Throughput dan latensi end-to-end | P028 membedakan forward pass dari throughput pipeline yang juga memuat preprocessing dan komunikasi PLC. Waktu total P012 terdiri dari komponen berlabel image dan transfer learning. P025 memprofilkan di GPU, dengan aplikasi edge yang dibahas sebagai target. [[02-Literature/Paper-Reviews/P028]] · [[02-Literature/Paper-Reviews/P012]] · [[02-Literature/Paper-Reviews/P025]] | Batas pengukuran, latensi forward dan end-to-end, batch, concurrency, thermal state, serta pemisahan host dan perangkat target. |
| Ukuran model dan memori | P001 melaporkan ukuran file dan parameter. P012 memisahkan RAM dan flash. P029 menjelaskan bahwa ukuran bobot belum mencakup aktivasi dan buffer SRAM. [[02-Literature/Paper-Reviews/P001]] · [[02-Literature/Paper-Reviews/P012]] · [[02-Literature/Paper-Reviews/P029]] | Format model, ukuran bobot, RAM puncak proses atau buffer runtime, presisi, satuan, dan cara pengukuran. |

**Interpretasi sementara.** Perbandingan yang paling dekat dengan fokus tesis adalah modifikasi blok atau pruning pada V3/V4 dengan baseline yang sama, disertai pengukuran langsung lintas backend. Korpus menyediakan contoh perangkat nyata, estimasi, hasil sintesis FPGA, dan profiling desktop. Hasil screening belum menyediakan perbandingan kuantitatif lintas studi dengan protokol yang seragam. Mulai dari [[02-Literature/Reading-Queue/PRIORITY_READING_LIST]], dan telusuri signifikansi batas pengukuran dalam [[03-Synthesis/Research-Gaps/PRELIMINARY_GAPS]].
