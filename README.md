# StatiQuest — Petualangan Mean, Median, Modus

> Nama kerja (*working title*). Boleh diganti.

Aplikasi belajar statistika dasar (mean, median, modus) untuk siswa SMP kelas VIII–IX, berjalan di **web (PWA)** dan **Android (APK)**. Aplikasi ini dirancang dari hasil wawancara siswa (beban kognitif) dan diarahkan untuk mengembangkan **literasi matematika**.

Aplikasi punya 3 komponen utama: **Materi**, **Kuis**, dan **Game**, dengan tema **petualangan platformer retro 8-bit** (orisinal, terinspirasi gaya game platformer klasik).

## Cara menjalankan

Butuh **Node.js 20 LTS** (unduh dari nodejs.org).

```bash
npm install      # sekali saja
npm run dev      # buka http://localhost:5173 di browser
npm run test     # jalankan semua test
npm run build    # hasil siap online di folder dist/
```

**Membuat APK Android** (butuh Android Studio):

```bash
npx cap add android     # sekali saja
npm run android:sync    # setiap kali kode berubah
npm run android:open    # di Android Studio: Build > Build APK(s)
```

**Mengubah isi materi/soal:** cukup edit file di `src/content/` (`materi.ts`, `soal.ts`, `situasiBos.ts`). Tidak perlu menyentuh kode komponen.

**Menu peneliti:** Profil → Menu peneliti → PIN `2026` → ekspor CSV jawaban dan skor literasi. Ganti PIN di `src/features/profil/ProfilPage.tsx`.

## Isi dokumen perencanaan

| File | Isi | Dibaca oleh |
|---|---|---|
| [PRD.md](PRD.md) | Kebutuhan produk: masalah, pengguna, fitur, kriteria keberhasilan | Pengembang, dosen, penguji |
| [CLAUDE.md](CLAUDE.md) | Aturan kerja untuk AI coding assistant (Claude Code) saat membangun aplikasi | Claude Code / AI assistant |
| [docs/LANDASAN-PEDAGOGIS.md](docs/LANDASAN-PEDAGOGIS.md) | Teori beban kognitif + literasi matematika, dan pemetaan temuan wawancara ke fitur | Dosen, pengembang |
| [docs/KONTEN-MATERI.md](docs/KONTEN-MATERI.md) | Rancangan isi Materi per layar | Pengembang konten |
| [docs/KUIS-DAN-GAME.md](docs/KUIS-DAN-GAME.md) | Bank soal, katalog miskonsepsi, umpan balik, desain game | Pengembang |
| [docs/DESAIN-UI.md](docs/DESAIN-UI.md) | Tema visual, warna, tipografi, komponen, aturan tampilan | Desainer, pengembang |
| [docs/ARSITEKTUR.md](docs/ARSITEKTUR.md) | Teknologi, struktur folder, model data, mesin hitung | Pengembang |
| [docs/ROADMAP.md](docs/ROADMAP.md) | Tahapan pengerjaan per sprint | Pengembang |
| [docs/PENGUJIAN.md](docs/PENGUJIAN.md) | Rencana uji teknis, uji kelayakan, dan uji coba ke siswa | Pengembang, peneliti |

## Urutan membaca yang disarankan

1. `PRD.md` → gambaran utuh.
2. `docs/LANDASAN-PEDAGOGIS.md` → alasan di balik setiap fitur.
3. `docs/ARSITEKTUR.md` + `docs/ROADMAP.md` → mulai membangun.
4. `CLAUDE.md` → letakkan di root proyek kode agar Claude Code mengikutinya.

## Sumber data

- Laporan hasil wawancara siswa S01 (Kelas IX, 4 November 2026), kode K/IL/EL/GL/DS.
- Kisi-kisi instrumen literasi matematika (4 indikator, skor maks. 24).
- Referensi visual tema platformer (lihat catatan hak cipta di `docs/DESAIN-UI.md`).
