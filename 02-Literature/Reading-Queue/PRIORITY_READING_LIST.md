---
type: priority-reading-list
updated: 2026-11-23
tags: [mobilenet, literature-review, thesis-progress]
---

# Prioritas full review

Shortlist memuat **15 paper** dari [[02-Literature/Literature-Matrix/SCREENING_MATRIX]]. Tier 1 (7 paper) memberi landasan dan pembanding langsung MobileNetV3/V4. Tier 2 (8 paper) melengkapi metode atau protokol pengukuran. Tier 3 mencatat opsi cadangan dan satu keputusan eksklusi. Urutan memperhitungkan konteks tiap tier, terutama kedekatan dengan V3/V4 dan metode yang belum punya pembanding di Tier 1. Nilai merupakan prioritas baca berdasarkan informasi screening.

## Rubrik

| Kriteria | Bobot | Sinyal K (kuat) | Sinyal C (cukup) |
| --- | ---: | --- | --- |
| MobileNet (M) | 30% | Studi langsung MobileNetV3/V4. | MobileNet generasi lain atau metode yang jelas dapat dibandingkan. |
| Kontribusi metode (T) | 25% | Perubahan arsitektur, metode pencarian, atau kompresi yang dapat dipelajari. | Aplikasi metode yang ada, ablation atau perbandingan yang dapat digunakan. |
| Efisiensi hardware (H) | 20% | Pengujian pada target mobile atau edge yang relevan dilaporkan. | GPU desktop, proksi biaya, simulator atau akselerator khusus dengan nilai pembanding. |
| Informasi eksperimen (E) | 15% | Kondisi, pembanding atau prosedur evaluasi cukup terperinci untuk ditelusuri. | Evaluasi dan metrik terlihat, dengan detail yang masih perlu dikonfirmasi. |
| Landasan teori (L) | 10% | Mengusulkan arsitektur dasar yang menjadi fokus. | Memberi baseline, mekanisme, atau protokol pembanding penting. |

Setiap K memakai seluruh bobot, C setengah, dan R (rendah) nol. Tanda `?` menandai bukti yang belum cukup dan memberi rentang dari nol sampai bobot kriteria itu. Skor nonkelipatan lima ditampilkan sebagai perkiraan kelipatan lima. Batas rentang dibulatkan keluar. Urutan tabel adalah urutan baca yang diusulkan, dengan prioritas pada studi langsung di Tier 1 ketika skor antar-tier beririsan.

## Tier 1 — Essential

| Paper ID | M/T/H/E/L | Skor indikatif /100 | Dasar prioritas |
| --- | --- | ---: | --- |
| [[02-Literature/Paper-Reviews/P002]] | K/K/K/K/K | 100 | Sumber primer versi lokal. UIB, Mobile MQA, NAS dan benchmark pada beberapa backend mobile mendefinisikan baseline. |
| [[02-Literature/Paper-Reviews/P028]] | K/K/K/K/C | 95 | Modifikasi V4 dengan ablation dan deployment Raspberry Pi 4B. Konfigurasi eksperimen dan stress test memberi bahan review protokol. |
| [[02-Literature/Paper-Reviews/P029]] | K/K/K/C/C | ≈90 | MobileNetV3 masuk eksperimen pruning blok/kanal. Table 6 melaporkan waktu target MCU dan diskusi mencatat batas generalisasi. |
| [[02-Literature/Paper-Reviews/P001]] | K/K/C/C/C | ≈80 | Menggabungkan perubahan blok V4 dan channel pruning, dengan pembanding, rasio pruning, serta model size. Timing di perangkat edge masih perlu dicek. |
| [[02-Literature/Paper-Reviews/P017]] | K/K/C/C/C | ≈80 | Ablation SimAM, Ghost convolution dan Mish pada V4-Small memberi pembanding kontribusi blok. Profil berasal dari workstation. |
| [[02-Literature/Paper-Reviews/P007]] | K/K/?/C/C | 65–90 | Mencari konfigurasi UIB dengan reward terikat latensi dan anggaran evaluasi. Perangkat timing belum terverifikasi. |
| [[02-Literature/Paper-Reviews/P025]] | K/C/C/K/C | ≈75 | Studi attention dan transfer learning multi-seed memuat perbandingan V4-Small. Timing GPU dengan warm-up dan pengulangan berguna untuk audit benchmark. |

## Tier 2 — Supporting

