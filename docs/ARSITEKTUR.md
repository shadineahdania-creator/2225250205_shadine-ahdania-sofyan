# Arsitektur Teknis

## 1. Pilihan teknologi

Satu basis kode web yang juga dibungkus menjadi aplikasi Android.

| Kebutuhan | Pilihan | Alasan |
|---|---|---|
| Bahasa | **TypeScript** | Mencegah salah tipe pada data angka |
| UI | **React 18 + Vite** | Populer, banyak tutorial, build cepat |
| Gaya | **Tailwind CSS** | Konsisten dengan token warna |
| State | **Zustand** | Sederhana dibanding Redux |
| Penyimpanan lokal | **Dexie (IndexedDB)** | Offline, tahan lama, bisa ekspor |
| Web offline | **vite-plugin-pwa** | Bisa dipasang dari browser, jalan tanpa internet |
| Android | **Capacitor** | Membungkus build web menjadi APK |
| Game | **DOM + CSS / Canvas 2D** (tanpa engine) | Mini-game sederhana; ringan untuk HP murah. Phaser hanya jika butuh fisika kompleks |
| Seret-lepas | **@dnd-kit** | Dukung sentuhan untuk urutkan data |
| Uji | **Vitest**, Testing Library, **Playwright** | Unit, komponen, end-to-end |

Alternatif yang dipertimbangkan: Flutter (performa bagus, tetapi kurva belajar Dart) dan Kodular/MIT App Inventor (mudah, tetapi sulit membuat kalkulator langkah & game kustom). React + Capacitor dipilih karena satu kode untuk web & Android dan sesuai pengalaman Python/pemrograman dasar.

## 2. Struktur folder

```
statiquest/
├─ CLAUDE.md
├─ PRD.md
├─ docs/
├─ public/              # ikon PWA, manifest
├─ android/             # dibuat oleh Capacitor
└─ src/
   ├─ core/
   │  ├─ stats/
   │  │  ├─ mean.ts          # hitungMean + langkahMean
   │  │  ├─ median.ts
   │  │  ├─ modus.ts
   │  │  ├─ urutkan.ts       # langkah pengurutan untuk animasi
   │  │  ├─ format.ts        # formatAngka, parseAngka (koma desimal)
   │  │  └─ *.test.ts
   │  └─ feedback/
   │     ├─ misconceptions.ts # katalog kode + teks umpan balik
   │     └─ diagnose.ts       # jawaban salah → kode miskonsepsi
   ├─ content/
   │  ├─ materi/dunia-1.ts …
   │  ├─ soal/dunia-1.json …
   │  └─ game/level-*.json
   ├─ features/
   │  ├─ peta/  materi/  kalkulator/  kuis/  game/  profil/  pengaturan/
   ├─ components/        # StepPlayer, NumberChip, FeedbackPanel, ...
   ├─ layouts/           # PlayLayout, LearnLayout
   ├─ store/             # progres, pengaturan (Zustand + Dexie)
   └─ assets/            # sprite, audio, CREDITS.md
```

## 3. Mesin hitung (`core/stats`)

Setiap fungsi mengembalikan **hasil** dan **daftar langkah** yang siap ditampilkan `StepPlayer`.

```ts
type Langkah = {
  judul: string;            // "Urutkan data"
  penjelasan: string;       // ≤ 25 kata
  data?: number[];          // keadaan data pada langkah ini
  sorot?: number[];         // indeks yang disorot
  rumus?: string;           // "(80 + 80) / 2 = 80"
  tabel?: { nilai: number; frekuensi: number }[];
};

type HasilStatistik<T> = { hasil: T; langkah: Langkah[]; makna: string };

langkahMedian(data: number[]): HasilStatistik<number>;
langkahMean(data: number[]): HasilStatistik<number>;
langkahModus(data: number[]): HasilStatistik<number[]>; // [] = tidak ada modus
```

Aturan:
- Fungsi murni; tidak mengubah array masukan.
- Hindari galat floating point: jumlahkan dengan pembulatan akhir `Math.round(x * 1e6) / 1e6`, tampilkan via `formatAngka` (maks. 2 desimal; bisa diatur).
- `diagnose(soal, jawabanSiswa)` membandingkan jawaban siswa dengan nilai-nilai "jawaban salah yang khas" (mean saat diminta median, elemen tengah data belum urut, dll.) dan mengembalikan kode miskonsepsi.

## 4. Model data (IndexedDB)

```ts
Profil      { id, namaPanggilan, avatar, dibuat }
Progres     { profilId, nodeId: "D2-kuis", status: "terkunci"|"terbuka"|"selesai", bintang, skorTerbaik, diperbarui }
Percobaan   { id, profilId, soalId, jawaban, benar, misconceptionId?, petunjukDipakai, durasiMs, waktu }
SkorLiterasi{ profilId, soalId, indikator, skorTahap: number[], total, waktu }
Pengaturan  { suara, animasi, ukuranHuruf, tema }
```

`Percobaan` dan `SkorLiterasi` dipakai untuk analisis penelitian (ekspor CSV dari menu Profil dengan PIN peneliti).

## 5. Navigasi (rute)

```
/                → layar judul
/peta            → peta dunia
/dunia/:d/materi → materi (StepPlayer)
/dunia/:d/kuis   → kuis
/dunia/:d/game   → mini-game
/kalkulator      → kalkulator langkah
/profil          → progres & ekspor
/pengaturan
```

Memakai `HashRouter` agar aman di Capacitor & hosting statis.

## 6. Build & distribusi

| Target | Cara |
|---|---|
| Web | `npm run build` → unggah `dist/` ke GitHub Pages / Netlify / Vercel (gratis) |
| Android | `npm run build && npx cap sync android` → Android Studio → *Build APK*. Untuk Play Store: *Build Bundle (AAB)* + tanda tangan |

Kebutuhan perangkat pengembang: Node.js 20 LTS, Android Studio (SDK 34), Git.
