# Rencana Pengujian

## 1. Uji unit mesin hitung (`core/stats`)

| # | Data | Mean | Median | Modus | Catatan |
|---|---|---|---|---|---|
| 1 | 9, 3, 7, 5, 1 | 5 | 5 | tidak ada | ganjil, tidak urut |
| 2 | 75, 80, 65, 90, 75, 85, 95, 70, 80, 80 | 79,5 | 80 | 80 | kisi-kisi no. 3 |
| 3 | 38, 39, 40, 40, 40, 41, 41, 42 | 40,125 | 40 | 40 | kisi-kisi no. 1 (mean 3 desimal) |
| 4 | 2000000, 2000000, 2500000, 3000000, 15000000 | 4900000 | 2500000 | 2000000 | kisi-kisi no. 4, outlier |
| 5 | 4, 8, 6, 2 | 5 | 5 | tidak ada | genap, dua tengah berbeda |
| 6 | 2, 3, 3, 5, 5, 6 | 4 | 4 | 3 dan 5 | dua modus |
| 7 | 7, 7, 7 | 7 | 7 | tidak ada* | semua sama |
| 8 | −3, 0, 2, −1 | −0,5 | −0,5 | tidak ada | negatif |
| 9 | 1,5; 2,25; 3 | 2,25 | 2,25 | tidak ada | desimal |
| 10 | 12, 7, 15, 9, 10, 8 | 10,17 (≈10,1667) | 9,5 | tidak ada | soal D2-05 |
| 11 | 168 → 182 pada tim 5×170 | 172,8 | — | — | kisi-kisi no. 2 (uji soal formulating) |

\* Kasus 7 perlu diputuskan dan dijelaskan konsisten di materi. Usulan: "Semua nilai sama → modus = 7" lebih umum di buku SMP; tetapkan bersama dosen lalu kunci di unit test.

Uji tambahan: `parseAngka("1,5; 2")`, input kosong, input huruf, > 30 data, angka di atas batas, urutan langkah median genap selalu memuat langkah "rata-rata dua nilai tengah".

## 2. Uji `diagnose()` miskonsepsi

| Soal | Jawaban siswa | Kode yang diharapkan |
|---|---|---|
| Median 12, 7, 15, 9, 10, 8 | 15 | `MED_TIDAK_URUT` |
| Median 12, 7, 15, 9, 10, 8 | 19 | `MED_GENAP_JUMLAH` |
| Median 12, 7, 15, 9, 10, 8 | 10,17 | `TERTUKAR_MEAN_MEDIAN` |
| Mean 70, 80, 90, 60, 75 | 375 | `MEAN_LUPA_BAGI` |
| Mean 70, 80, 90, 60, 75 | 93,75 | `MEAN_SALAH_N` |
| Modus 7, 8, 7, 9, 8, 7, 6 | 9 | `MODUS_TERBESAR` |
| Modus 7, 8, 7, 9, 8, 7, 6 | 3 | `MODUS_FREKUENSI` |

## 3. Uji end-to-end (Playwright)

- Siswa baru → hanya Dunia 1 terbuka → selesaikan materi → kuis ≥ 70% → game terbuka → Dunia 2 terbuka.
- Tutup & buka ulang aplikasi → progres tetap.
- Mode offline (jaringan dimatikan) → semua rute bisa dibuka.
- Lebar 360 px dan 1280 px: tidak ada scroll horizontal.
- Mode tenang → tidak ada animasi.

## 4. Uji kesesuaian dengan prinsip desain (checklist manual)

- [ ] Tidak ada layar belajar yang menampilkan > 1 langkah hitung.
- [ ] Tidak ada blok teks > ±25 kata di zona belajar.
- [ ] Setiap umpan balik salah menyebut jenis kesalahan.
- [ ] Benar/salah ditunjukkan dengan ikon + teks, bukan warna saja.
- [ ] Tidak ada aset/nama milik game komersial; `CREDITS.md` lengkap.

## 5. Uji dengan pengguna (sesuai matriks wawancara)

| Fitur | Cara uji | Instrumen |
|---|---|---|
| Pengenalan konsep & penyorot soal cerita | Beri soal berbagai konteks; amati apakah siswa membedakan konsep & menemukan data relevan | Lembar observasi, log `pilihData` |
| Kalkulator langkah & urut otomatis | Data ganjil & genap; amati ketepatan mengikuti proses | Observasi, log mode "Coba dulu" |
| Tampilan sederhana, 1 langkah/layar | Uji navigasi; amati kebingungan | Think-aloud, skala beban kognitif (Paas 1–9) |
| Tutorial & petunjuk bertahap | Siswa belajar tutorial lalu kerjakan soal serupa tanpa bantuan; minta jelaskan langkahnya | Rubrik penjelasan lisan |
| Latihan bertingkat & koreksi | Amati ketepatan, kemampuan memperbaiki setelah umpan balik, kemandirian | Log `Percobaan` (benar setelah salah pertama) |
| Literasi matematika | Pretest–posttest dengan instrumen kisi-kisi (soal paralel) | Skor 0–24, per indikator; N-gain |
| Kelayakan | Ahli materi & ahli media | Angket skala Likert |
| Kepraktisan | Siswa & guru | Angket kepraktisan |

## 6. Kriteria lulus rilis v1

- Semua uji unit & e2e lulus.
- Checklist §4 lengkap.
- Kelayakan ahli ≥ kategori "Layak".
- Tidak ada temuan "sangat membingungkan" dari uji perorangan yang belum direvisi.
