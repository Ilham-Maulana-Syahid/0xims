# Tasks

> Kerjakan **satu tugas per permintaan**, urut dari atas.
> Alur tiap tugas: Baca → Rencanakan → Implementasi → Tes → Review → Commit → Centang di sini.
> Aturan lengkap ada di `RULES.md`.

## Status Saat Ini

- **Posisi terakhir:** Phase 0 (dokumentasi)
- **Tugas berikutnya:** TASK-005
- **Alat yang terakhir bekerja:** -
- **Catatan serah-terima:** -

> Perbarui bagian ini setiap selesai satu tugas atau saat berpindah alat (Freebuff ↔ Copilot).

---

## Phase 0: Dokumentasi

- [x] TASK-001 `docs/PRD.md`
- [x] TASK-002 `docs/ARCHITECTURE.md`
- [x] TASK-003 `docs/DESIGN.md`
- [x] TASK-004 `RULES.md`
- [ ] TASK-005 `docs/SECURITY.md`
- [ ] TASK-006 `docs/DECISIONS.md` (catat keputusan penting yang sudah dibuat)
- [ ] TASK-007 `README.md`
- [ ] TASK-008 `.env.example` (nama variabel tanpa isi)

## Phase 1: Setup Proyek

- [ ] TASK-009 Buat proyek Next.js (TypeScript, Tailwind, App Router) tanpa menimpa `docs/`, `.gitignore`, `RULES.md`, `TASKS.md`
- [ ] TASK-010 Pasang font (Space Grotesk, Inter, JetBrains Mono) lewat fitur font bawaan Next.js
- [ ] TASK-011 Definisikan design token warna dan ukuran sesuai `DESIGN.md`
- [ ] TASK-012 Pastikan skrip `lint`, `typecheck`, dan `build` berjalan
- [ ] TASK-013 Buat struktur folder sesuai `ARCHITECTURE.md`
- [ ] TASK-014 Commit dan push
- [ ] TASK-015 Hubungkan Vercel dan buat preview deployment pertama (tanpa rahasia)

## Phase 2: Fondasi

- [ ] TASK-016 Tipe TypeScript dan kerangka data di `src/data` (isi sementara jelas bertanda kosong, **jangan mengarang data pemilik**)
- [ ] TASK-017 Komponen dasar: Button (utama dan sekunder), Card, Tag, SectionHeading
- [ ] TASK-018 Utilitas dan hook "kurangi gerakan" (`prefers-reduced-motion`)
- [ ] TASK-019 Kerangka layout dan menu atas (termasuk tombol Unduh CV)
- [ ] TASK-020 Footer
- [ ] TASK-021 Uji kerangka di 375px, 768px, 1440px

## Phase 3: Hero (alur lengkap pertama)

- [ ] TASK-022 Hero statis: nama lengkap **satu baris**, peran, ringkasan, dua tombol
- [ ] TASK-023 Foto sementara dan tata letak desktop dan mobile
- [ ] TASK-024 Lingkaran cahaya violet dan kartu kaca melayang
- [ ] TASK-025 Efek Split Text pada nama (tetap satu baris)
- [ ] TASK-026 Efek Decrypted Text pada peran
- [ ] TASK-027 Latar hero (Dark Veil atau Faulty Terminal), satu saja, dimuat belakangan, ada cadangan statis
- [ ] TASK-028 Foto bergeser sedikit mengikuti kursor (nonaktif di layar sentuh)
- [ ] TASK-029 Efek scanning pada foto
- [ ] TASK-030 Pembukaan sinematik sekali per kunjungan, bisa dilewati
- [ ] TASK-031 Uji hero di tiga lebar layar dan dengan "kurangi gerakan"

## Phase 4: Profil, Skill, Pencapaian

- [ ] TASK-032 Bagian Profil
- [ ] TASK-033 Kartu Skill (Tilted Card) dengan tag teknologi
- [ ] TASK-034 Susunan kartu Skill yang bisa digeser
- [ ] TASK-035 Bagian Pencapaian dan perjalanan belajar

## Phase 5: Proyek

- [ ] TASK-036 Kartu "Episode" (Spotlight Card) dengan penomoran
- [ ] TASK-037 Data proyek dan tautan GitHub dari `src/data`
- [ ] TASK-038 Slot kosong "arsip" untuk proyek berikutnya

## Phase 6: Sertifikat