| Paper ID | M/T/H/E/L | Skor indikatif /100 | Dasar prioritas |
| --- | --- | ---: | --- |
| [[02-Literature/Paper-Reviews/P012]] | C/C/K/K/C | ≈70 | MobileNetV2 INT8 punya tabel RAM/flash dan waktu lintas perangkat, dengan definisi waktu total yang perlu diaudit. |
| [[02-Literature/Paper-Reviews/P024]] | C/K/K/K/C | 80 | Pembanding quantization dan memori pada CNN ringan. Studi membedakan ponsel Huawei fisik dan satu perangkat virtual. |
| [[02-Literature/Paper-Reviews/P004]] | K/C/C/K/C | ≈75 | Survei sekaligus benchmark V3/V4 pada RTX 4090. Table 17 langsung menguji relasi FLOPs dan timing pada satu setup. |
| [[02-Literature/Paper-Reviews/P020]] | C/K/C/K/C | 70 | Prediktor dan loss latensi diuji lintas CPU/GPU untuk memberi pembanding hardware-aware. Target utamanya desktop. |
| [[02-Literature/Paper-Reviews/P009]] | C/K/C/K/C | 70 | Co-design pada MobileNetV1 dan demo ZCU104 membantu memahami rasio beban depthwise/pointwise dan biaya pruning. |
| [[02-Literature/Paper-Reviews/P010]] | C/K/C/K/C | 70 | Cross-filter pruning menyediakan perbandingan sparsity, simulasi siklus, dan sintesis PE. Berguna untuk memisahkan speedup terlapor dan deployment nyata. |
| [[02-Literature/Paper-Reviews/P008]] | C/K/C/C/C | ≈65 | Mixed precision memakai sensitivitas lapisan dan membandingkan MobileNetV2 dengan HAQ. Biaya hardware terutama perlu dilacak dari proksi. |
| [[02-Literature/Paper-Reviews/P006]] | C/K/?/C/C | 50–75 | Distilasi dan structured pruning MobileNetV2 dapat diperiksa dari ablation CIFAR-10. Perangkat untuk Fig. 4 tidak disebutkan. |

## Tier 3 — Optional

| Paper ID | Kapan ditinjau |
| --- | --- |
| [[02-Literature/Paper-Reviews/P003]] | Profil offloading dua perangkat berguna bila batas sistem mencakup biaya komunikasi. |
| [[02-Literature/Paper-Reviews/P005]] | Akselerator VLSI memberi konteks daya dan memori. Perlu menelusuri konfigurasi CNN uji. |
| [[02-Literature/Paper-Reviews/P011]] | EXCLUDE E2. Simpan rekam keputusan. Bagian evaluasi berfokus pada klasifikasi aplikasi. |
| [[02-Literature/Paper-Reviews/P013]] | Survei awal untuk menelusuri sumber akselerasi dan codesign primer. |
| [[02-Literature/Paper-Reviews/P014]] | Backlog INCLUDE untuk skenario codesign N:M/INT4 dan FPGA. |
| [[02-Literature/Paper-Reviews/P015]] | Estimasi sumber daya dari LeNet-5 dapat ditinjau jika merancang pemeriksaan sebelum deployment. |
| [[02-Literature/Paper-Reviews/P016]] | Survei baru untuk menelusuri compiler, runtime dan sparse kernels. |
| [[02-Literature/Paper-Reviews/P018]] | Rancang profil multiobjektif dari analogi NAS GNN bila relevan bagi metode. |
| [[02-Literature/Paper-Reviews/P019]] | Contoh pengukuran lookup table per perangkat pada ConvNet 1D. |
| [[02-Literature/Paper-Reviews/P021]] | Metode pengukuran dan prediksi peak memory pada GNN dapat menjadi rujukan protokol. |
| [[02-Literature/Paper-Reviews/P022]] | Backlog INCLUDE untuk mixed precision FPGA. Gunakan locator bagian saat evaluasi. |
| [[02-Literature/Paper-Reviews/P023]] | Backlog INCLUDE jika memerlukan pembanding prediktor latensi dan quality ranking. |
| [[02-Literature/Paper-Reviews/P026]] | Backlog INCLUDE untuk alur kompresi gabungan pada deteksi berbasis YOLO. |
| [[02-Literature/Paper-Reviews/P027]] | Bandingkan variasi head klasifikasi jika fokus tesis mengarah ke bagian tersebut. |

Setelah full review, masukkan data terverifikasi ke [[02-Literature/Literature-Matrix/LITERATURE_MATRIX]]. [[02-Literature/Reading-Queue/READING_QUEUE]] menyimpan urutan inventaris awal dan tautan ke antrean yang berlaku.
