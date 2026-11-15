# Architecture

> Architecture = BAGAIMANA web ini dibangun.
> Apa yang dibangun ada di `PRD.md`. Tampilannya ada di `DESIGN.md`. Aturan keamanan ada di `SECURITY.md`.

## 1. Teknologi

| Kebutuhan | Pilihan | Catatan |
|---|---|---|
| Kerangka web | Next.js (App Router) | Satu proyek untuk tampilan dan endpoint chatbot |
| Bahasa | TypeScript | Strict mode aktif |
| Gaya tampilan | Tailwind CSS | Warna dan ukuran lewat design token, lihat `DESIGN.md` |
| Animasi komponen | Motion | Muncul, hover, transisi kartu |
| Animasi berurutan dan scroll | GSAP | Intro hero, efek yang digerakkan scroll |
| 3D | three.js | Satu scene di hero saja, dimuat belakangan |
| Komponen animasi siap pakai | React Bits (varian TypeScript + Tailwind) | Disalin sebagai kode sumber ke proyek, bukan paket, dan ditinjau dulu |
| Isi portfolio | File data bertipe (TypeScript/JSON), konten panjang boleh MDX | Tanpa database |
| Chatbot | Gemini API lewat endpoint server Next.js | Paket gratis, key hanya di server |
| Hosting | Vercel | Preview deployment sebelum production |
| Kontrol versi | Git + GitHub | Repository `0xims` |

**Tidak dipakai (dan alasannya):**
- Popmotion: sudah tercakup Motion.
- mo.js: efek serupa bisa dibuat dengan GSAP. Boleh dipertimbangkan lagi nanti dengan alasan jelas di `DECISIONS.md`.
- Database: isi web cukup berupa file.

## 2. Alur Besar

```
Pengunjung
   ↓
Halaman Next.js (tampilan, animasi)
   ↓ membaca
Data portfolio (src/data)

Chatbot:
Pengunjung → Jendela chat → /api/chat (server Next.js) → Gemini
                                  ↑
                  GEMINI_API_KEY + validasi input + batas penggunaan
```

## 3. Struktur Folder

```
0xims/
├── docs/                  ← dokumen perencanaan
├── public/
│   ├── sertifikat/        ← gambar sertifikat (thumbnail dan versi besar)
│   ├── belajar/           ← file belajar berukuran kecil
│   └── cv/                ← file CV
├── src/
│   ├── app/               ← halaman dan route (termasuk app/api/chat)
│   ├── components/        ← komponen UI yang dipakai ulang
│   ├── features/          ← per bagian: hero, skills, projects, certificates, library, contact, chatbot
│   ├── data/              ← SEMUA isi portfolio (satu sumber kebenaran)
│   ├── services/          ← logika di luar UI (mis. pemanggilan Gemini)
│   ├── lib/               ← util umum, konfigurasi animasi, batas penggunaan
│   ├── types/             ← tipe TypeScript
│   └── utils/
├── tests/
├── .env.example
├── .gitignore
├── README.md
├── RULES.md
└── TASKS.md
```

## 4. Aturan Arsitektur

1. **Komponen UI tidak berisi logika data.** Data dibaca dari `src/data`, logika ada di `services` atau `lib`.
2. **Satu sumber kebenaran untuk isi.** Profil, skill, proyek, sertifikat, dan file belajar hanya ditulis di `src/data`. Halaman dan chatbot membaca dari sana, jadi jawabannya selalu konsisten.
3. **Menambah konten tidak boleh mengubah kode tampilan.** Cukup menambah entri di `src/data` (dan menaruh filenya di `public`).
4. **Hanya server yang boleh memegang rahasia.** `GEMINI_API_KEY` tidak pernah diberi awalan `NEXT_PUBLIC_` dan tidak pernah dipakai di komponen browser.
5. **Satu elemen hanya dianimasikan oleh satu library.** Jangan menganimasikan elemen yang sama dengan Motion dan GSAP sekaligus.
6. **Bagian yang berat dimuat belakangan.** three.js dan chatbot dimuat dengan lazy loading.
7. **Web tetap berfungsi tanpa bagian yang berat.** Jika 3D atau chatbot gagal atau dimatikan, halaman lain tetap utuh.
8. **Reuse sebelum membuat baru.** Cek komponen yang sudah ada sebelum membuat yang baru.

## 5. Data Portfolio

- Setiap jenis data punya tipe TypeScript di `src/types` (misalnya `Project`, `Certificate`, `LibraryFile`).
- Contoh bidang minimal:
  - **Project:** id, judul, deskripsi singkat, teknologi, tautan GitHub, urutan tampil.
  - **Certificate:** id, judul, penerbit, tahun, kategori, gambar kecil, gambar besar, tautan verifikasi (opsional), tingkat relevansi.
  - **LibraryFile:** id, judul, deskripsi, kategori, jalur file atau tautan, ukuran.
- Gambar sertifikat memakai format ringan dan dimuat dengan komponen gambar bawaan Next.js.

## 6. Animasi

- **Motion:** animasi komponen dan transisi layout.
- **GSAP:** urutan animasi dan animasi berbasis scroll.
- **three.js:** satu scene di hero, dimuat dengan dynamic import.
  - Dimatikan atau diganti tampilan statis jika perangkat lemah, layar kecil, atau pengguna memilih mengurangi gerakan.
