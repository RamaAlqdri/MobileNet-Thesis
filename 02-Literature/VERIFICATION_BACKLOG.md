---
type: verification-backlog
scope: [P002, P028, P001, P029]
updated: 2026-11-23
tags: [mobilenet, literature-review, thesis-progress]
---

# Verification backlog

Status `OPEN` berarti data atau klarifikasi berikutnya masih diperlukan. Klaim terlapor dapat diperiksa di [[02-Literature/Paper-Reviews/P002#Evidence references]] dan [[02-Literature/Paper-Reviews/P028#Evidence references]]. Peneliti manusia dapat menerima angka sebagai transkripsi lokal setelah mengaudit locator dan memutuskan bagaimana membatasi interpretasinya. Dataset, versi final atau artefak tambahan diperiksa hanya jika tersedia secara lokal sesuai policy proyek.

| ID | Prioritas/status | Evidence dan pertanyaan | Bukti yang menutup item |
| --- | --- | --- | --- |
| V01 | Tinggi · OPEN | P002-C014, Abstract/§8/Table 6 (PDF 1, 11, 13–14). Klaim 87,0% dan 3,8 ms. Timing baris Table 6 terkait dengan akurasi 83,4%. | Runner log atau tabel yang memasangkan export model/checkpoint 87,0% dengan timing Pixel 8 EdgeTPU, precision dan konfigurasi yang sama. |
| V02 | Tinggi · OPEN | P002-C011, Appendix B (PDF 19). Detail timer I/O, thread, warm-up, batch, versi TFLite/delegate, dan prosedur khusus ANE belum lengkap. | Profil reproduksi dengan model export, checksum, perangkat, raw timing dan versi runner. |
| V03 | Menengah · OPEN | P002-C013, §8/Table 8/Appendix H (PDF 13–14, 31–32). D1 disebut 500 replika di teks, 1000× di tabel. | Resep eksperimen, metadata replika, checkpoint dan dataset yang tersedia lokal. |
| V04 | Menengah · OPEN | P002-C004–P002-C007, Tables 2–3 dan 17 (PDF 8, 31). Persentase penghematan biaya attention atau gain berbeda dari reduksi latency model. | Konfirmasi denominator dan raw operator timings. Untuk penulisan saat ini gunakan nilai mentah dan denominator eksplisit. |
| V05 | Tinggi · OPEN | P028-C001/P028-C002, Table 1/Fig. 3 (PDF 5–6). LA stride 2 ditulis pada tabel, tetapi bentuk layer berikutnya dan gambar mempertahankan H×W. | Graph ONNX atau definisi sumber lokal yang menunjukkan strand Q/K/V, stride, padding, normalisasi dan output shape. |
| V06 | Tinggi · OPEN | P028-C007–P028-C009/P028-C017, Tables 7/9 dan deployment (PDF 16–18, 23–25). 6,28 ms hasil selisih tabel vs narasi sekitar 3,6 ms. Kalimat 39 ms memiliki scope yang belum jelas. | Log Pi per konfigurasi, batas timer, presisi, CPU/provider/runtime/version, warm-up, repetisi, statistik, threads dan batch yang diaudit. |
| V07 | Menengah · OPEN | P028-C006/P028-C007/P028-C009/P028-C012, Tables 5/7/10 (PDF 13, 17–19). Perbedaan parameter Conv-S/Ours antar Table 5/7 dan parameter attention dibandingkan narasi. | Kode penghitung, graph/checkpoint tiap tabel dan aturan pemisahan trainable/non-trainable serta FLOP/MAC. |
| V08 | Tinggi · OPEN | P028-C005/P028-C010/P028-C019 (PDF 10–16). Holdout 80:20 disebut validation/test, split NEU-CLS kurang jelas, dan Fig. 11 hanya memperlihatkan matriks ternormalisasi. | ID split per lot/waktu, kelas, kebijakan seleksi checkpoint, seed, prediksi/label asli dan definisi macro/micro/weighted metrics. |
| V09 | Menengah · OPEN | P028-C011/P028-C013 (PDF 17–20). t-test dapat direkalkulasi secara nominal dengan df=4. Pairing per seed, multiplicity dan perbedaan F1 default 97,25/98,05 pada Tables 11/12 masih terbuka. | Lima pasangan nilai mentah, analisis per metrik dan protokol tuning dengan identitas split. |
| V10 | Tinggi · OPEN | P028-C015–P028-C017, Fig. 16(c) dan deployment (PDF 23–26). Kurva memori meningkat secara visual meskipun teks menyebut stabil. Throughput 8 jam memberi FPS tingkat sistem. | Raw telemetry, definisi agregat memori/RSS, log suhu dan throttle, timestamp per tahap, serta jejak antrean drop frame/capture/PLC. |
| V11 | Menengah · OPEN | P002-C010/P002-C015 dan P028-C016. File model, peak RAM model, dan energi per inferensi tidak dilaporkan untuk perbandingan yang dibutuhkan. | Profil runtime dan daya idle-dikoreksi, ukuran artefak pasca-ekspor, serta peak process/allocator memory dari workload dan presisi yang sama. |
| V12 | Menengah · OPEN | P002 metadata preprint dan referensi [19], PDF 1/16. P028 mengutip terbitan konferensi MobileNetV4 pada ref. [7], PDF 27. Kesetaraan versi P002 dan rujukan primer MobileNetV3 belum dapat diuji dengan dokumen lokal. | Salinan lokal versi konferensi/terbit dan `Searching for MobileNetV3` bila tersedia, lalu bandingkan versi/tabel secara eksplisit. |
| V13 | Tinggi · OPEN | Validasi manusia atas evidence P002-C001–P002-C016 dan P028-C001–P028-C020, terutama denominator MQA, konflik tabel P028 dan interpretasi Fig. 16(c). | Peneliti memeriksa PDF, mengesahkan atau mengoreksi transkripsi dan perhitungan, serta menandai klaim yang dapat dipakai dalam tesis. |

Matriks pelacakan ada di [[02-Literature/Literature-Matrix/CRITICAL_EVIDENCE_MATRIX]]. Item lama di luar target sesi tetap terdokumentasi di [[02-Literature/PAPER_INVENTORY]].

## Phase 4 — structured pruning dan hardware

| ID | Prioritas/status | Evidence dan pertanyaan | Bukti yang menutup item |
| --- | --- | --- | --- |
| V14 | Tinggi · OPEN | [[02-Literature/Paper-Reviews/P001#Evidence references]] P001-C001–P001-C005, PDF 4–7, 12–13. L1 kernel vs BN dan cara menerapkan alignment. DW pada Eq. 2 vs Fig. 6, baris 23–24 dan shape kanal IB/DSA Table 2, subvarian baseline dan peta sensitivity belum tuntas. | Proof final atau kode/graph, mask dan indeks kanal, ranking per layer serta konfigurasi ekspor. |
| V15 | Tinggi · OPEN | P001-C006–P001-C012, PDF 3, 7–14. Total 4.944 vs 4.994 dan prosedur cross-domain. Cek Table 6, macro-F1, resep dense+FT dan identitas checkpoint. | Daftar split per video/hari, seed, prediksi/label, definisi agregasi F1 serta run matched recovery. |
| V16 | Tinggi · OPEN | P001-C007–P001-C009, PDF 8, 13. Untuk DF pruned, FLOPs dan timing perangkat belum dilaporkan. | Profil paired dense/pruned dengan hardware, runtime, precision, scope, batch/threads, pengulangan, peak RAM, artefak dan perhitungan FLOPs yang sama. |
| V17 | Tinggi · OPEN | [[02-Literature/Paper-Reviews/P029#Evidence references]] P029-C013–P029-C014, PDF 5, 16–19. Audit Table 6, device provenance, jenis precision masing-masing model dan cakupan timer, termasuk STFT/CA-CFAR. Precision parity belum diketahui. | Model/timing log asli dengan board, clock, compiler/runtime/kernel, format INT8, batch/thread, warm-up, statistik dan timestamp untuk tiap konfigurasi. Tambahkan paired timings SP, UP+Q dan hybrid. |
| V18 | Tinggi · OPEN | P029-C002, P029-C011–P029-C012, P029-C016–P029-C017, PDF 5–6, 11–12, 14, 16–17, 20–21. Fig. 9(a) memberi 478/500 benar, T4 V3 hybrid memuat accuracy 94.8%. T2/T4 V2 berbeda. Prosa 0.58 M params dapat tertukar dengan 0.58 MB. Angka T3 SP V2 dibahas sebagai hybrid. | Identitas holdout dan checkpoint yang sama, confusion matrix mentah, kode agregasi klasifikasi, param count dan metadata artefak T2/T3/T4. |
| V19 | Menengah · OPEN | P029-C003–P029-C008, PDF 5–10, 12–15, 18. Fig. 6(a) input 24 vs RGB dan delapan baris bneck vs retained count sembilan. Cek anggaran/oracle, bisection order, kuantil `1−r`, lima vs dua evaluasi dan per-channel vs global min–max INT8. | Graph tiap export, peta order/repack/mask, konvensi kuantil, logs per kandidat dan skema kalibrasi dengan precision aktivasi yang dipakai. |
| V20 | Tinggi · OPEN | P029-C010, P029-C014, PDF 16–19. Weights-only hybrid di prosa cocok dengan baris SP untuk sebagian backbone. Peak SRAM/aktivasi/flash dan energi belum diukur dalam hasil yang tersedia. | File dan format serialisasi, build map/flash, peak SRAM atau arena dengan scope, daya dikoreksi idle, serta konfigurasi pengukuran per artefak. |
| V21 | Tinggi · OPEN | Audit manusia P001-C001–P001-C012 dan P029-C001–P029-C017. Prioritas pada T6 P001, T3–T6 P029, Fig. 6(a) dan Fig. 9(a). | Selesaikan Human Validation Checklist di kedua review, verifikasi hitungan pada [[02-Literature/Literature-Matrix/PRUNING_EVIDENCE_MATRIX]], lalu catat status/evidence yang boleh dipakai pada tesis. |
