---
type: gap-evidence-update
phase: 4
scope: [P001, P002, P028, P029]
updated: 2026-11-23
tags: [mobilenet, pruning, research-gap, hardware-aware]
---

# Bukti tambahan untuk G01–G06

Hipotesis awal dicatat di [[03-Synthesis/Research-Gaps/PRELIMINARY_GAPS]] dan audit Phase 3 di [[03-Synthesis/Research-Gaps/GAP_EVIDENCE_UPDATE_P002_P028]]. Status di halaman ini berlaku pada korpus empat paper. **PARTIALLY SUPPORTED** berarti ada bukti atas sebagian hubungan. **INSUFFICIENT EVIDENCE** berarti hubungan inti masih membutuhkan data yang sepadan. **HYPOTHESIZED** dan **CONTRADICTED** tersedia untuk kasus hipotesis tanpa uji dan bukti yang bertentangan secara langsung. Seluruh G01–G06 masih kandidat tesis dengan novelty terbuka.

## G01 — FLOPs/MACs dan latensi aktual

- **Hipotesis** — Pengurangan FLOPs atau MACs dapat mengubah latency secara berbeda antar-model dan backend.
- **Mendukung** — [[02-Literature/Paper-Reviews/P002]] memperlihatkan rank MAC–latency berbeda antara CPU dan DSP serta timing berbeda setelah perubahan layout pada MACs tetap (P002-C007, P002-C015–P002-C016). [[02-Literature/Paper-Reviews/P028]] melaporkan timing berbeda untuk dua varian dengan 0.71 GFLOPs yang dibulatkan (P028-C007). [[02-Literature/Paper-Reviews/P001]] tidak memberi FLOPs pruned/timing pasangan (P001-C009).
- **Bukti pembatas** — Dalam [[02-Literature/Paper-Reviews/P029]], FLOPs dan timing sama-sama turun pada empat pasangan baseline→hybrid. Efek komponen dan provenance hardware belum terurai. Pada P002 korelasi CPU justru kuat. [P029-C010, P029-C013, P002-C016]
- **Status** — PARTIALLY SUPPORTED, keyakinan tinggi untuk variasi hubungan dalam korpus dan rendah untuk prediksi pengaruh pruning V4.
- **Belum diketahui, uji dan kelayakan** — Ukur dense/pruned pada graph dan precision tetap di smartphone CPU, dengan SBC sebagai replikasi jika tersedia. Rekam biaya operator, jumlah operasi tanpa pembulatan dan p50/p90 per-run. Pengujian satu perangkat layak setelah build model dan runner tersedia.
- **Literatur tambahan** — [[02-Literature/Paper-Reviews/P007]] untuk definisi biaya UIB dan [[02-Literature/Paper-Reviews/P020]] untuk evaluasi prediktor latensi.

## G02 — Accuracy–efficiency trade-off dalam pruning

- **Hipotesis** — Pemilihan channel atau blok dengan latency target dapat menghasilkan trade-off accuracy–efficiency berbeda dari pruning berbasis magnitudo atau jumlah blok.
- **Mendukung** — P001 menjadwalkan rasio per layer serta fine-tuning, tanpa kriteria latency terukur (P001-C003–P001-C009). P029 memasukkan latency cap dan klaim pengukuran paket hybrid, dengan oracle latensi dan timing SP tidak diketahui (P029-C003–P029-C004, P029-C009–P029-C013).
- **Bukti pembatas** — P029 Table 4 menunjukkan accuracy hybrid terlapor dapat membaik atau menurun menurut backbone dan SP V2 menghasilkan accuracy lebih tinggi daripada hybrid. P001 accuracy dapat dipengaruhi recovery dan loss. [P029-C010–P029-C012, P029-C016, P001-C006–P001-C008]
- **Status** — PARTIALLY SUPPORTED, sedang untuk keberadaan trade-off, rendah untuk manfaat pemilihan berdasarkan actual latency.
- **Belum diketahui, uji dan kelayakan** — Jalankan dense+FT, magnitude+FT dan measured latency-aware+FT dengan anggaran accuracy, seed, split dan presisi sama. Simpan precision dan graph per export. Skala kecil layak setelah dependency pruning tervalidasi.
- **Literatur tambahan** — [[02-Literature/Paper-Reviews/P006]] untuk kontrol structured pruning dan distilasi, [[02-Literature/Paper-Reviews/P010]] untuk pemilihan filter.

## G03 — Sensitivitas blok MobileNetV4

- **Hipotesis** — Pengubahan jenis/lokasi blok V4 menimbulkan dampak berbeda bagi accuracy dan latency, yang perlu diuji untuk pruning.
- **Mendukung** — P002 melaporkan UIB/IB dan ablasi MQA/layout. P028 mengukur efek LA dan PIB secara terpisah pada Pi. P001 membedakan rasio menurut layer dan mengecualikan DSA/head. [P002-C003–P002-C007, P028-C007, P001-C004–P001-C005]
- **Bukti pembatas** — P001 tidak menyediakan kurva blok atau timing. P029 menguji jumlah blok MobileNetV3 pada urutan tertentu dan tidak menyediakan bukti sensitivitas pruning UIB individual (P029-C004–P029-C005). Pilihan layout P002 bukan penghapusan blok.
- **Status** — PARTIALLY SUPPORTED, sedang untuk perbedaan biaya desain V4, rendah untuk ranking sensitivitas pruning blok.
- **Belum diketahui, uji dan kelayakan** — Profil satu perubahan per UIB dengan residual/shape yang kompatibel dan kategorikan pointwise/depthwise dengan posisi downsampling tetap. Sweeping penuh lintas seed mahal, lakukan pilot beberapa blok pada satu varian.
- **Literatur tambahan** — [[02-Literature/Paper-Reviews/P007]] untuk ruang pencarian UIB, [[02-Literature/Paper-Reviews/P017]] untuk modifikasi V4-Small.