- **Wajib:** hormati pengaturan sistem untuk mengurangi gerakan (`prefers-reduced-motion`).
- **Anggaran performa (target awal, diukur ulang saat build):**
  - Informasi utama (nama, peran, tombol aksi) terlihat sebelum animasi berat selesai.
  - Elemen melayang tidak boleh menutupi tombol penting.
  - Animasi hanya memakai properti yang ringan (transform dan opacity) bila memungkinkan.

### Komponen siap pakai: React Bits

- Dipakai untuk mempercantik tampilan: teks beranimasi, latar, kartu interaktif, dan micro interaction.
- Pakai varian **TypeScript + Tailwind** agar sesuai dengan teknologi proyek.
- Disalin sebagai kode sumber ke `src/components/ui/react-bits/` (lewat CLI atau salin manual). Kodenya menjadi milik proyek, bukan paket yang diperbarui otomatis.
- **Sebelum dipakai:** baca kodenya (tidak ada pemanggilan jaringan atau skrip yang mencurigakan), cek dependensi yang dibawanya, dan cek lisensi serta ketentuan atribusi di repository resminya.
- **Dependensi baru** yang dibawa komponen (misalnya library WebGL lain) hanya boleh ditambah dengan alasan tertulis di `DECISIONS.md`.
- **Batas pemakaian:** maksimal satu latar WebGL/canvas per halaman. Jika latar dari React Bits dipilih untuk hero, ia menggantikan scene three.js buatan sendiri, bukan ditumpuk di atasnya.
- Pilih sedikit komponen dan sesuaikan warna, ukuran, dan geraknya agar tidak terlihat seperti template.
- Semua komponen tetap tunduk pada aturan `prefers-reduced-motion` dan lazy loading.

## 7. Chatbot

**Endpoint:** `src/app/api/chat/route.ts` (hanya server).

**Cara kerja:**
1. Terima pesan pengunjung dan validasi (panjang maksimum, tipe data).
2. Cek batas penggunaan.
3. Susun permintaan ke Gemini: instruksi sistem yang membatasi ruang lingkup + data portfolio dari `src/data` sebagai konteks + pesan pengunjung.
4. Kembalikan jawaban dengan panjang terbatas.

**Pengamanan:**
- Key hanya dari environment variable di server.
- Batas panjang input dan output.
- Instruksi sistem melarang membocorkan instruksi internal dan menolak topik di luar portfolio.
- Pesan pengunjung diperlakukan sebagai data, bukan perintah.
- Penanganan kesalahan: jika kuota habis atau Gemini gagal, tampilkan pesan sopan, bukan error mentah.
- Catatan privasi di jendela chat (permintaan paket gratis dapat dipakai Google untuk meningkatkan produknya; pengunjung jangan mengetik data pribadi).

**Keputusan terbuka:** cara menyimpan hitungan batas penggunaan di lingkungan serverless. Pilihan awal ada di bagian 11.

## 8. Hosting dan Lingkungan

- **Hosting:** Vercel, tersambung ke repository GitHub.
- **Alur:** lokal → preview deployment → uji → production.
- **Environment variable:**
  - Lokal: `.env.local` (tidak masuk Git).
  - Preview dan production: diisi lewat pengaturan Vercel, bisa berbeda nilai.
- `.env.example` berisi nama variabel tanpa isi.

## 9. Keamanan Dasar (rincian di `SECURITY.md`)

- `.env*` ada di `.gitignore` sejak commit pertama.
- Tidak ada rahasia di kode, di prompt ke AI coding, atau di repository.
- Header keamanan dan kebijakan konten dipasang sebelum production.
- Kontak diatur agar tidak mengundang spam.
- **Kebijakan dependensi:** setiap paket baru diperiksa dulu di registry resminya (nama, penerbit, jumlah pemakai, tanggal rilis) sebelum dipasang, karena AI kadang mengarang nama paket. Versi dikunci lewat lockfile, dan paket yang baru dirilis diberi jeda sebelum dipakai.

## 10. Pengujian (rincian di `TEST_PLAN.md` nanti)

- Lint, type check, dan build harus lolos sebelum commit penting.
- Alur utama diuji lewat uji end-to-end (Playwright) pada tahap pengujian.
- Uji manual di lebar 375px, 768px, dan 1440px.

## 11. Keputusan Terbuka

1. **Batas penggunaan chatbot:** butuh penyimpanan kecil bersama agar batas per pengunjung tetap berlaku di server tanpa ingatan. Opsi gratis akan dicek saat mengerjakan fitur chatbot. Sebagai lapisan dasar: batas global harian dan batas per sesi.
2. **Jalur kontak:** tautan langsung (email, LinkedIn) atau formulir. Tautan langsung lebih sederhana dan tidak butuh backend.
3. **Ukuran file belajar:** file kecil disimpan di `public/belajar`, file besar dihubungkan lewat tautan eksternal.
4. **three.js:** dipakai langsung atau lewat pembungkus React, diputuskan saat implementasi hero.
5. **Domain pribadi:** opsional.
