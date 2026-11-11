# Kuis dan Game

## 1. Katalog miskonsepsi

Setiap distraktor (jawaban salah yang disediakan) dan setiap kesalahan di game dipetakan ke salah satu kode berikut, sehingga umpan balik selalu spesifik (DS04).

| Kode | Miskonsepsi | Cara mendeteksi | Umpan balik (contoh) |
|---|---|---|---|
| `MED_TIDAK_URUT` | Mengambil nilai tengah dari data yang belum diurutkan | Jawaban = elemen tengah data asli | "Kamu mengambil angka tengah sebelum data diurutkan. Urutkan dulu dari kecil ke besar." |
| `MED_GENAP_SATU` | Data genap: hanya mengambil salah satu nilai tengah | Jawaban = data ke-n/2 atau ke-(n/2+1) | "Datanya genap, jadi ada dua nilai tengah. Jumlahkan keduanya lalu bagi 2." |
| `MED_GENAP_JUMLAH` | Data genap: menjumlahkan dua nilai tengah tanpa membagi 2 | Jawaban = a + b | "Dua nilai tengah sudah dijumlahkan. Langkah terakhir: bagi 2." |
| `MED_POSISI` | Menjawab posisi, bukan nilai (mis. "data ke-3" → 3) | Jawaban = (n+1)/2 | "3 adalah posisinya. Median adalah nilai yang ada di posisi itu." |
| `MEAN_LUPA_BAGI` | Menjawab jumlah, lupa membagi | Jawaban = Σx | "Itu jumlah semua data. Jangan lupa bagi dengan banyak data." |
| `MEAN_SALAH_N` | Membagi dengan n yang salah | Jawaban = Σx/(n±1) | "Coba hitung lagi banyak datanya. Ada berapa angka?" |
| `MEAN_SALAH_JUMLAH` | Salah menjumlah | Jawaban dekat Σx/n tapi tidak tepat | "Cara kamu sudah benar, tapi jumlahnya kurang tepat. Coba jumlahkan sedikit demi sedikit." |
| `TERTUKAR_MEAN_MEDIAN` | Diminta median, menghitung mean (atau sebaliknya) | Jawaban = mean saat soal minta median | "Itu rata-rata (mean). Soal meminta median, yaitu nilai tengah setelah diurutkan." |
| `MODUS_TERBESAR` | Modus dianggap nilai terbesar | Jawaban = max | "Modus bukan nilai terbesar, tetapi nilai yang paling sering muncul." |
| `MODUS_FREKUENSI` | Menjawab frekuensinya, bukan nilainya | Jawaban = frekuensi maks | "3 adalah berapa kali muncul. Modusnya adalah angka yang muncul 3 kali itu." |
| `MODUS_SATU_SAJA` | Hanya menyebut satu modus padahal ada dua | Jawaban sebagian | "Ada lebih dari satu nilai dengan frekuensi tertinggi. Semuanya adalah modus." |
| `DATA_TIDAK_RELEVAN` | Memakai data yang tidak diperlukan dari soal cerita | Pilihan data salah | "Angka ini hanya keterangan cerita, tidak termasuk data yang dihitung." |
| `PILIH_UKURAN_OUTLIER` | Memakai mean pada data dengan pencilan | Pilihan ukuran salah | "Ada nilai yang jauh lebih besar. Mean jadi tertarik ke atas. Ukuran apa yang tidak terpengaruh?" |
| `PILIH_UKURAN_TIDAK_NYATA` | Memilih hasil yang tidak bermakna di dunia nyata (ukuran 40,125) | Pilihan ukuran salah | "Apakah ada sepatu ukuran 40,125? Pilih ukuran yang benar-benar dijual dan paling laku." |

## 2. Struktur soal (skema data)

```ts
type Soal = {
  id: string;                 // "D2-07"
  dunia: 1 | 2 | 3 | 4;
  level: "mudah" | "sedang" | "sulit";
  literasi?: "reasoning" | "formulating" | "employing" | "interpreting";
  jenis: "pilihan" | "isian" | "urutkan" | "pilihData" | "alasan" | "uraianBertahap";
  teks: string;               // maks. ±40 kata; soal cerita boleh lebih, wajib punya `sorotan`
  sorotan?: [number, number][]; // rentang karakter data penting (IL05)
  data?: number[];
  jawaban: number | number[] | string;
  distraktor?: { nilai: number | string; misconceptionId: string }[];
  petunjuk: [string, string, string]; // 3 tingkat
  pembahasan: string[];       // 1 langkah per elemen (ditampilkan via StepPlayer)
};
```

## 3. Komposisi kuis per dunia

| Dunia | Mudah | Sedang | Sulit | Catatan |
|---|---|---|---|---|
| 1 Modus | 3 | 3 | 2 | Termasuk dua modus & tanpa modus |
| 2 Median | 3 | 4 | 3 | ≥ 50% soal data genap; 1 soal "urutkan" seret |
| 3 Mean | 3 | 3 | 3 | Termasuk soal mencari data hilang dari mean & 1 soal cerita panjang |
| 4 Literasi | — | — | 4 uraian bertahap | Sesuai kisi-kisi, skor 0–6 per soal |

### Contoh soal

