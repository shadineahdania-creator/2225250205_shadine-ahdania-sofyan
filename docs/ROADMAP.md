# Roadmap Pengerjaan

Asumsi: 1 sprint = 1 minggu, dikerjakan sendiri dengan bantuan Claude Code. Model pengembangan bisa diselaraskan dengan **ADDIE** atau **4D** untuk laporan penelitian.

| Tahap ADDIE | Sprint | Keluaran |
|---|---|---|
| Analysis | (selesai) | Wawancara, matriks kebutuhan |
| Design | 0 | Dokumen ini, wireframe, aset tokoh |
| Development | 1–6 | Aplikasi web + APK |
| Implementation | 7 | Uji coba siswa |
| Evaluation | 8 | Analisis data, revisi |

## Sprint 0 — Persiapan desain
- [ ] Validasi PRD dengan dosen pembimbing.
- [ ] Gambar sketsa tokoh Kiko, Si Kacau, ikon Blok Petunjuk (tangan/Piskel/Aseprite) atau pilih aset CC0.
- [ ] Wireframe di Figma (peta, StepPlayer, kuis, umpan balik).
- [ ] Tulis 100% konten Dunia 1–2 di `docs/KONTEN-MATERI.md`.

## Sprint 1 — Fondasi
- [ ] Inisialisasi proyek Vite + React + TS + Tailwind + ESLint/Prettier.
- [ ] Token warna, font, `LearnLayout`, `PlayLayout`.
- [ ] `core/stats`: mean, median, modus, urutkan, format + unit test lengkap.
- **Selesai jika**: semua kasus uji di `PENGUJIAN.md` §1 lulus.

## Sprint 2 — Kalkulator Langkah
- [ ] Input data (koma/spasi, tombol +), validasi.
- [ ] `StepPlayer`, `NumberChip`, `SortAnimator`, `FreqTable`.
- [ ] Mode "Coba dulu".
- **Selesai jika**: data ganjil/genap/dua modus tampil benar satu langkah per layar di HP 360 px.

## Sprint 3 — Materi & Peta
- [ ] Peta dunia + sistem kunci/buka node + penyimpanan progres (Dexie).
- [ ] Materi Dunia 1–4 dari `src/content/`.
- [ ] `StoryText` (penyorot soal cerita), Kartu Pembeda, slider gaji.

## Sprint 4 — Kuis
- [ ] Mesin kuis: jenis pilihan, isian, urutkan, pilih data, alasan.
- [ ] Katalog miskonsepsi + `diagnose()` + `FeedbackPanel`.
- [ ] Petunjuk 3 tingkat, pembahasan bertahap, bintang.
- [ ] Uraian bertahap Dunia 4 + `SkorLiterasi`.
- [ ] Bank soal ≥ 2× per dunia.

## Sprint 5 — Game (MVP web selesai)
- [ ] Jembatan Urutan (prioritas, konsep tersulit).
- [ ] Pemburu Koin Kembar, Timbangan Gunung, Bos Benteng.
- [ ] Hadiah & aksesori avatar.
- [ ] PWA: manifest, ikon, offline.
- **Selesai jika**: alur Dunia 1→4 bisa dimainkan penuh secara offline.

## Sprint 6 — Android & penyempurnaan
- [ ] Capacitor, build APK, uji di 2 HP Android nyata (termasuk RAM ≤ 3 GB).
- [ ] Pengaturan (suara, mode tenang, huruf besar), ekspor CSV.
- [ ] Audit aksesibilitas & performa (Lighthouse ≥ 90 PWA/Accessibility).
- [ ] Uji ahli materi & ahli media (angket kelayakan).

## Sprint 7 — Uji coba
- [ ] Uji perorangan (1–3 siswa, think-aloud) → revisi.
- [ ] Uji kelompok kecil (5–10 siswa) → angket kepraktisan + skala beban kognitif.
- [ ] (Opsional) Uji lapangan dengan pretest–posttest literasi.

## Sprint 8 — Evaluasi & revisi
- [ ] Analisis log `Percobaan`: miskonsepsi paling sering, soal tersulit.
- [ ] Revisi konten & UI.
- [ ] Rilis v1.0 (web + APK), dokumentasi pengguna untuk guru.

## Backlog v2 (setelah v1)
- Data berkelompok & histogram.
- Mode guru (kode kelas, rekap nilai) — butuh backend.
- Suara narasi untuk teks materi.
- Soal yang dibuat otomatis (data acak) untuk latihan tanpa batas.