## G04 — Hardware transferability

- **Hipotesis** — Sebuah konfigurasi pruning yang unggul di satu perangkat dapat berubah ranking pada perangkat atau runtime lain.
- **Mendukung** — MobileNetV4 Hybrid pada P002 mencatat kegagalan DSP dan model biaya menunjukkan variasi berkaitan backend. P028 hanya mengaitkan hasil acuan dengan Pi 4B. P001 tidak menguji latency perangkat fisik. P029 hanya melaporkan satu setup edge yang identitasnya belum jelas. [P002-C010–P002-C011, P002-C016, P028-C007, P001-C009, P029-C013]
- **Bukti pembatas** — Keluarga Conv P002 bekerja pada beberapa target, sehingga keterbatasan transfer dapat tergantung desain. Belum ada bukti kontradiktif dari pasangan pruning yang direplikasi lintas perangkat. [P002-C010]
- **Status** — INSUFFICIENT EVIDENCE, rendah untuk transfer efek pruning. Bukti hardware-dependence arsitektur sudah tersedia.
- **Belum diketahui, uji dan kelayakan** — Pertahankan export, input dan precision, ukur dua target (misalnya mobile CPU dan Pi CPU), dokumentasikan kernel/fallback dan estimasi peringkat Pareto. Target kedua bergantung ketersediaan perangkat.
- **Literatur tambahan** — [[02-Literature/Paper-Reviews/P020]] dan [[02-Literature/Paper-Reviews/P023]] untuk generalisasi prediktor lintas target.

## G05 — Benchmark comparability

- **Hipotesis** — Perbedaan scope timer, presisi, runtime dan agregasi membatasi perbandingan langsung lintas paper/perangkat.
- **Mendukung** — P002 Appendix B menjelaskan backend dan agregasi untuk profil utama, dengan detail timer dan ANE belum lengkap. P028 memisahkan forward dari throughput pipeline, dengan sebagian metadata runner masih terbuka. T6 P029 menyebut end-to-end tanpa timestamp atau deskripsi komponen. P001 hanya menyediakan setup eksperimen desktop. [P002-C011, P028-C007, P028-C015–P028-C017, P029-C013–P029-C014, P001-C006, P001-C009]
- **Bukti pembatas** — P028 menyediakan contoh pengukuran terpisah dan P002 mendokumentasikan repeated runs. Di P029 hardware dan batch diklaim tetap sehingga perbandingan aritmetis intra-tabel dapat dinilai bersyarat.
- **Status** — PARTIALLY SUPPORTED, keyakinan tinggi untuk keterbatasan komparasi pada korpus.
- **Belum diketahui, uji dan kelayakan** — Simpan versi runner, precision, model checksum dan log posisi start/stop untuk forward dan workload end-to-end, termasuk pre/postprocessing. Protokol feasible pada satu perangkat dengan kontrol suhu/thread.
- **Literatur tambahan** — [[02-Literature/Paper-Reviews/P012]] dan [[02-Literature/Paper-Reviews/P024]] untuk definisi waktu total dan pemisahan perangkat fisik dari emulator.

## G06 — Model size, peak memory dan latency

- **Hipotesis** — Reduksi parameter dan ukuran file bisa memberi perubahan peak RAM dan latency yang berbeda menurut aktivasi, runtime dan device.
- **Mendukung** — P001 memberi ukuran file dan parameter, tanpa peak atau timing. P029 memberi file dan timing hybrid, lalu mencatat bahwa peak SRAM juga mencakup aktivasi dan buffer. P002 memasukkan bytes bobot/aktivasi sebagai estimasi roofline, dan P028 memberi kurva penggunaan memori dengan scope peak/RSS belum jelas. [P001-C007–P001-C009, P029-C010, P029-C013–P029-C014, P002-C015, P028-C016]
- **Bukti pembatas** — Size dan latency kontras baseline–hybrid P029 turun bersamaan. Ini konsisten dengan salah satu kemungkinan, dengan efek peak RAM belum dapat diuji dari nilai tersebut. Pada B0 UP+Q ukuran file tabel tidak berubah meskipun sparsitas bertambah. [P029-C010, P029-C013]
- **Status** — INSUFFICIENT EVIDENCE, rendah untuk hubungan empiris peak RAM, sedang untuk kebutuhan memisahkan metrik.
- **Belum diketahui, uji dan kelayakan** — Pada bentuk input dan precision tetap, catat byte file serialisasi, peak RSS dengan scope proses, peak allocator/arena bila tersedia, dan timing. Observasi memori proses realistis di SBC; instrumentasi flash/SRAM MCU mensyaratkan part, build map dan arena yang teridentifikasi.
- **Literatur tambahan** — [[02-Literature/Paper-Reviews/P024]] untuk deployment/memori, [[02-Literature/Paper-Reviews/P008]] untuk pembanding precision.

Lihat [[03-Synthesis/PRUNING_ARCHITECTURE_SYNTHESIS]], [[03-Synthesis/Research-Directions/INITIAL_PRUNING_RESEARCH_DIRECTIONS]] dan [[02-Literature/VERIFICATION_BACKLOG]] untuk hubungan dengan evidence dan hal yang perlu diuji.