- [ ] TASK-039 Data sertifikat dan kategori
- [ ] TASK-040 Rak "Arsip" (Masonry), sertifikat relevan di depan
- [ ] TASK-041 Tampilan besar saat diklik, gambar dimuat bertahap
- [ ] TASK-042 Filter per kategori
- [ ] TASK-043 Tautan verifikasi (jika ada)
- [ ] TASK-044 Optimasi gambar sertifikat

## Phase 7: Belajar Bareng

- [ ] TASK-045 Data file belajar dan kategori
- [ ] TASK-046 Kartu file dengan tombol unduh
- [ ] TASK-047 Filter per kategori
- [ ] TASK-048 Aturan ukuran file: yang besar lewat tautan eksternal

## Phase 8: Kontak dan CV

- [ ] TASK-049 Bagian Kontak dengan jalur yang sudah diputuskan
- [ ] TASK-050 Tombol Unduh CV
- [ ] TASK-051 Perlindungan dari spam (alamat tidak terbaca mudah oleh pengikis otomatis)

## Phase 9: Chatbot

- [ ] TASK-052 Tombol mengambang dan panel obrolan (tampilan saja)
- [ ] TASK-053 Keadaan memuat, kosong (saran pertanyaan), kuota habis, dan error
- [ ] TASK-054 Endpoint `/api/chat`: validasi input (panjang, tipe)
- [ ] TASK-055 Instruksi sistem yang membatasi ruang lingkup + konteks dari `src/data`
- [ ] TASK-056 Panggil Gemini dari sisi server, key dari environment variable
- [ ] TASK-057 Batas penggunaan (cek opsi gratis terbaru, catat keputusan di `DECISIONS.md`)
- [ ] TASK-058 Pesan sopan saat kuota habis atau Gemini gagal
- [ ] TASK-059 Catatan privasi di panel obrolan
- [ ] TASK-060 Uji penyalahgunaan: prompt injection, input sangat panjang, pertanyaan di luar topik
- [ ] TASK-061 Pastikan seluruh web tetap berfungsi jika chatbot dimatikan

## Phase 10: Penyempurnaan

- [ ] TASK-062 Audit "kurangi gerakan" di semua animasi
- [ ] TASK-063 Audit performa (ukuran gambar, pemuatan 3D, skor Lighthouse)
- [ ] TASK-064 Audit aksesibilitas (keyboard, kontras, teks alternatif, fokus)
- [ ] TASK-065 Pemeriksaan responsif akhir (375px, 768px, 1440px)

## Phase 11: Pengujian dan Review Keamanan

- [ ] TASK-066 `docs/TEST_PLAN.md`
- [ ] TASK-067 Uji end-to-end (Playwright) untuk alur utama
- [ ] TASK-068 Pindai rahasia di repository dan **riwayat Git**
- [ ] TASK-069 Header keamanan dan kebijakan konten
- [ ] TASK-070 Audit dependensi
- [ ] TASK-071 Pastikan aset bersih dari EXIF/metadata dan data pribadi
- [ ] TASK-072 Review seluruh kode terhadap semua dokumen

## Phase 12: Deploy

- [ ] TASK-073 Isi environment variable di Vercel (preview dan production)
- [ ] TASK-074 QA di preview deployment
- [ ] TASK-075 Jadikan repository Public **setelah** audit lolos
- [ ] TASK-076 Deploy ke production
- [ ] TASK-077 QA di URL live (refresh, URL langsung, HP, koneksi lambat)
- [ ] TASK-078 Domain pribadi (opsional)

## Phase 13: Pemantauan

- [ ] TASK-079 Pemantauan ringan (uptime dan error)
- [ ] TASK-080 Perbarui `README.md`, `DECISIONS.md`, dan buat `MEMORY.md`

---

## Jalur Paralel: Konten dari Pemilik (bukan tugas AI)

Disiapkan sendiri, kapan saja sebelum bagian terkait dikerjakan.

- [ ] Foto tanpa latar (PNG/WebP), terang dan jelas, **EXIF sudah dihapus**
- [ ] File CV terbaru
- [ ] Gambar sertifikat + kategori + tautan verifikasi (jika ada), dan **cek data pribadi** di dalamnya
- [ ] File belajar yang boleh dibagikan (buatan sendiri atau bebas dibagikan), bebas data pribadi
- [ ] Deskripsi singkat tiap proyek + tautan GitHub
- [ ] Teks profil dan daftar pencapaian
- [ ] Keputusan jalur kontak (email, LinkedIn, atau lainnya)
