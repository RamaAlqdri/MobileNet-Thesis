---
type: skill-documentation
updated: 2026-11-23
tags: [mobilenet, literature-review, thesis-progress]
---

# Skill Academic Research

## Identifikasi lokal

| Bidang | Hasil |
| --- | --- |
| Nama terdaftar | `academic-research-suite` (ARS-Codex) |
| Lokasi | `/Users/ramadhanial-qadri/.codex/skills/academic-research-suite/SKILL.md` |
| Versi | `3.23.1`, cocok antara frontmatter `SKILL.md` dan `manifest.json`. Berkas `VERSION` yang disebut router absen pada salinan lokal. |
| Sumber instruksi | `SKILL.md` adalah router. Workflow ada di `ars/<workflow>/WORKFLOW.md` dengan prompt pendukung yang dipilih sesuai tahap. |
| Status penggunaan | Dokumentasi router dan kontrak lokal tersedia. Tahap awal menerapkan ingest korpus pengguna, deduplikasi, jejak asal metadata, dan preflight PDF. |

## Rute yang relevan

| Tahap | Petunjuk yang perlu dibaca |
| --- | --- |
| Inventaris dan literature review | `ars/deep-research/WORKFLOW.md`, mode `lit-review` untuk review literatur. Untuk ingest, lihat `agents/bibliography_agent.md` tentang korpus pengguna dan provenance. |
| Verifikasi bukti dan rujukan | `ars/deep-research/agents/source_verification_agent.md`, `ars/shared/references/evidence_row_protocol.md` untuk kontrak Phase E jika tahap tersebut dipakai, dan `ars/academic-paper/WORKFLOW.md` mode `citation-check` untuk pemeriksaan sitasi saat penulisan. |
| Sintesis dan research gap | `ars/deep-research/agents/synthesis_agent.md`. Bentuk matriks, bandingkan konvergensi, perbedaan dan kontradiksi, lalu nilai cakupan dan calon gap. |
| Screening studi | `ars/sr-screener/WORKFLOW.md` bila screening diminta. Phase 2 memakai mode `quick` untuk 29 PDF dengan [[02-Literature/SCREENING_PROTOCOL]] dan jejak bukti per ID. Triase satu reviewer ini belum menjalani pilot, review independen, atau verifikasi manusia. |

Alias `/ars-*` dijelaskan dalam router sebagai rute ke resep prompt. Di Codex, baca resep dan workflow yang ditunjuk sesuai tugas.

## Penerapan pada proyek ini

1. Korpus dibatasi ke sumber lokal yang diizinkan. Catat identitas, provenance, status baca, dan duplikat di [[02-Literature/PAPER_INVENTORY]]. Prioritas awal mendahului penilaian kelayakan studi.
2. Cocokkan judul, penulis, tahun atau versi, dan pengenal dengan PDF asli atau rekaman penerbit yang sudah tersedia secara lokal. Gunakan `Not verified` bila tidak ada bukti yang cukup. Status resolusi DOI, versi terbit, retraksi, atau indeks yang tak bisa diperiksa masuk kebutuhan verifikasi. Jalur web, resolver API, cross-model, dan fallback pencarian eksternal ARS dinonaktifkan oleh kebijakan proyek.
3. Sebelum mengandalkan nomor halaman sebagai bukti, jalankan `ars/scripts/pdf_read_preflight.py` dari root paket ARS. CLI menerima path PDF sebagai argumen posisi dan opsi `--output` untuk JSON. Simpan sidecar dengan nama Paper ID di `.local/ars/pdf-preflight/`, dan catat kesimpulan serta hambatan di Markdown. `PASS` hanya menilai struktur. Untuk `FAIL` atau `UNAVAILABLE`, catat alasan, verifikasi bagian atau label tabel secara lokal, dan jangan menaikkan tingkat kepastian nomor halaman tanpa pemeriksaan.
4. Untuk setiap klaim, catat Paper ID, versi, halaman PDF atau bagian, tabel atau gambar, konteks pengukuran, dan status verifikasi. Bedakan bukti yang dilaporkan penulis, interpretasi agent, dan hipotesis. Isi sintesis setelah review menyediakan bukti yang dapat dibandingkan.

## Pemeriksaan lingkungan

- Poppler 26.01.0 tersedia melalui `pdfinfo`, `pdftotext`, dan `pdftoppm`. Runtime Python bawaan Codex yang sudah terpasang menyediakan `pypdf` untuk preflight lokal. `python3` sistem tidak memiliki `pypdf`.
- Preflight dijalankan pada 29 PDF. Hasilnya 28 `PASS` dan satu `UNAVAILABLE` ([[02-Literature/Paper-Reviews/P022]]). Sidecar teknis bersifat lokal dan dikecualikan dari Git.
- Ekstraksi dibatasi pada metadata dan halaman pembuka. Layout halaman pembuka diperiksa visual untuk [[02-Literature/Paper-Reviews/P017]] dan [[02-Literature/Paper-Reviews/P025]]. [[logs/2026-11-23]] mencatat batas cakupan sesi.
