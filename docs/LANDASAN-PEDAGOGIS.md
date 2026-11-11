# Landasan Pedagogis

Dokumen ini menjelaskan **mengapa** setiap fitur ada: dari temuan wawancara, ke teori, ke keputusan desain.

## 1. Teori Beban Kognitif (Cognitive Load Theory)

| Jenis beban | Arti | Kode wawancara | Strategi aplikasi |
|---|---|---|---|
| **Intrinsik (IL)** | Kesulitan yang melekat pada materi (banyak elemen yang harus diolah bersamaan) | IL01–IL05 | Pecah materi (segmenting), urutan dari mudah ke sulit, alat bantu urut otomatis |
| **Ekstrinsik (EL)** | Beban dari cara penyajian yang buruk | EL01–EL03 | Tampilan polos, satu langkah per layar, tabel ringkas, tanpa hiasan di zona belajar |
| **Germane (GL)** | Usaha mental yang membangun skema/pemahaman | GL01–GL04 | Worked example, faded example, petunjuk bertahap, contoh nyata, refleksi "kapan dipakai" |

### Prinsip multimedia yang diterapkan

| Prinsip (Mayer/Sweller) | Penerapan | Temuan |
|---|---|---|
| **Segmenting** | `StepPlayer`: 1 langkah per layar dengan tombol Lanjut/Kembali | DS02, EL03 |
| **Coherence** | Tidak ada gambar/animasi dekoratif di layar belajar | EL02, DS03 |
| **Signaling** | Sorot angka yang sedang diproses; sorot data relevan di soal cerita | IL05 |
| **Spatial contiguity** | Rumus dan angka ditampilkan berdekatan, bukan terpisah | EL01 |
| **Worked example effect** | Contoh lengkap sebelum latihan | GL01 |
| **Fading / expertise reversal** | Bantuan dikurangi bertahap: contoh lengkap → setengah → mandiri | GL03 |
| **Pre-training** | Kenalan konsep + istilah (n, urut, frekuensi) sebelum hitung | K01 |
| **Feedback elaboratif** | Umpan balik menjelaskan letak kesalahan | DS04 |

## 2. Literasi Matematika

Mengacu pada kerangka PISA dan kisi-kisi instrumen yang dipakai dalam penelitian:

| Indikator | Arti | Bentuk di aplikasi |
|---|---|---|
| **Mathematical Reasoning** | Membandingkan alternatif, memberi alasan, memutuskan | Dunia 4: pilih ukuran + pilih alasan (mis. stok ukuran sepatu) |
| **Formulating** | Menerjemahkan situasi nyata ke model matematika | Soal cerita: pilih data relevan, susun model (mis. total tinggi baru = 850 − 168 + 182) |
| **Employing** | Memakai konsep & prosedur dengan benar | Kalkulator langkah, kuis hitung, Jembatan Urutan |
| **Interpreting & Evaluating** | Menafsirkan hasil dalam konteks, menilai kewajaran | Soal outlier gaji: apakah mean mewakili? Kalimat makna di akhir setiap hitungan |

Setiap hasil hitung di aplikasi diakhiri dengan **kalimat makna**, contoh: *"Rata-rata nilai 80 artinya jika nilai dibagi rata, setiap siswa mendapat 80."* Ini melatih interpretasi, bukan sekadar menghasilkan angka.

## 3. Matriks temuan → fitur → cara uji

| Kode | Temuan singkat | Fitur | Cara menguji |
|---|---|---|---|
| K01, IL03 | Mean/median/modus dianggap sama, tertukar | Kartu Pembeda, Dunia 4, soal "pilih ukuran" | Soal membedakan konsep di berbagai konteks |
| K03, IL02 | Median sulit: lupa urut, data genap | Dunia 2 khusus median, urut otomatis, langkah genap eksplisit, game Jembatan Urutan | Bandingkan ketepatan median ganjil vs genap |
| IL01 | Salah jumlah, lupa bagi, salah n | Langkah mean: hitung n dulu, jumlah, bagi; penghitung n otomatis | Kuis mean dengan data 8–15 angka |
| IL04 | Butuh lancar operasi & teliti | Mode "Coba dulu" di kalkulator, cek ulang | Rasio langkah benar pada mode coba |
| IL05 | Bingung data relevan di soal panjang | Penyorot soal cerita, soal "ketuk data penting" | Ketepatan memilih data relevan |
| EL01 | Tabel rapi membantu, tabel besar membingungkan | Tabel frekuensi maks. 8 baris per layar, data acak langsung bisa diurutkan | Observasi kelancaran |
| EL02, DS03 | Warna/hiasan ramai mengganggu | Zona belajar polos, palet terbatas, mode tenang | Angket kenyamanan tampilan |
| EL03, DS02 | Langkah sekaligus membingungkan | `StepPlayer` 1 langkah/layar | Observasi + skala beban kognitif |
| GL01, GL03 | Pembahasan & petunjuk bertahap membantu | Worked/faded example, 3 tingkat petunjuk | Siswa mengerjakan soal serupa tanpa bantuan |
| GL02 | Memeriksa jawaban dengan hitung ulang | Tombol "Cek dengan kalkulator" setelah kuis | Frekuensi pemakaian |
| GL04 | Contoh nyata membantu membedakan | Konteks nyata tiap konsep | Soal konteks bervariasi |
| DS01 | Kalkulator dengan langkah + urut otomatis + contoh | Kalkulator Langkah, contoh soal siap pakai | Uji fungsi + observasi |
| DS04 | Latihan bertingkat + umpan balik letak salah | Kuis mudah→sulit, katalog miskonsepsi | Kemampuan memperbaiki jawaban setelah umpan balik |

## 4. Tema game vs beban kognitif

Tema petualangan menaikkan motivasi, tetapi hiasan yang ramai menaikkan beban ekstrinsik (EL02). Penyelesaiannya:

- **Zona main** (peta, game, layar hadiah): tema penuh, warna cerah, sprite.
- **Zona belajar** (materi, kalkulator, kuis): latar polos, satu tokoh pemandu kecil yang **diam** di pojok, aksen warna dunia hanya pada tombol & penanda langkah.
- Transisi ke zona belajar diberi jeda tenang (tanpa animasi panjang).

Ini juga bisa dijadikan variabel yang diamati dalam penelitian.
