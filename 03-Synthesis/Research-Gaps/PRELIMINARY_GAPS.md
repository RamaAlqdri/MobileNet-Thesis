---
type: preliminary-gap-register
classification: hypothesized gap
scope: korpus-lokal
updated: 2026-11-23
tags: [mobilenet, hardware-aware, pruning, research-gap]
---

# Kandidat persoalan penelitian

Enam observasi berikut berasal dari screening korpus lokal dan berstatus **hypothesized gap**. Keyakinan mengacu pada alasan untuk menyelidiki kandidat, dengan cakupan terbatas pada bagian PDF yang disebut di [[02-Literature/Literature-Matrix/SCREENING_MATRIX]]. Klaim gap pada literatur yang lebih luas menunggu full review dan pembandingan sumber yang memadai.

## G01 — FLOPs/MACs dan latensi perangkat

- **Deskripsi** Peringkat efisiensi berdasarkan biaya operasi dapat berubah ketika diukur pada runtime atau perangkat tertentu.
- **Bukti** P002 Appendix F Table 16 melaporkan korelasi peringkat MACs–latensi lebih tinggi pada Pixel 6 CPU daripada Pixel 4 DSP. Model roofline yang memasukkan bandwidth memori memperkuat korelasi di keduanya pada konfigurasi yang ditelaah. P004 Table 17 memperlihatkan V4-Conv-M lebih cepat daripada V3-Large pada klasifikasi RTX 4090 meski FLOPs tercatat lebih tinggi. [[02-Literature/Paper-Reviews/P002]] · [[02-Literature/Paper-Reviews/P004]]
- **Interpretasi** FLOPs/MACs dapat menjadi proksi murah. Reliabilitasnya perlu diukur pada masing-masing perangkat dan backend.
- **Bukti pembatas** P029 melaporkan latensi yang mendekati proporsional terhadap FLOPs untuk baseline dan versi hybrid di bawah hardware serta batch tetap. Hubungan tersebut dapat berlaku dalam kondisi terkontrol. [[02-Literature/Paper-Reviews/P029]]
- **Keyakinan** HIGH untuk memeriksa ketergantungan proksi pada perangkat.
- **Investigasi dan arah potensial** Ekstrak definisi MACs, input, runtime, presisi, dan prosedur timing. Uji korelasi per backend untuk kandidat V3/V4 dan bandingkan biaya operasi dengan profil blok.

## G02 — Trade-off akurasi dan latensi setelah structured pruning

- **Deskripsi** Rasio dan letak pemangkasan yang memberi penurunan latensi dengan kehilangan akurasi yang dapat diterima mungkin berubah antarvarian dan backend.
- **Bukti** P001 Table 6 melaporkan akurasi dan ukuran file yang membaik relatif terhadap DF-MobileNetV4 sebelum pruning, tetapi ukuran file terpangkas tetap lebih besar daripada baseline MobileNetV4. Tabel itu tidak menyajikan latensi. P006 Figs. 4–5 melaporkan waktu per gambar dan perubahan akurasi pada MobileNetV2, tanpa spesifikasi hardware di uraian waktu. [[02-Literature/Paper-Reviews/P001]] · [[02-Literature/Paper-Reviews/P006]]
- **Interpretasi** Keuntungan pruning perlu dibandingkan dengan baseline asli dan model modifikasi sebelum pruning untuk mengisolasi pengaruhnya.
- **Bukti pembatas** P029 sudah melaporkan latensi sebelum/sesudah pruning hybrid pada target MCU, termasuk V3. Pengaruh gabungan bisection, sparsity, repacking, dan INT8 serta transfer ke V4 perlu ditelusuri. [[02-Literature/Paper-Reviews/P029]]
- **Keyakinan** MEDIUM.
- **Investigasi dan arah potensial** Audit baseline, granularity dan teknik fine-tuning. Bandingkan channel pruning dan block pruning dengan rasio bertingkat, aturan alignment, dan timing perangkat yang sama.

## G03 — Sensitivitas blok MobileNetV4 terhadap perubahan

- **Deskripsi** Berapa biaya latensi setiap penambahan atau penggantian komponen, dan blok mana yang paling sensitif pada tiap varian?
- **Bukti** P002 menjelaskan opsi UIB dan Mobile MQA. P007 mencari beberapa pengaturan UIB. P017 §3.2 memberi ablation untuk SimAM, Ghost convolution, dan Mish pada MobileNetV4-Small. [[02-Literature/Paper-Reviews/P002]] · [[02-Literature/Paper-Reviews/P007]] · [[02-Literature/Paper-Reviews/P017]]
- **Interpretasi** Hasil ablation satu task dapat memandu pemilihan konfigurasi yang perlu diuji lintas backend.
- **Bukti pembatas** P025 melaporkan CBAM menurunkan akurasi rata-rata V4-Small pada konfigurasi transfer learning di tabel ablation. P017 menguji jenis attention, dataset, dan konfigurasi yang berbeda. Kedua hasil belum menjadi perbandingan attention yang setara. [[02-Literature/Paper-Reviews/P025]] · [[02-Literature/Paper-Reviews/P017]]
- **Keyakinan** MEDIUM.
- **Investigasi dan arah potensial** Lacak konfigurasi UIB/MQA dan letak modul. Jalankan ablation satu perubahan per kondisi dengan bentuk tensor dan input yang sama, lalu profilkan blok serta full model.

