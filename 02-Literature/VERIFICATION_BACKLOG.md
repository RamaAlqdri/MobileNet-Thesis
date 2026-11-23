---
type: verification-backlog
scope: [P002, P028]
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
