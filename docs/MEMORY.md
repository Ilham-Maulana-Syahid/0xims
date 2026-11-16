# Project Memory

> MEMORY = kondisi proyek SAAT INI. Perbarui setiap selesai satu tugas atau sebelum berpindah alat.
> Keputusan permanen ada di `DECISIONS.md`. Daftar tugas ada di `TASKS.md`.

## Status Saat Ini

- **Fase:** Phase 1, setup proyek
- **Kode aplikasi:** proyek Next.js sudah dibuat (TASK-009), masih halaman kosong bawaan scaffold
- **Lingkungan:** Windows dengan PowerShell, folder proyek `$HOME\projects\0xims`; Node.js v26.13.1, npm 11.20.0
- **Repository:** GitHub `0xims`, cabang `main`, terhubung; semua dokumen perencanaan sudah ter-commit. Perubahan TASK-009 belum di-commit (menunggu TASK-014).
- **Identitas Git:** memakai email noreply GitHub

## Sudah Selesai

- Perencanaan produk (Step 1 sampai 12 panduan): masalah, pengguna, tujuan, MVP, di luar cakupan
- Dokumen tersusun: `PRD.md`, `ARCHITECTURE.md`, `DESIGN.md`, `RULES.md`, `TASKS.md`, `DECISIONS.md`
- Lingkungan siap: Node.js, npm, Git, VS Code, GitHub Copilot (Student aktif), Freebuff
- `.gitignore` melindungi `.env*`, `node_modules/`, dan `.next/`
- **TASK-009 selesai:** Next.js 16.4.6 (App Router, Turbopack), React 19.3.0, Tailwind CSS 4.3.3, TypeScript 5.9.3 strict, ESLint 9. `src/app` sudah ada. Scaffold dibuat di folder sementara lalu disalin, jadi `docs/`, `RULES.md`, `TASKS.md`, `.env.example`, dan `.gitignore` tidak tertimpa (`.gitignore` hanya ditambah).
- **TASK-010 selesai:** Space Grotesk, Inter, JetBrains Mono dipasang lewat `next/font/google` di `src/app/layout.tsx` sebagai variabel `--font-space-grotesk`, `--font-inter`, `--font-jetbrains-mono`. Di `globals.css` dipetakan ke token Tailwind `font-heading`, `font-body`, `font-label`. Font ter-self-host (woff2) dan `body` memakai Inter.
- **TASK-011 selesai:** token warna dan bentuk di `src/app/globals.css` memakai `@theme static` (agar semua variabel selalu ter-emit) plus token di `:root`: `--color-bg/surface/border/text/muted/primary/primary-strong/primary-light/primary-soft`, `--radius-tag/button/card`, `--touch-target-min` (2.75rem), `--text-body-min` (1rem), `--focus-ring-width` (2px), `--focus-ring-offset` (2px).
- `npm run lint` dan `npm run build` lolos di root proyek
- **TASK-012 selesai:** skrip `lint` (`eslint`), `typecheck` (`tsc --noEmit`), dan `build` (`next build`) semuanya berjalan dan lolos. `typecheck` sudah diuji menangkap error tipe (exit 2) dengan file probe sementara yang sudah dihapus.
- **TASK-013 selesai:** struktur folder dibuat sesuai `ARCHITECTURE.md` — `src/{components,data,services,lib,types,utils}`, `src/features/{hero,skills,projects,certificates,library,contact,chatbot}`, `tests/`, dan `public/{sertifikat,belajar,cv}`. Folder kosong ditandai `.gitkeep` (17 file) dan dihapus saat folder terisi file sungguhan.

## Tugas Saat Ini

- **TASK-014:** commit dan push

## Hal yang Perlu Diperhatikan

- Pastikan semua dokumen perencanaan sudah ter-commit dan berada di lokasi yang benar (`RULES.md` dan `TASKS.md` di folder utama, sisanya di `docs/`). Cek dengan `git status`.
- **TASK-007 (`README.md`) ditunda** atas keputusan pemilik. README bawaan scaffold tidak disalin.
- Pembagian `src/lib` vs `src/utils` belum diputuskan (ADR belum ada). Sementara: `lib` untuk util umum, konfigurasi animasi, dan batas penggunaan seperti di `ARCHITECTURE.md`; `utils` untuk helper kecil murni. Tanyakan pemilik sebelum menaruh logika penting.
- `AGENTS.md` bawaan scaffold sengaja tidak disalin (belum diminta; aturan AI proyek ada di `RULES.md`).
- `npm test` belum ada karena framework tes unit belum dipilih; Playwright direncanakan di TASK-067. Jangan mengarang hasil tes.
- `npm audit` melaporkan **7 moderate** vulnerability setelah pemasangan. Tangani di TASK-070, jangan `npm audit fix` buta.
- `eslint@9.39.5` memberi peringatan sudah tidak didukung. Jangan naikkan mayor tanpa persetujuan (ADR-015).
- `layout.tsx` masih memakai `lang="en"` dan metadata "Create Next App"; ganti saat TASK-019. Halaman `page.tsx` masih placeholder.
- Token warna, bentuk, dan font sudah ada. Jangan menulis warna/ukuran langsung di komponen; pakai `bg-bg`, `text-muted`, `rounded-card`, `font-label`, dst.
- Halaman masih berlatar putih: latar `bg` belum dipasang ke `body` (menunggu TASK-019 layout).
- Telemetri Next.js aktif secara bawaan; putuskan sebelum production.
- `next-env.d.ts` diabaikan Git (bawaan Next.js), jadi file itu tidak ikut ter-commit.
- Visibilitas repository GitHub belum dikonfirmasi. Rencananya **Private** sampai audit rahasia lolos.
- Foto tanpa latar belum disiapkan (butuh EXIF dihapus).
- Jalur kontak belum diputuskan.
- Cara membatasi penggunaan chatbot belum diputuskan.
- Batas gratis Gemini harus dicek di Google AI Studio.
- Lisensi dan ketentuan React Bits belum diverifikasi di repository resminya.
- Komponen React Bits untuk latar hero belum dipilih (Dark Veil atau Faulty Terminal, tentukan setelah melihat demo).
- `TEST_PLAN.md` sudah dibuat. Skrip `lint`, `typecheck`, dan `build` sudah ada; `test` belum.

## Langkah Berikutnya

1. Selesaikan Phase 0 yang tersisa: `README.md` (TASK-007, ditunda) dan centang TASK-005/006/008 yang sebenarnya sudah ada.
2. Lanjut Phase 1: TASK-010 font, TASK-011 design token, TASK-012 skrip, TASK-013 struktur folder.
3. Kerjakan hero (Phase 3) sebagai alur lengkap pertama.

## Catatan Serah-Terima Antar Alat

- **Alat terakhir yang bekerja:** Freebuff (TASK-009)
- **Catatan:** scaffold Next.js sudah ada dan lolos lint + build; belum di-commit.
- **Ingat:** commit sebelum berpindah alat, dan perbarui bagian ini.