## G04 — Transfer optimasi arsitektur antarhardware

- **Deskripsi** Arsitektur yang dipilih dengan satu model biaya bisa memiliki urutan latensi berbeda pada backend lain.
- **Bukti** P020 menguji prediktor dan pencarian HL-DARTS pada CPU/GPU yang berbeda. P019 membangun LUT per Raspberry Pi untuk ConvNet 1D. P007 memasukkan timing ke reward pencarian UIB, sementara perangkat untuk timing tersebut belum terverifikasi dari bagian yang diperiksa. [[02-Literature/Paper-Reviews/P020]] · [[02-Literature/Paper-Reviews/P019]] · [[02-Literature/Paper-Reviews/P007]]
- **Interpretasi** Satu LUT atau prediktor sebaiknya divalidasi pada targetnya sebelum dipakai untuk menyaring kandidat MobileNet.
- **Bukti pembatas** P002 melaporkan varian V4 yang sebagian besar Pareto-optimal pada rangkaian target yang diuji. Hal itu menunjukkan bahwa pilihan yang efektif lintas perangkat sudah dilaporkan pada kondisi tertentu. [[02-Literature/Paper-Reviews/P002]]
- **Keyakinan** MEDIUM.
- **Investigasi dan arah potensial** Audit bagaimana paper memisahkan prediksi dari validasi nyata. Bandingkan ranking kandidat UIB lintas perangkat, termasuk kandidat yang menang pada satu backend atau stabil lintas backend.

## G05 — Kesebandingan benchmark lintas perangkat

- **Deskripsi** Angka timing lintas paper sulit dipakai untuk membandingkan model saat batas pengukuran, delegate, atau bentuk eksekusi berbeda.
- **Bukti** P028 melaporkan forward pass dan throughput pipeline industri secara terpisah. P012 Tables 2–3 memakai waktu total dengan komponen tersendiri, sedangkan P024 membedakan dua ponsel fisik dan satu emulator. [[02-Literature/Paper-Reviews/P028]] · [[02-Literature/Paper-Reviews/P012]] · [[02-Literature/Paper-Reviews/P024]]
- **Interpretasi** Matriks eksperimen perlu merekam batas timing bersama detail perangkat. Nama perangkat saja belum menjamin angka sebanding.
- **Bukti pembatas** P002 Fig. 1 membedakan presisi dan jenis backend per panel. P028 memberi contoh pemisahan batas pengukuran dalam studi yang sama. [[02-Literature/Paper-Reviews/P002]] · [[02-Literature/Paper-Reviews/P028]]
- **Keyakinan** HIGH untuk kebutuhan audit protokol.
- **Investigasi dan arah potensial** Ekstrak batch, input, warm-up, jumlah pengulangan, thermal state, runtime/delegate, dan batas pipa. Bangun protokol pembanding forward dan end-to-end pada perangkat yang tersedia.

## G06 — Ukuran file, memori puncak, dan latensi

- **Deskripsi** Penghematan file atau bit-width belum cukup untuk memperkirakan memori proses dan waktu inferensi.
- **Bukti** P001 membandingkan ukuran file setelah pruning. P012 melaporkan RAM dan flash secara terpisah setelah INT8. P029 membedakan ukuran bobot dari aktivasi dan buffer yang ikut memakai SRAM. [[02-Literature/Paper-Reviews/P001]] · [[02-Literature/Paper-Reviews/P012]] · [[02-Literature/Paper-Reviews/P029]]
- **Interpretasi** Ukur ketiga keluaran secara berpasangan pada artefak dan runtime yang sama agar kontribusi perubahan dapat dipisahkan.
- **Bukti pembatas** P024 sudah melaporkan memori dan kecepatan inferensi untuk model ringan dengan pengaturan kedalaman dan INT8. P021 menilai memori puncak pada GNN dan memberi contoh metode profiling. Transfer ke perbandingan V3/V4 masih perlu diperiksa. [[02-Literature/Paper-Reviews/P024]] · [[02-Literature/Paper-Reviews/P021]]
- **Keyakinan** MEDIUM.
- **Investigasi dan arah potensial** Periksa definisi dan metode pengukuran tiap sumber. Ukur file model, memori puncak, dan latensi untuk FP32 dan INT8 pada baseline yang disamakan sebelum menguji mixed precision atau pruning.

## Bukti setelah screening

[[03-Synthesis/Research-Gaps/GAP_EVIDENCE_UPDATE_P002_P028]] memetakan bukti P002 dan P028 ke G01–G06. Hipotesis awal dipertahankan di atas agar perubahan keyakinan dapat dilacak.
