---
type: critical-evidence-matrix
scope: [P002, P028, P001, P029]
updated: 2026-11-23
tags: [mobilenet, literature-review, hardware-aware]
---

# Matriks bukti kritis

Satu baris per konfigurasi model × perangkat atau kondisi eksperimen yang dipilih. Untuk varian utama, matriks mencakup seluruh perangkat Table 6 P002 dan seluruh kondisi Table 7 P028. Baris lain mencakup pembanding yang membantu interpretasi, eksperimen search, distilasi, detektor, attention, robustness, dan deployment. Data agregat yang tidak dapat dipasangkan dengan konfigurasi eksekusi, seperti statistik korelasi roofline, tetap di review sumber. Angka yang muncul kembali pada tabel lain ditautkan tanpa menduplikasi konfigurasi identik. Titik desimal pada tabel mengikuti notasi PDF.

`NR` = Not reported pada evidence yang dicatat. `NV` = Not verified. `VL` = Verified locally untuk transkripsi hasil tersebut. `PV` = Partially verified, dengan hubungan checkpoint, scope atau konfigurasi yang belum tuntas. VL belum berarti replikasi. Kolom Evidence merujuk claim register di [[02-Literature/Paper-Reviews/P002#Evidence references]] dan [[02-Literature/Paper-Reviews/P028#Evidence references]]. Angka adalah angka penulis kecuali estimasi visual memori yang diberi label. `T` berarti Table di PDF dan `PDF` adalah ordinal 1-based.

## Profil pengukuran dan dataset

Kode di kolom Profil merujuk parameter di bawah. Bentuk input dalam tanda kurung di baris P002 adalah spesifikasi model dari Appendix D, kecuali ditandai NR. Detail runtime yang dilaporkan secara umum berlaku hanya pada profil yang menyatakannya.

| Profil | Dataset/split dan input | Framework, backend, precision | Batch inferensi, thread, warm-up, repetisi dan statistik | Batas pengukuran dan sumber |
| --- | --- | --- | --- | --- |
| A-CLS | ImageNet-1K val Top-1. RGB per varian menurut Appendix D. | TFLite INT8 (CPU XNNPACK, Pixel 4 Hexagon, Pixel 8 EdgeTPU), TFLite FP16 (GPU Pixel 7/S23). CPU affinity ke core tercepat. Versi package/driver NR. | Batch, thread dan warm-up NR di metode benchmark. Sekitar 1.000 run per kelompok × 5, median dari mean per kelompok. | **Unclear / Not reported** untuk timer input-copy/pre/post. Model on-device. T6, Appendix B (PDF 11, 19). |
| A-ANE | Seperti A-CLS. | PyTorch ke Core ML MLProgram, float16 model dan input, ANE iPhone 13. iOS 16.6.1, CoreMLTools 7.1, Xcode 15.0.1. | Batch/thread/warm-up NR. Repetisi dan statistik khusus ANE NV karena Appendix B membedakan metode. T6 tetap berlabel median. | **Unclear / Not reported**. Upaya mengurangi copy dijelaskan, cakupan timer NV. PDF 11, 19. |
| A-ABL | Ablasi arsitektur klasifikasi P002. Crop resolusi dan split per tabel NV. | Backend family mengikuti target, presisi per konfigurasi NV. | Batch, thread, warm-up, jumlah repetisi dan statistik per tabel NR. | **Unclear / Not reported** untuk timer input/output. Nilai ms T2/T17 untuk model. Persentase incremental attention punya denominator terpisah dan bukan profil operator langsung. PDF 7–8, 31. |
| A-SEARCH | Evaluasi varian search TuNAS, T4/T5 val dan train. Input masing-masing baris NR. | Perangkat sesuai baris. Presisi, khususnya Pixel 4 GPU, NR dalam tabel. | Batch, thread, warm-up, jumlah repetisi dan statistik spesifik T4/T5 NR. | **Unclear / Not reported**. Timing search tidak digabung dengan hasil klasifikasi T6. PDF 9–10. |
| A-DET | COCO 2017 val AP, input 384 px. | RetinaNet / FPN di Pixel 6 CPU. Profil umum CPU memakai TFLite INT8/XNNPACK. Pengaturan khusus detektor NV. | Batch inferensi, thread, warm-up, repetisi dan statistik spesifik T7 NR. | **Unclear / Not reported** untuk decoding/NMS. Parameter, MACs dan timing berlaku bagi detektor utuh. PDF 12–13, 19. |
| A-KD | ImageNet-1K val Top-1, training tambahan sesuai resep di §8 dan Tables 8–9. | Training AdamW dengan soft labels. Framework/backend, precision dan artefak timing per checkpoint NR. | NR untuk benchmark checkpoint distilasi. | **Unclear / Not reported**. Angka latency ulang untuk checkpoint 87.0% NV. PDF 13–14, 31–32. |
| B-CLS | Citra kemasan, 80:20 train/held-out (disebut validation/test), 224² RGB. | RTX 4090/Windows, PyTorch 1.13.1, CUDA 11.7, Python 3.9.18 untuk setup umum. Precision eval NR. | Batch training 128. Batch inferensi, thread, warm-up inferensi, pengulangan dan statistik timing NR. | Timing NR untuk baris ini. PDF 10–13, 18–19. |
| B-FWD | Konteks klasifikasi kemasan, arsitektur input 224². Reuse split untuk timing NV. | Pi 4B. CPU/GPU/delegate aktual, inference engine dan presisi benchmark NR. | Satu citra/inferensi terisolasi. Threads, warm-up, repetisi dan statistik latency NR. | **Model forward-pass only**. T7–9 dan penjelasan di PDF 23, FPS laju forward per citra. |
| B-NEU | NEU-CLS 1.800 grayscale, enam kelas. Split dan bentuk tensor evaluasi NV. | Protokol training sama antarbaselines menurut penulis. Perangkat/runtime/precision untuk baris NR. | NR. | Timing NR. PDF 14–16. |
| B-PAIR | Kemasan, perbandingan Ours vs PIB+ASML, mean lima run training. | Setup spesifik per run, runtime dan presisi NV. | Lima pasangan training run. Repetisi timing NR. | Timing NR. Statistik t dan batasnya di review. PDF 17. |
| B-ROB | Kemasan holdout, input terganggu pada kondisi di baris. Top-1 accuracy. | Perangkat pengujian gangguan dan precision per kondisi NR. | Parameter inference dan runtime per kondisi NR. | Timing NR. PDF 21–23. |
| B-PIPE | Full-load pipeline pada stress test. Dataset/sequence dan resolusi per frame untuk uji ini NV. | Raspberry Pi 4B dengan heatsink dan fan. ONNX digambarkan pada deployment, engine/version/provider/presisi untuk stress test NR. | Logging tiap 0,5 s selama 8 jam. Warm-up awal, durasi NR. Batch/thread NR. Mean FPS, SD dan P5–P95 throughput. | **Full application pipeline**. Preprocessing dan komunikasi PLC masuk. Batas timestamp untuk akuisisi/aktuasi NV. PDF 23–25. |
| B-AMB | Deployment inspeksi satu citra, workload NV. | Pi 4B sebagai perangkat sistem. ONNX di desain. Runtime/precision timer NV. | Batch/thread/warm-up/repetisi dan statistik NR. | **Unclear / Not reported**. Scalar single-image processing 39 ms. PDF 23. |

Model size dalam byte dan energi per inferensi `NR` untuk konfigurasi yang dicatat. Peak RAM model juga `NR`. Fig. 16(c) P028 menunjukkan tren memory usage tingkat sistem/lingkup belum jelas dan tidak menjadi peak RAM. Kolom throughput hanya menampilkan FPS ketika penulis melaporkannya. Definisi dan pemisahan metrik tersedia di [[docs/METRIC_DEFINITIONS]].

## P002 — konfigurasi yang diekstrak

| ID | Model atau konfigurasi | Profil dan input bila tersedia | Hardware | Akurasi / kualitas | Parameter / kompleksitas | Latensi atau throughput | Evidence dan halaman PDF | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| P002-E001 | Conv-S | A-CLS (224²) | Pixel 6 CPU | 73.8% Top-1 | 3.8 M / 0.2 GMAC | 2.4 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E002 | Conv-S | A-CLS (224²) | Pixel 8 EdgeTPU | 73.8% Top-1 | 3.8 M / 0.2 GMAC | 0.7 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E003 | Conv-S | A-ANE (224²) | iPhone 13 ANE | 73.8% Top-1 | 3.8 M / 0.2 GMAC | 0.6 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | PV |
| P002-E004 | Conv-S | A-CLS (224²) | Pixel 4 DSP | 73.8% Top-1 | 3.8 M / 0.2 GMAC | 2.4 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E005 | Conv-S | A-CLS (224²) | Pixel 7 GPU | 73.8% Top-1 | 3.8 M / 0.2 GMAC | 8.4 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E006 | Conv-S | A-CLS (224²) | Galaxy S23 CPU | 73.8% Top-1 | 3.8 M / 0.2 GMAC | 1.8 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E007 | Conv-S | A-CLS (224²) | Galaxy S23 GPU | 73.8% Top-1 | 3.8 M / 0.2 GMAC | 2.0 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E008 | Conv-M | A-CLS (256²) | Pixel 6 CPU | 79.9% Top-1 | 9.2 M / 1.0 GMAC | 11.4 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E009 | Conv-M | A-CLS (256²) | Pixel 8 EdgeTPU | 79.9% Top-1 | 9.2 M / 1.0 GMAC | 1.1 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E010 | Conv-M | A-ANE (256²) | iPhone 13 ANE | 79.9% Top-1 | 9.2 M / 1.0 GMAC | 1.1 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | PV |
| P002-E011 | Conv-M | A-CLS (256²) | Pixel 4 DSP | 79.9% Top-1 | 9.2 M / 1.0 GMAC | 7.3 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E012 | Conv-M | A-CLS (256²) | Pixel 7 GPU | 79.9% Top-1 | 9.2 M / 1.0 GMAC | 18.1 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E013 | Conv-M | A-CLS (256²) | Galaxy S23 CPU | 79.9% Top-1 | 9.2 M / 1.0 GMAC | 8.6 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E014 | Conv-M | A-CLS (256²) | Galaxy S23 GPU | 79.9% Top-1 | 9.2 M / 1.0 GMAC | 4.1 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E015 | Hybrid-M | A-CLS (256²) | Pixel 6 CPU | 80.7% Top-1 | 10.5 M / 1.2 GMAC | 14.3 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E016 | Hybrid-M | A-CLS (256²) | Pixel 8 EdgeTPU | 80.7% Top-1 | 10.5 M / 1.2 GMAC | 1.5 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E017 | Hybrid-M | A-ANE (256²) | iPhone 13 ANE | 80.7% Top-1 | 10.5 M / 1.2 GMAC | Model file tidak tersedia untuk benchmark | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E018 | Hybrid-M | A-CLS (256²) | Pixel 4 DSP | 80.7% Top-1 | 10.5 M / 1.2 GMAC | Target tidak mendukung konfigurasi | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E019 | Hybrid-M | A-CLS (256²) | Pixel 7 GPU | 80.7% Top-1 | 10.5 M / 1.2 GMAC | 17.9 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E020 | Hybrid-M | A-CLS (256²) | Galaxy S23 CPU | 80.7% Top-1 | 10.5 M / 1.2 GMAC | 10.8 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E021 | Hybrid-M | A-CLS (256²) | Galaxy S23 GPU | 80.7% Top-1 | 10.5 M / 1.2 GMAC | 5.9 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E022 | Conv-L | A-CLS (384²) | Pixel 6 CPU | 82.9% Top-1 | 31 M / 5.9 GMAC | 59.9 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E023 | Conv-L | A-CLS (384²) | Pixel 8 EdgeTPU | 82.9% Top-1 | 31 M / 5.9 GMAC | 2.4 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E024 | Conv-L | A-ANE (384²) | iPhone 13 ANE | 82.9% Top-1 | 31 M / 5.9 GMAC | 3.0 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | PV |
| P002-E025 | Conv-L | A-CLS (384²) | Pixel 4 DSP | 82.9% Top-1 | 31 M / 5.9 GMAC | 20.8 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E026 | Conv-L | A-CLS (384²) | Pixel 7 GPU | 82.9% Top-1 | 31 M / 5.9 GMAC | 37.6 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E027 | Conv-L | A-CLS (384²) | Galaxy S23 CPU | 82.9% Top-1 | 31 M / 5.9 GMAC | 43.0 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E028 | Conv-L | A-CLS (384²) | Galaxy S23 GPU | 82.9% Top-1 | 31 M / 5.9 GMAC | 13.2 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E029 | Hybrid-L | A-CLS (384²) | Pixel 6 CPU | 83.4% Top-1 | 35.9 M / 7.2 GMAC | 87.6 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E030 | Hybrid-L | A-CLS (384²) | Pixel 8 EdgeTPU | 83.4% Top-1 | 35.9 M / 7.2 GMAC | 3.8 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E031 | Hybrid-L | A-ANE (384²) | iPhone 13 ANE | 83.4% Top-1 | 35.9 M / 7.2 GMAC | Model file tidak tersedia untuk benchmark | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E032 | Hybrid-L | A-CLS (384²) | Pixel 4 DSP | 83.4% Top-1 | 35.9 M / 7.2 GMAC | Target tidak mendukung konfigurasi | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E033 | Hybrid-L | A-CLS (384²) | Pixel 7 GPU | 83.4% Top-1 | 35.9 M / 7.2 GMAC | 61.3 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E034 | Hybrid-L | A-CLS (384²) | Galaxy S23 CPU | 83.4% Top-1 | 35.9 M / 7.2 GMAC | 61.8 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E035 | Hybrid-L | A-CLS (384²) | Galaxy S23 GPU | 83.4% Top-1 | 35.9 M / 7.2 GMAC | 18.1 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E036 | MobileNet-V2-0.5× | A-CLS (NR) | Pixel 6 CPU | 66% Top-1 | 2.0 M / 0.1 GMAC | 2.4 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E037 | MobileNet-V2-0.5× | A-CLS (NR) | Pixel 8 EdgeTPU | 66% Top-1 | 2.0 M / 0.1 GMAC | 0.7 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E038 | MobileNet-V2-0.5× | A-CLS (NR) | Galaxy S23 GPU | 66% Top-1 | 2.0 M / 0.1 GMAC | 1.9 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E039 | MobileNet-V3L-0.5× | A-CLS (NR) | Pixel 6 CPU | 69.2% Top-1 | 2.7 M / 0.1 GMAC | 2.4 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E040 | MobileNet-V3L-0.5× | A-CLS (NR) | Pixel 8 EdgeTPU | 69.2% Top-1 | 2.7 M / 0.1 GMAC | 0.8 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E041 | MobileNet-V3L-0.5× | A-CLS (NR) | Galaxy S23 GPU | 69.2% Top-1 | 2.7 M / 0.1 GMAC | 2.1 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E042 | MobileOne-S4 | A-CLS (NR) | Pixel 6 CPU | 79.4% Top-1 | 14.8 M / 1.5 GMAC | 26.7 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E043 | MobileOne-S4 | A-CLS (NR) | Pixel 8 EdgeTPU | 79.4% Top-1 | 14.8 M / 1.5 GMAC | 1.7 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E044 | MobileOne-S4 | A-CLS (NR) | Galaxy S23 GPU | 79.4% Top-1 | 14.8 M / 1.5 GMAC | 5.9 ms | P002-C010/P002-C011 · T6, PDF 11, 19 | VL |
| P002-E045 | Search block IB | A-ABL | Pixel 8 EdgeTPU | 82.3% Top-1 | 32.4 M / 6.1 GMAC | 2.61 ms | P002-C003 · T1, PDF 7 | VL |
| P002-E046 | Search block ConvNeXt-like | A-ABL | Pixel 8 EdgeTPU | 83.2% Top-1 | 35.1 M / 6.9 GMAC | 2.69 ms | P002-C003 · T1, PDF 7 | VL |
| P002-E047 | Search block UIB | A-ABL | Pixel 8 EdgeTPU | 83.3% Top-1 | 33.0 M / 6.2 GMAC | 2.68 ms | P002-C003 · T1, PDF 7 | VL |
| P002-E048 | Conv-L base | A-ABL | Pixel 7 EdgeTPU | 84.88% Top-1 | 30.9 M / 6.0 GMAC | 4.31 ms (model) | P002-C004 · T2, PDF 8 · angka berulang di T17, PDF 31 (P002-C007) | VL |
| P002-E049 | Conv-L base | A-ABL | Pixel 8 EdgeTPU | 84.88% Top-1 | 30.9 M / 6.0 GMAC | 2.35 ms (model) | P002-C004 · T2, PDF 8 · angka berulang di T17, PDF 31 (P002-C007) | VL |
| P002-E050 | Conv-L base | A-ABL | Galaxy S23 GPU | 84.88% Top-1 | 30.9 M / 6.0 GMAC | 13.15 ms (model) | P002-C004 · T2, PDF 8 | VL |
| P002-E051 | Conv-L +3 MHSA | A-ABL | Pixel 7 EdgeTPU | 85.27% Top-1 | 36.0 M / 6.7 GMAC | 9.69 ms (model) | P002-C004 · T2, PDF 8 | VL |
| P002-E052 | Conv-L +3 MHSA | A-ABL | Pixel 8 EdgeTPU | 85.27% Top-1 | 36.0 M / 6.7 GMAC | 2.76 ms (model) | P002-C004 · T2, PDF 8 | VL |
| P002-E053 | Conv-L +3 MHSA | A-ABL | Galaxy S23 GPU | 85.27% Top-1 | 36.0 M / 6.7 GMAC | 16.46 ms (model) | P002-C004 · T2, PDF 8 | VL |
| P002-E054 | Conv-L +3 MQA | A-ABL | Pixel 7 EdgeTPU | 85.24% Top-1 | 34.7 M / 6.5 GMAC | 5.16 ms (model) | P002-C004 · T2, PDF 8 · angka berulang di T17, PDF 31 (P002-C007) | VL |
| P002-E055 | Conv-L +3 MQA | A-ABL | Pixel 8 EdgeTPU | 85.24% Top-1 | 34.7 M / 6.5 GMAC | 2.60 ms (model) | P002-C004 · T2, PDF 8 · angka berulang di T17, PDF 31 (P002-C007) | VL |
| P002-E056 | Conv-L +3 MQA | A-ABL | Galaxy S23 GPU | 85.24% Top-1 | 34.7 M / 6.5 GMAC | 15.10 ms (model) | P002-C004 · T2, PDF 8 | VL |
| P002-E057 | Conv-L +3 MQA tanpa optimasi Einsum | A-ABL | Pixel 7 EdgeTPU | 85.22% Top-1 | 34.7 M / 6.5 GMAC | 7.00 ms (model) | P002-C007 · T17, PDF 31 | VL |
| P002-E058 | Conv-L +3 MQA tanpa optimasi Einsum | A-ABL | Pixel 8 EdgeTPU | 85.22% Top-1 | 34.7 M / 6.5 GMAC | 2.64 ms (model) | P002-C007 · T17, PDF 31 | VL |
| P002-E059 | Hybrid-M tanpa KV downsampling | A-ABL | Galaxy S23 CPU | 80.77% Top-1 | NR params / 1.285 GMAC | 15.8 ms (model) | P002-C006 · T3, PDF 8 | VL |
| P002-E060 | Hybrid-M tanpa KV downsampling | A-ABL | Galaxy S23 GPU | 80.77% Top-1 | NR params / 1.285 GMAC | 7.4 ms (model) | P002-C006 · T3, PDF 8 | VL |
| P002-E061 | Hybrid-M dengan KV downsampling | A-ABL | Galaxy S23 CPU | 80.71% Top-1 | NR params / 1.245 GMAC | 12.8 ms (model) | P002-C006 · T3, PDF 8 | VL |
| P002-E062 | Hybrid-M dengan KV downsampling | A-ABL | Galaxy S23 GPU | 80.71% Top-1 | NR params / 1.245 GMAC | 5.9 ms (model) | P002-C006 · T3, PDF 8 | VL |
| P002-E063 | TuNAS One-stage | A-SEARCH | Pixel 6 EdgeTPU | 81.26 / 74.64% Top-1 val/train | NR | 3.85 ms | P002-C008 · T4, PDF 9 | VL |
| P002-E064 | TuNAS Two-stage | A-SEARCH | Pixel 6 EdgeTPU | 81.48 / 78.24% Top-1 val/train | NR | 3.67 ms | P002-C008 · T4, PDF 9 | VL |
| P002-E065 | TuNAS data ImageNet | A-SEARCH | Pixel 4 GPU | 82.4 / 72.9% Top-1 val/train | 43.5 M / 7.2 GMAC | 59.2 ms | P002-C009 · T5, PDF 10 | VL |
| P002-E066 | TuNAS data ImageNet | A-SEARCH | Pixel 6 CPU | 82.4 / 72.9% Top-1 val/train | 43.5 M / 7.2 GMAC | 70.4 ms | P002-C009 · T5, PDF 10 | VL |
| P002-E067 | TuNAS data JFT distill | A-SEARCH | Pixel 4 GPU | 82.3 / 74.0% Top-1 val/train | 34.4 M / 6.2 GMAC | 51.0 ms | P002-C009 · T5, PDF 10 | VL |
| P002-E068 | TuNAS data JFT distill | A-SEARCH | Pixel 6 CPU | 82.3 / 74.0% Top-1 val/train | 34.4 M / 6.2 GMAC | 67.3 ms | P002-C009 · T5, PDF 10 | VL |
| P002-E069 | RetinaNet / EfficientFormer L1 | A-DET | Pixel 6 CPU | 29.5 AP | 12.77 M / 6.54 GMAC | 84.3 ms (detektor) | P002-C012 · T7, PDF 12–13 | VL |
| P002-E070 | RetinaNet / MobileNetV1 @1.5 | A-DET | Pixel 6 CPU | 31.0 AP | 9.05 M / 6.68 GMAC | 66.4 ms (detektor) | P002-C012 · T7, PDF 12–13 | VL |
| P002-E071 | RetinaNet / Conv-M | A-DET | Pixel 6 CPU | 32.6 AP | 9.79 M / 5.06 GMAC | 51.3 ms (detektor) | P002-C012 · T7, PDF 12–13 | VL |
| P002-E072 | RetinaNet / MobileNet Multi-AVG @1.5 | A-DET | Pixel 6 CPU | 32.7 AP | 9.51 M / 5.42 GMAC | 58.1 ms (detektor) | P002-C012 · T7, PDF 12–13 | VL |
| P002-E073 | RetinaNet / MobileNetV2 @2.0 | A-DET | Pixel 6 CPU | 32.9 AP | 10.15 M / 5.81 GMAC | 66.4 ms (detektor) | P002-C012 · T7, PDF 12–13 | VL |
| P002-E074 | RetinaNet / MobileNetV3 Large @2.0 | A-DET | Pixel 6 CPU | 33.2 AP | 17.92 M / 4.99 GMAC | 59.9 ms (detektor) | P002-C012 · T7, PDF 12–13 | VL |
| P002-E075 | RetinaNet / Hybrid-M | A-DET | Pixel 6 CPU | 34.0 AP | 11.15 M / 5.62 GMAC | 60.5 ms (detektor) | P002-C012 · T7, PDF 12–13 | VL |
| P002-E076 | Conv-S, distilasi usulan | A-KD (224²) | NR (eval) | 75.5% Top-1 | Lihat spesifikasi model, angka checkpoint NR | NR | P002-C013 · T9, PDF 14 | VL |
| P002-E077 | Conv-M, distilasi pembanding | A-KD (256²) | NR (eval) | 81.5% Top-1 | Lihat spesifikasi model, angka checkpoint NR | NR | P002-C013 · T9, PDF 14 | VL |
| P002-E078 | Conv-M, distilasi usulan | A-KD (256²) | NR (eval) | 82.7% Top-1 | Lihat spesifikasi model, angka checkpoint NR | NR | P002-C013 · T9, PDF 14 | VL |
| P002-E079 | Hybrid-M, distilasi pembanding | A-KD (256²) | NR (eval) | 82.7% Top-1 | Lihat spesifikasi model, angka checkpoint NR | NR | P002-C013 · T9, PDF 14 | VL |
| P002-E080 | Hybrid-M, distilasi usulan | A-KD (256²) | NR (eval) | 83.7% Top-1 | Lihat spesifikasi model, angka checkpoint NR | NR | P002-C013 · T9, PDF 14 | VL |
| P002-E081 | Conv-L, distilasi pembanding | A-KD (384²) | NR (eval) | 84.4% Top-1 | Lihat spesifikasi model, angka checkpoint NR | NR | P002-C013 · T9, PDF 14 | VL |
| P002-E082 | Conv-L, distilasi usulan | A-KD (384²) | NR (eval) | 85.9% Top-1 | Lihat spesifikasi model, angka checkpoint NR | NR | P002-C013 · T9, PDF 14 | VL |
| P002-E083 | Hybrid-L, distilasi pembanding | A-KD (384²) | NR (eval) | 85.7% Top-1 | Lihat spesifikasi model, angka checkpoint NR | NR | P002-C013 · T9, PDF 14 | VL |
| P002-E084 | Hybrid-L, distilasi usulan | A-KD (384²) | NR (eval) | 86.6% Top-1 | Lihat spesifikasi model, angka checkpoint NR | NR | P002-C013 · T9, PDF 14 | VL |
| P002-E085 | Hybrid-L, tambah JFT pretraining | A-KD (384²) | Pixel 8 EdgeTPU dalam klaim abstrak | 87.0% Top-1 | NR per checkpoint | 3.8 ms dalam abstrak. Timing ulang checkpoint ini NV. | P002-C014 · Abstract, T6, §8, PDF 1, 11, 13–14 | PV |

## P028 — konfigurasi yang diekstrak

| ID | Model atau konfigurasi | Profil dan input bila tersedia | Hardware | Akurasi / kualitas | Parameter / kompleksitas | Latensi atau throughput | Evidence dan halaman PDF | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| P028-E001 | Conv-S | B-CLS | RTX 4090 (setup eksperimen) | 97.69% Acc / 98.26% F1 | 2.30 M / 0.49 GFLOPs | NR | P028-C006 · T5, PDF 13 | VL |
| P028-E002 | Conv-M | B-CLS | RTX 4090 (setup eksperimen) | 98.32% Acc / 98.55% F1 | 8.43 M / 1.81 GFLOPs | NR | P028-C006 · T5, PDF 13 | VL |
| P028-E003 | Hybrid-M | B-CLS | RTX 4090 (setup eksperimen) | 96.98% Acc / 97.51% F1 | 9.79 M / 2.05 GFLOPs | NR | P028-C006 · T5, PDF 13 | VL |
| P028-E004 | MobileNetV2 | B-CLS | RTX 4090 (setup eksperimen) | 98.32% Acc / 98.74% F1 | 2.23 M / 0.65 GFLOPs | NR | P028-C006 · T5, PDF 13 | VL |
| P028-E005 | MobileNetV3-Small | B-CLS | RTX 4090 (setup eksperimen) | 96.98% Acc / 96.86% F1 | 1.49 M / 0.12 GFLOPs | NR | P028-C006 · T5, PDF 13 | VL |
| P028-E006 | Ours | B-CLS | RTX 4090 (setup eksperimen) | 99.54% Acc / 99.75% F1 | 3.70 M / 0.71 GFLOPs | NR | P028-C006 · T5, PDF 13 | VL |
| P028-E007 | Base | B-FWD | Raspberry Pi 4B | 97.69% Acc / 98.26% F1 | 2.36 M / 0.49 GFLOPs | 33.30 ms / 30.03 FPS | P028-C007 · T7, PDF 17 · timing berulang di T9, PDF 18 (P028-C008) | VL |
| P028-E008 | LocalAttention | B-FWD | Raspberry Pi 4B | 98.61% Acc / 98.75% F1 | 2.42 M / 0.51 GFLOPs | 35.50 ms / 28.17 FPS | P028-C007 · T7, PDF 17 | VL |
| P028-E009 | PIB | B-FWD | Raspberry Pi 4B | 98.15% Acc / 98.25% F1 | 3.70 M / 0.71 GFLOPs | 38.50 ms / 25.97 FPS | P028-C007 · T7, PDF 17 | VL |
| P028-E010 | ASML | B-FWD | Raspberry Pi 4B | 98.32% Acc / 98.05% F1 | 2.36 M / 0.49 GFLOPs | 33.30 ms / 30.03 FPS | P028-C007 · T7, PDF 17 | VL |
| P028-E011 | LA+PIB | B-FWD | Raspberry Pi 4B | 99.07% Acc / 99.00% F1 | 3.76 M / 0.71 GFLOPs | 39.58 ms / 25.27 FPS | P028-C007 · T7, PDF 17 | VL |
| P028-E012 | LA+ASML | B-FWD | Raspberry Pi 4B | 98.61% Acc / 99.00% F1 | 2.42 M / 0.51 GFLOPs | 35.50 ms / 28.17 FPS | P028-C007 · T7, PDF 17 | VL |
| P028-E013 | PIB+ASML | B-FWD | Raspberry Pi 4B | 99.08% Acc / 99.50% F1 | 3.70 M / 0.71 GFLOPs | 38.50 ms / 25.97 FPS | P028-C007 · T7, PDF 17 | VL |
| P028-E014 | Ours | B-FWD | Raspberry Pi 4B | 99.54% Acc / 99.75% F1 | 3.76 M / 0.71 GFLOPs | 39.58 ms / 25.27 FPS | P028-C007 · T7, PDF 17 · timing berulang di T9, PDF 18 (P028-C008) | VL |
| P028-E015 | DenseNet121 | B-FWD | Raspberry Pi 4B | NR untuk baris T9 | NR untuk baris T9 | 512.53 ms / 1.95 FPS | P028-C008 · T9, PDF 18 | VL |
| P028-E016 | ResNet34 | B-FWD | Raspberry Pi 4B | NR untuk baris T9 | NR untuk baris T9 | 445.83 ms / 2.24 FPS | P028-C008 · T9, PDF 18 | VL |
| P028-E017 | CoatNet_0 | B-FWD | Raspberry Pi 4B | NR untuk baris T9 | NR untuk baris T9 | 421.65 ms / 2.37 FPS | P028-C008 · T9, PDF 18 | VL |
| P028-E018 | MobileNetViT-S | B-FWD | Raspberry Pi 4B | NR untuk baris T9 | NR untuk baris T9 | 376.65 ms / 2.66 FPS | P028-C008 · T9, PDF 18 | VL |
| P028-E019 | MobileNetViT-XXS | B-FWD | Raspberry Pi 4B | NR untuk baris T9 | NR untuk baris T9 | 155.37 ms / 6.44 FPS | P028-C008 · T9, PDF 18 | VL |
| P028-E020 | MobileNetV3-Small | B-FWD | Raspberry Pi 4B | NR untuk baris T9 | NR untuk baris T9 | 150.11 ms / 6.66 FPS | P028-C008 · T9, PDF 18 | VL |
| P028-E021 | MobileNetV4-Hybrid-Medium | B-FWD | Raspberry Pi 4B | NR untuk baris T9 | NR untuk baris T9 | 122.39 ms / 8.17 FPS | P028-C008 · T9, PDF 18 | VL |
| P028-E022 | MobileNetV2 | B-FWD | Raspberry Pi 4B | NR untuk baris T9 | NR untuk baris T9 | 57.59 ms / 17.36 FPS | P028-C008 · T9, PDF 18 | VL |
| P028-E023 | ShuffleNetV2 | B-FWD | Raspberry Pi 4B | NR untuk baris T9 | NR untuk baris T9 | 56.07 ms / 17.83 FPS | P028-C008 · T9, PDF 18 | VL |
| P028-E024 | MobileNetV3-Large | B-FWD | Raspberry Pi 4B | NR untuk baris T9 | NR untuk baris T9 | 53.92 ms / 18.55 FPS | P028-C008 · T9, PDF 18 | VL |
| P028-E025 | Conv-S | B-NEU | Setup per baris NV | 99.44% Acc | 2.30 M / 0.49 GFLOPs | NR | P028-C010 · T6, PDF 16 | PV |
| P028-E026 | MobileNetV2 | B-NEU | Setup per baris NV | 99.44% Acc | 2.23 M / 0.65 GFLOPs | NR | P028-C010 · T6, PDF 16 | PV |
| P028-E027 | MobileNetV3-Small | B-NEU | Setup per baris NV | 99.44% Acc | 1.49 M / 0.12 GFLOPs | NR | P028-C010 · T6, PDF 16 | PV |
| P028-E028 | MobileMamba_S6 | B-NEU | Setup per baris NV | 99.72% Acc | 14.36 M / 1.28 GFLOPs | NR | P028-C010 · T6, PDF 16 | PV |
| P028-E029 | Ours | B-NEU | Setup per baris NV | 99.72% Acc | 3.70 M / 0.71 GFLOPs | NR | P028-C010 · T6, PDF 16 | PV |
| P028-E030 | MHA | B-CLS | Setup per baris NV | 97.32% Acc / 96.87% F1 | 2.50 M / NR GFLOPs | NR | P028-C012 · T10, PDF 19 | VL |
| P028-E031 | Mobile_Mqa | B-CLS | Setup per baris NV | 96.30% Acc / 96.66% F1 | 2.40 M / NR GFLOPs | NR | P028-C012 · T10, PDF 19 | VL |
| P028-E032 | SCSA | B-CLS | Setup per baris NV | 96.99% Acc / 96.64% F1 | 2.50 M / NR GFLOPs | NR | P028-C012 · T10, PDF 19 | VL |
| P028-E033 | Biformer | B-CLS | Setup per baris NV | 98.32% Acc / 98.74% F1 | 2.52 M / NR GFLOPs | NR | P028-C012 · T10, PDF 19 | VL |
| P028-E034 | MLLA | B-CLS | Setup per baris NV | 97.66% Acc / 97.26% F1 | 2.56 M / NR GFLOPs | NR | P028-C012 · T10, PDF 19 | VL |
| P028-E035 | IRMB | B-CLS | Setup per baris NV | 98.33% Acc / 98.75% F1 | 2.55 M / NR GFLOPs | NR | P028-C012 · T10, PDF 19 | VL |
| P028-E036 | LocalAttention | B-CLS | Setup per baris NV | 98.61% Acc / 98.75% F1 | 2.42 M / NR GFLOPs | NR | P028-C012 · T10, PDF 19 | VL |
| P028-E037 | Ours | B-PAIR | Setup per run NV | Mean % Acc/P/R/F1 99.45/99.52/99.67/99.59 | Lihat T7 untuk konfigurasi | NR | P028-C011 · T8, PDF 17 | VL |
| P028-E038 | PIB+ASML | B-PAIR | Setup per run NV | Mean % Acc/P/R/F1 98.80/99.13/99.04/99.08 | Lihat T7 untuk konfigurasi | NR | P028-C011 · T8, PDF 17 | VL |
| P028-E039 | Ours, brightness −30% | B-ROB | Setup timing NR | 89.26% Top-1 | NR untuk baris ini | NR | P028-C014 · T14, PDF 22 | VL |
| P028-E040 | Conv-M, brightness −30% | B-ROB | Setup timing NR | 91.28% Top-1 | NR untuk baris ini | NR | P028-C014 · T14, PDF 22 | VL |
| P028-E041 | Ours, noise σ=0.10 | B-ROB | Setup timing NR | 93.29% Top-1 | NR untuk baris ini | NR | P028-C014 · T14, PDF 22 | VL |
| P028-E042 | Conv-S, noise σ=0.10 | B-ROB | Setup timing NR | 95.97% Top-1 | NR untuk baris ini | NR | P028-C014 · T14, PDF 22 | VL |
| P028-E043 | Ours, blur k=11 | B-ROB | Setup timing NR | 99.07% Top-1 | NR untuk baris ini | NR | P028-C014 · T15, PDF 23 | VL |
| P028-E044 | Ours, clean kontrol blur | B-ROB | Setup timing NR | 99.54% Top-1 | NR untuk baris ini | NR | P028-C014 · T15, PDF 23 | VL |
| P028-E045 | Ours, stress test 8 jam | B-PIPE | Raspberry Pi 4B, lingkungan lini | Akurasi per frame NR | Memory 1.08→1.14 GB visual, peak RAM NV | Mean 23.35 FPS, SD 0.015, P5–P95 23.34–23.38 | P028-C015/P028-C016/P028-C017 · Fig. 16, PDF 23–25 | PV |
| P028-E046 | Ours, kalimat single-image processing | B-AMB | Raspberry Pi 4B (sistem) | NR | NR | 39 ms, cakupan timer NV | P028-C017 · On-site system deployment, PDF 23 | PV |

Perbandingan yang memakai matriks ini ada di [[03-Synthesis/PAIRWISE_COMPARISON_P002_P028]]. Konflik dan metadata yang belum lengkap ada di [[02-Literature/VERIFICATION_BACKLOG]].

## P001 — konfigurasi pruning Phase 4

C-CLS merujuk eksperimen klasifikasi RASUFI (§§2.2, 3.5–3.6, 4.4). Data crop 1920×1053, bentuk tensor inferensi NR. Workstation RTX 3090/Xeon Gold 6146, PyTorch 2.5.1/CUDA 12.4 adalah setup eksperimen. Perangkat target, runtime dan presisi inference, batch, threads, warm-up, pengulangan timing, peak RAM dan energi NR. Tidak ada hasil latensi/throughput untuk menyatakan batas timing. Ukuran model di kolom biaya berasal dari Table 6. Pasangan dan reduksi ada di [[02-Literature/Literature-Matrix/PRUNING_EVIDENCE_MATRIX]].

| ID | Model atau konfigurasi | Profil | Hardware | Akurasi / kualitas | Parameter / kompleksitas / ukuran | Latensi atau throughput | Evidence dan halaman PDF | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| P001-E001 | MobileNetV4 asli | C-CLS | Workstation setup | 92.89% accuracy / 92.88% F1 | 2.5 M / NR FLOPs / 9.5 MB | NR | P001-C007 · T6, PDF 13 | VL |
| P001-E002 | DF-MobileNetV4 padat | C-CLS | Workstation setup | 95.59% accuracy / 95.60% F1 | 4.0 M / 1 G FLOPs (T3) / 15.4 MB | NR | P001-C007, P001-C009 · T3 (4.04 M), T6 (4.0 M), PDF 8, 13 | VL |
| P001-E003 | DF-MobileNetV4-pruning + fine-tuning | C-CLS | Workstation setup | 96.49% accuracy / 96.44% F1 | 3.2 M / NR FLOPs / 12.5 MB | NR | P001-C007 · T6, PDF 13 | VL |

## P029 — konfigurasi pruning Phase 4

**D-CLS** adalah task internal radar CW, input STFT 227×227 RGB menurut metode. Penulis menyebut held-out test dan aturan validasi yang belum konsisten. Training di Intel Core i5-13600K dengan batch 64, framework dan precision evaluasi klasifikasi NR. FLOPs dibaca dari T4 dalam juta operasi. MACs, cara menghitung FLOPs, parameter mentah tabel, peak RAM, flash aktual, energi serta throughput NR. Precision untuk metode UP+Q/hybrid dilaporkan sebagai INT8 untuk bobot, tetapi skala, aktivasi dan artefak evaluasi NV. P029-C002/C005–C006/C009–C014.

**D-RT** adalah timing T6 untuk baseline dan hybrid pada target edge yang disebut penulis. Board/part number, clock, backend, presisi tiap artefak, delegate, compiler, batch aktual, threads, warm-up, repetisi dan statistik NR. **Unclear / Not reported** karena titik awal/akhir timer dan inklusi preprocessing belum diketahui. Angka ms ditranskripsi secara lokal dan provenance pengukuran berstatus PV. Setup training bukan bukti hardware timing. Pasangan dan perhitungan ada di [[02-Literature/Literature-Matrix/PRUNING_EVIDENCE_MATRIX]].

| ID | Model atau konfigurasi | Profil | Hardware | Akurasi / kualitas | Parameter / kompleksitas / ukuran | Latensi atau throughput | Evidence dan halaman PDF | Status |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| P029-E001 | MobileNetV3 baseline | D-CLS + D-RT | Target edge terlapor (board NV) | 94.0% acc / 93.9% recall / 93.9% F1 | NR parameter / 71 M FLOPs / 5.94 MB / spar. 0 | 592.5 ms (statistik NR) | P029-C010 · T4, PDF 16; P029-C013 · T6, PDF 18 | PV |
| P029-E002 | MobileNetV3 UP+Q | D-CLS | Timing NR | 94.4% acc / 94.4% recall / 94.3% F1 | NR parameter / 71 M FLOPs / 4.42 MB / spar. 0.70 | NR | P029-C010 · T4, PDF 16 | VL |
| P029-E003 | MobileNetV3 SP | D-CLS | Timing NR | 94.2% acc / 94.2% recall / 94.1% F1 | NR parameter / 44 M FLOPs / 1.05 MB / spar. 0 | NR | P029-C010 · T4, PDF 16; T3, PDF 14 | VL |
| P029-E004 | MobileNetV3 Hybrid | D-CLS + D-RT | Target edge terlapor (board NV) | 94.8% acc / 94.8% recall / 95.6% F1 | NR parameter / 44 M FLOPs / 1.02 MB / spar. 0.40 | 366.7 ms (statistik NR) | P029-C010 · T4, PDF 16; P029-C011; P029-C013 · T6, PDF 18 | PV |
| P029-E005 | MobileNetV2 baseline | D-CLS + D-RT | Target edge terlapor (board NV) | 94.4% acc / 94.4% recall / 94.4% F1 | NR parameter / 373 M FLOPs / 8.74 MB / spar. 0 | 3108.8 ms (statistik NR) | P029-C010 · T4, PDF 16; P029-C012 (baseline); P029-C013 · T6, PDF 18 | PV |
| P029-E006 | MobileNetV2 UP+Q | D-CLS | Timing NR | 92.8% acc / 92.7% recall / 92.9% F1 | NR parameter / 373 M FLOPs / 6.50 MB / spar. 0.30 | NR | P029-C010 · T4, PDF 16; P029-C012 (baseline) | PV |
| P029-E007 | MobileNetV2 SP | D-CLS | Timing NR | 95.0% acc / 95.0% recall / 95.0% F1 | NR parameter / 160 M FLOPs / 0.58 MB / spar. 0 | NR | P029-C010 · T4, PDF 16; T3, PDF 14; P029-C012 (baseline) | PV |
| P029-E008 | MobileNetV2 Hybrid | D-CLS + D-RT | Target edge terlapor (board NV) | 93.6% acc / 93.6% recall / 93.6% F1 | NR parameter / 160 M FLOPs / 0.48 MB / spar. 0.10 | 1334.2 ms (statistik NR) | P029-C010 · T4, PDF 16; P029-C012 (baseline); P029-C013 · T6, PDF 18 | PV |
| P029-E009 | EfficientNet-B0 baseline | D-CLS + D-RT | Target edge terlapor (board NV) | 93.2% acc / 93.2% recall / 93.2% F1 | NR parameter / 27 M FLOPs / 15.59 MB / spar. 0 | 226.5 ms (statistik NR) | P029-C010 · T4, PDF 16; P029-C013 · T6, PDF 18 | PV |
| P029-E010 | EfficientNet-B0 UP+Q | D-CLS | Timing NR | 94.6% acc / 94.6% recall / 94.6% F1 | NR parameter / 27 M FLOPs / 15.59 MB / spar. 0.80 | NR | P029-C010 · T4, PDF 16 | VL |
| P029-E011 | EfficientNet-B0 SP | D-CLS | Timing NR | 93.6% acc / 93.5% recall / 93.6% F1 | NR parameter / 21 M FLOPs / 1.35 MB / spar. 0 | NR | P029-C010 · T4, PDF 16; T3, PDF 14 | VL |
| P029-E012 | EfficientNet-B0 Hybrid | D-CLS + D-RT | Target edge terlapor (board NV) | 93.4% acc / 93.4% recall / 93.4% F1 | NR parameter / 21 M FLOPs / 1.20 MB / spar. 0.40 | 179.2 ms (statistik NR) | P029-C010 · T4, PDF 16; P029-C013 · T6, PDF 18 | PV |
| P029-E013 | SqueezeNet baseline | D-CLS + D-RT | Target edge terlapor (board NV) | 92.8% acc / 92.7% recall / 92.7% F1 | NR parameter / 287 M FLOPs / 2.78 MB / spar. 0 | 2399.2 ms (statistik NR) | P029-C010 · T4, PDF 16; P029-C013 · T6, PDF 18 | PV |
| P029-E014 | SqueezeNet UP+Q | D-CLS | Timing NR | 93.0% acc / 93.0% recall / 93.0% F1 | NR parameter / 287 M FLOPs / 2.48 MB / spar. 0.80 | NR | P029-C010 · T4, PDF 16 | VL |
| P029-E015 | SqueezeNet SP | D-CLS | Timing NR | 92.2% acc / 92.2% recall / 92.2% F1 | NR parameter / 170 M FLOPs / 0.47 MB / spar. 0 | NR | P029-C010 · T4, PDF 16; T3, PDF 14 | VL |
| P029-E016 | SqueezeNet Hybrid | D-CLS + D-RT | Target edge terlapor (board NV) | 92.2% acc / 92.1% recall / 92.1% F1 | NR parameter / 170 M FLOPs / 0.47 MB / spar. 0.50 | 1422.5 ms (statistik NR) | P029-C010 · T4, PDF 16; P029-C013 · T6, PDF 18 | PV |

Klaim Phase 4 ada di [[02-Literature/Paper-Reviews/P001#Evidence references]] dan [[02-Literature/Paper-Reviews/P029#Evidence references]]. Table 2 P029 dipertahankan sebagai konteks di review, khususnya perbedaan baseline MobileNetV2. Rumus dan pasangan antar-rezim ada di matriks pruning.