**D2-05 (sedang, employing, isian)**
Data: 12, 7, 15, 9, 10, 8. Tentukan median!
- Jawaban: 9,5 (urut: 7, 8, 9, 10, 12, 15 → (9+10)/2)
- Distraktor/deteksi: 15 atau 9 dari data asli tengah (`MED_TIDAK_URUT`: elemen ke-3/4 data asli = 15, 9), 9 atau 10 (`MED_GENAP_SATU`), 19 (`MED_GENAP_JUMLAH`), 10,17 (`TERTUKAR_MEAN_MEDIAN`)
- Petunjuk: (1) "Urutkan dulu, lalu hitung banyak data." (2) "Urutan: 7, 8, 9, 10, 12, 15. n = 6, genap." (3) "Contoh: 2, 4, 6, 8 → (4+6)/2 = 5."

**D3-08 (sulit, formulating, isian)**
Rata-rata nilai 4 siswa adalah 80. Setelah nilai Rani masuk, rata-ratanya menjadi 82. Berapa nilai Rani?
- Jawaban: 90 (5 × 82 − 4 × 80)
- Distraktor: 82 (`TERTUKAR` rata-rata dengan data), 2 (selisih saja)

**D4-01 (uraian bertahap, reasoning)** — soal sepatu kisi-kisi no. 1. Dipecah menjadi tahapan interaktif yang memetakan rubrik:

| Tahap rubrik | Interaksi di aplikasi | Skor |
|---|---|---|
| 1 Identifikasi masalah | Pilih: "Masalahnya adalah …" (pilihan ganda) | 1 |
| 2 Analisis mean/median/modus | Isi median dan modus (mean diberikan) | 2 (1 per isian) |
| 3 Bandingkan, sadari 40,125 tidak nyata | Pilih pernyataan yang benar | 1 |
| 4 Putuskan modus | Pilih ukuran pemusatan | 1 |
| 5 Alasan | Pilih alasan dari 4 pilihan | 1 |

Skor parsial: kesalahan di satu tahap **tidak** menghapus skor tahap lain (sesuai pedoman penskoran). Soal no. 2–4 kisi-kisi dipecah dengan cara yang sama. Skor disimpan per indikator untuk analisis penelitian.

> Catatan: untuk penelitian, instrumen pretest/posttest sebaiknya diberikan **di luar** aplikasi (kertas) atau dengan soal paralel, agar tidak bias karena siswa sudah melihat soal yang sama di Dunia 4.

## 4. Sistem petunjuk & penilaian

- Petunjuk 1/2/3 mengurangi bintang (3 → 2 → 1), bukan skor benar/salah.
- Salah pertama: umpan balik spesifik + coba lagi. Salah kedua: tampilkan pembahasan bertahap.
- Kelulusan kuis: ≥ 70% benar. Bisa diulang tanpa batas; soal diacak dari bank (bank ≥ 2× jumlah soal per kuis).

## 5. Desain Game

Semua game memakai **tokoh orisinal** (lihat `docs/DESAIN-UI.md`) dan berjalan di `PlayLayout`. Kontrol: ketuk/seret saja.

### 5.1 Pemburu Koin Kembar (Dunia 1 – Modus)
- Tokoh berlari otomatis di jalur pendek; siswa mengetuk untuk melompat dan mengumpulkan **koin berangka**.
- Di akhir level muncul kotak penghitung (tabel frekuensi otomatis dari koin yang terkumpul).
- Tantangan: "Angka mana yang paling sering kamu kumpulkan?" → pilih.
- Level sulit: koin dikumpulkan tanpa tabel otomatis; siswa mengisi turus sendiri.
- Kesalahan → `MODUS_TERBESAR`, `MODUS_FREKUENSI`, `MODUS_SATU_SAJA`.

### 5.2 Jembatan Urutan (Dunia 2 – Median)
- Jurang dengan balok angka berserakan. Siswa **menyeret** balok ke slot jembatan dari kecil ke besar.
- Balok yang salah tempat bergetar dan menunjukkan petunjuk ("Angka ini lebih kecil dari tetangganya").
- Setelah jembatan jadi: "Lompat ke balok tengah!" — data ganjil: 1 balok; data genap: 2 balok bersinar, siswa mengisi rata-ratanya.
- Level: 5 balok (ganjil) → 6 balok (genap) → 10 balok desimal/negatif.

### 5.3 Timbangan Gunung (Dunia 3 – Mean)
- Ada beberapa peti dengan tumpukan koin berbeda tinggi. Siswa memindahkan koin antar-peti sampai **semua sama tinggi** → tinggi akhir = mean.
- Setelah itu ditunjukkan cara cepat: jumlah ÷ n, menghubungkan pengalaman dengan rumus.
- Level sulit: "Peti ke-5 tersembunyi. Rata-rata harus 6. Berapa isinya?"

### 5.4 Bos Benteng: Si Kacau (Dunia 4 – Literasi)
- Bos "Si Kacau" (makhluk orisinal dari tumpukan angka acak) melempar **kartu situasi**: mis. "Toko ingin tahu ukuran kaos yang harus distok."
- Siswa memilih "senjata": **Mean / Median / Modus**, lalu memilih **alasan**.
- Benar → bos kehilangan 1 bar nyawa. Salah → umpan balik `PILIH_UKURAN_*` dan pemain kehilangan 1 hati.
- 6–8 kartu situasi per pertarungan, diacak dari bank 20 situasi.

### 5.5 Hadiah
- Bintang 0–3 per level; koin untuk membuka aksesori avatar (topi, warna syal) — murni kosmetik.
- Bendera di akhir level (dengan lambang aplikasi sendiri) menandakan level selesai.
