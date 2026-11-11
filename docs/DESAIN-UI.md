# Desain UI & Tema Visual

## 1. Catatan hak cipta tentang tema Mario

Referensi yang dilampirkan (Mario, pipa hijau, blok "?", jamur merah/hijau, Goomba, bendera "M", kastil) adalah **milik Nintendo** dan dilindungi hak cipta serta merek dagang. Memakainya di aplikasi yang dibagikan, diunggah ke Play Store, atau dipublikasikan dalam penelitian berisiko pelanggaran.

**Solusinya:** ambil **suasananya**, bukan karakternya: dunia platformer retro 8-bit dengan langit biru, balok bata, koin, bendera finis, dan benteng. Ini adalah unsur umum genre platformer. Semua tokoh, nama, logo, dan ikon dibuat **orisinal**.

| Referensi | Pengganti orisinal di StatiQuest |
|---|---|
| Mario | **Kiko**: penjelajah cilik bertopi kupluk oranye, syal biru, membawa buku catatan & pensil (pemandu belajar) |
| Blok "?" | **Blok Petunjuk** kuning dengan ikon **bola lampu** (memberi petunjuk) |
| Pipa hijau | **Portal Batu** berbentuk lengkung (pindah antar-dunia) |
| Jamur merah / hijau | **Kristal Bintang** (bonus bintang) dan **Daun Hati** (tambah nyawa) |
| Goomba | **Si Kacau**: makhluk dari tumpukan angka acak yang ingin "mengacak data" (cocok dengan tema mengurutkan) |
| Bendera "M" | Bendera finis dengan lambang **Σ kecil / grafik batang** milik aplikasi |
| Kastil bata | **Benteng Keputusan** (Dunia 4) dengan gaya bata sendiri |
| Layar WELCOME | Layar judul "StatiQuest" dengan tulisan piksel orisinal |

Aturan: jangan menjiplak bentuk/warna khas tokoh Nintendo (topi merah berlogo huruf, overall biru-merah, kumis, jamur merah berbintik putih). Simpan sumber & lisensi setiap aset di `src/assets/CREDITS.md`.

Sumber aset bebas yang bisa dipakai/diadaptasi: Kenney.nl (CC0, "Pixel Platformer"), OpenGameArt (cek lisensi per aset), font "Press Start 2P" (OFL) untuk judul.

## 2. Dua zona tampilan

| | Zona main (`PlayLayout`) | Zona belajar (`LearnLayout`) |
|---|---|---|
| Layar | Judul, peta dunia, game, layar hadiah | Materi, kalkulator, kuis, pembahasan |
| Latar | Langit + awan + tanah bata (statis/parallax pelan) | **Polos** (krem terang / gelap pada mode gelap) |
| Tokoh | Animasi sprite | Kiko kecil **diam** di pojok, ekspresi berubah hanya saat umpan balik |
| Font | Judul piksel "Press Start 2P", isi font sans | **Hanya font sans** yang mudah dibaca (mis. Nunito/Inter), ≥ 16 px |
| Warna | Palet tema penuh | Maks. 3 aksen: aksen dunia, hijau benar, merah salah |
| Animasi | Boleh, tapi tidak terus-menerus di belakang teks | Hanya respons aksi ≤ 400 ms |

## 3. Palet warna

| Token | Hex | Pemakaian |
|---|---|---|
| `--langit` | `#6BB8FF` | Latar zona main |
| `--bata` | `#B5582E` | Tanah, benteng |
| `--koin` | `#F7C531` | Koin, bintang, blok petunjuk |
| `--daun` | `#3DAA5C` | Benar / sukses (+ ikon ✓) |
| `--salah` | `#D9443A` | Salah (+ ikon ✗ + teks) |
| `--kertas` | `#FFF9EE` | Latar zona belajar |
| `--tinta` | `#1F2430` | Teks utama |
| `--tinta-2` | `#5B6272` | Teks sekunder |

Aksen per dunia (dipakai di tombol & penanda langkah saja): Dunia 1 `#3DAA5C`, Dunia 2 `#4C7BE0`, Dunia 3 `#E08A2E`, Dunia 4 `#8A5CD1`.

Sediakan juga mode gelap (token yang sama didefinisikan ulang).

## 4. Tipografi

- Judul layar: 22–24 px, tebal.
- Isi: 18 px (HP), maks. ±25 kata per blok, lebar baris maks. 60 karakter.
- Angka data: **font monospace tabular** 20–24 px agar mudah dibandingkan & diurutkan.
- Opsi "huruf besar" menaikkan semua ukuran 1,25×.

## 5. Komponen inti

| Komponen | Fungsi |
|---|---|
| `StepPlayer` | Menampilkan 1 langkah/layar, indikator "Langkah 2 dari 4", tombol Kembali/Lanjut besar di bawah |
| `NumberChip` | Satu angka data; bisa disorot, dinomori (untuk menghitung n), diseret |
| `SortAnimator` | Animasi chip berpindah ke posisi urut; tombol "Putar ulang" |
| `FreqTable` | Tabel frekuensi ringkas, maks. 8 baris per layar |
| `StoryText` | Soal cerita dengan mode sorot data penting & redupkan sisanya |
| `FeedbackPanel` | Panel bawah: ikon + judul ("Hampir benar!") + 1–2 kalimat spesifik + tombol "Coba lagi" / "Lihat pembahasan" |
| `HintBlock` | Blok Petunjuk berikon bola lampu, membuka petunjuk 1→2→3 |
| `WorldNode` | Titik level di peta (terkunci / terbuka / selesai + bintang) |

## 6. Sketsa layar (wireframe teks)

**Peta dunia (zona main)**
```
┌──────────────────────────────┐
│ ★ 12   🪙 340        [⚙]      │
│   ☁        ☁                  │
│  (1)Desa ─ (2)Lembah ─ (3)Gunung ─ [4]Benteng│
│   ✓✓✓       ✓ ·  🔒             🔒          │
│ ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ │
│ [Kalkulator]       [Profil]  │
└──────────────────────────────┘
```

**Langkah median (zona belajar)**
```
┌──────────────────────────────┐
│ ← Median       Langkah 3 / 5 │
│                              │
│ n = 10, genap.               │
│ Nilai tengah ada di data     │
│ ke-5 dan ke-6.               │
│                              │
│ 65 70 75 75 [80][80] 80 85 90 95 │
│                              │
│                     (Kiko 🙂) │
│ [ Kembali ]      [ Lanjut ▶ ] │
└──────────────────────────────┘
```

**Umpan balik salah (zona belajar)**
```
│ ✗ Hampir benar!                        │
│ Datanya genap, jadi ada dua nilai      │
│ tengah. Jumlahkan keduanya lalu bagi 2.│
│ [ Coba lagi ]   [ 💡 Petunjuk ]         │
```

## 7. Audio

Efek suara pendek orisinal/CC0 (lompat, koin, benar, salah) — tidak ada musik latar di zona belajar. Semua suara bisa dimatikan.
