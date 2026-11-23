---
type: screening-protocol
version: "1.0"
scope: korpus-lokal
mode: ars-sr-screener-quick
updated: 2026-11-23
tags: [mobilenet, literature-review, thesis-progress]
---

# Protokol screening literatur MobileNet

Versi 1.0, 2026-11-23 (Asia/Jakarta). Kriteria berasal dari instruksi Phase 2 pengguna yang juga meminta eksekusi langsung. Protokol ini mendokumentasikan penerapannya sebelum pemberian label.

## Tujuan dan cakupan

Petakan studi yang membantu memahami atau menguji optimasi arsitektur MobileNet dan efisiensi inferensi mobile atau edge. Unit keputusan adalah satu Paper ID dalam [[02-Literature/PAPER_INVENTORY]]. Korpus sudah dikumpulkan pengguna. Analisis ini merupakan triase eksploratif, dengan satu reviewer AI memakai aturan ARS 3.23.1 `sr-screener` mode `quick`. Validasi manusia, kalibrasi pilot, dan penilaian independen antarreviewer belum dilakukan.

## Kriteria inklusi

1. Prioritaskan arsitektur dasar MobileNetV3/V4, modifikasi arsitekturnya, dan lightweight CNN yang memuat kontribusi efisiensi yang dapat dinilai.
2. Masukkan metode yang langsung membantu menilai atau merancang optimasi CNN mobile/edge, misalnya hardware-aware NAS, latency-aware structured pruning, channel pruning, quantization dan mixed precision, optimasi arsitektur, serta benchmarking hardware.
3. Pertimbangkan perbandingan yang menghubungkan akurasi dengan latensi, memori, ukuran model, atau biaya komputasi. Paper aplikasi dapat maju jika memiliki modifikasi relevan atau baseline dan pengukuran efisiensi yang berguna.
4. Gunakan 2023–2026 sebagai preferensi. Sumber fundamental dan pembanding yang relevan dari tahun lain tetap dapat dipertimbangkan. Preprint, survei, dan versi penulis dinilai berdasarkan isi dengan status publikasi tercatat sesuai bukti.
5. CPU, GPU, akselerator mobile, FPGA, dan edge board dapat relevan sebagai pembanding atau sumber metode. Penggunaan smartphone bersifat informatif untuk relevansi hardware.

## Keputusan dan urutan pengecualian

| Label | Kode | Aturan |
| --- | --- | --- |
| INCLUDE | INC | Hubungan langsung dengan optimasi MobileNet, efisiensi lightweight CNN, metode yang dapat diterapkan, atau benchmarking yang relevan tampak pada sumber. |
| MAYBE | UNC | Relevansi masih masuk akal tetapi rincian penentu belum bisa dinilai, atau sumber berguna sebagai konteks dengan transfer yang perlu diperiksa. |
| EXCLUDE | E1 lalu E2 | Gunakan kode pertama yang jelas terpenuhi. E1 berarti di luar fokus efisiensi model atau arsitektur untuk tesis ini dan tanpa nilai metodologis yang dapat diidentifikasi. E2 berarti penggunaan untuk aplikasi tanpa modifikasi efisiensi atau pembanding relevan setelah memeriksa bagian penentu. |

Informasi yang masih kurang dapat membuat sebuah studi tidak layak setelah penelusuran lanjutan. Pada triase ini, gunakan MAYBE bila relevansi masuk akal dan keterbatasan informasi menghalangi keputusan yang lebih kuat. Judul, jenis publikasi, dan bagian yang berhasil dibaca tetap dicatat ketika abstrak tidak tersedia. Detail yang hanya bisa dinilai pada full review, seperti pengulangan benchmark, runtime persis, kualitas baseline, dan definisi batas memori, dicatat untuk verifikasi. Ketiadaannya di abstrak tidak cukup untuk eksklusi.

Contoh uji aturan (sintetis)

- Backbone MobileNetV4 baru dengan modifikasi bottleneck dan pembanding efisiensi edge → INCLUDE.
- FPGA menggunakan mixed precision dengan pengujian CNN dan relevansi terhadap deployment yang masih perlu diperiksa → kontribusi langsung ke metode CNN dapat INCLUDE, relevansi terutama pada desain rangkaian menjadi MAYBE.
- MobileNet siap pakai untuk diagnosis dengan evaluasi akurasi saja dan tanpa modifikasi efisiensi atau baseline pembanding → EXCLUDE (E2) jika isi yang diperiksa menetapkan batas itu.

**Keyakinan** HIGH jika bukti eksplisit menetapkan label, MEDIUM jika kedekatan topik dapat dinilai dengan rincian penerapan terbatas, dan LOW jika akses teks atau bukti metodologis lemah. Keyakinan mengacu pada keputusan triase.

## Prosedur dan bukti

1. Periksa judul, abstrak, dan keywords atau index terms yang tercantum. Gunakan bagian pendahuluan, metode, eksperimen, kesimpulan, atau keterbatasan yang relevan untuk menangani ambiguitas atau mengidentifikasi perangkat dan metrik. Catat bagian yang benar-benar diperiksa.
2. Daftarkan satu keputusan per ID di [[02-Literature/Literature-Matrix/SCREENING_MATRIX]], dengan alasan, locator berbasis bagian, dan tingkat keyakinan. Utamakan parafrasa. Untuk P022, periksa keterbacaan secara lokal dan gunakan nama bagian selama status struktur belum terverifikasi.
3. `Not reported` berarti informasi absen dari bagian yang semestinya diperiksa. `Not verified` berarti bukti belum cukup untuk memastikan detail. `Not available` berarti bahan atau bagian sumber tidak tersedia.
4. Catat pembanding dan sifat pengukuran jika jelas, seperti pengukuran pada perangkat, prediksi, simulasi, atau estimasi. Skor prioritas dicatat terpisah dari keputusan screening di [[02-Literature/Reading-Queue/PRIORITY_READING_LIST]].
5. Verifikasi provenance dan isi terhadap PDF lokal. Tinjauan ini menggunakan judul, abstrak, dan bagian terpilih. Keputusan adalah dukungan triase AI dan dapat dikaji ulang peneliti sebelum dipakai sebagai seleksi final.

## Amendemen

Belum ada amendemen. Jika kriteria berubah, catat tanggal, alasan, dan Paper ID yang perlu disaring ulang.
