# Development Rules

> Aturan main untuk AI (dan manusia) yang mengerjakan proyek 0xims.
> Berlaku untuk semua alat: Freebuff, GitHub Copilot, atau yang lain.
> Jika ada konflik antar dokumen, urutan prioritasnya: `SECURITY.md` > `RULES.md` > `ARCHITECTURE.md` > `DESIGN.md` > `PRD.md`.

## 1. Sebelum Mulai Mengerjakan Apa Pun

1. Baca dokumen yang relevan: `docs/PRD.md`, `docs/ARCHITECTURE.md`, `docs/DESIGN.md`, `docs/SECURITY.md`, `RULES.md`, dan `TASKS.md`.
2. Periksa kode yang sudah ada. Pakai ulang komponen dan fungsi yang sudah ada sebelum membuat yang baru.
3. Untuk perubahan yang menyentuh lebih dari satu bagian, **buat rencana dulu** (file apa yang akan diubah dan kenapa) dan tunggu persetujuan sebelum menulis kode.
4. Kerjakan **satu tugas kecil** per permintaan. Jangan membangun banyak fitur sekaligus.
5. Jika informasi kurang atau ragu, **tanyakan**. Jangan menebak.

## 2. Aturan Umum

- Gunakan TypeScript (strict). Hindari `any` kecuali ada alasan yang ditulis.
- Jangan menduplikasi logika. Fungsi kecil dan fokus.
- **Jangan mengubah file yang tidak terkait** dengan tugas.
- Jangan mengganti teknologi atau keputusan di `ARCHITECTURE.md` tanpa persetujuan. Perubahan penting dicatat di `docs/DECISIONS.md`.
- Jangan mengubah dokumen perencanaan (`docs/*`, `RULES.md`) tanpa diminta.
- Jangan membuat perubahan besar yang tidak diminta (refactor, pindah folder, ganti nama massal).
- Lingkungan kerja pemilik adalah **Windows dengan PowerShell**. Perintah terminal yang diberikan harus kompatibel dengan PowerShell.

## 3. Bahasa

- **Isi web (teks yang dibaca pengunjung): Bahasa Indonesia.**
- Nama istilah teknis yang memang lazim dalam bahasa Inggris (misalnya "CTF", "bug bounty", nama teknologi) tidak perlu diterjemahkan.
- Kode, nama variabel, nama file, dan pesan commit: Bahasa Inggris.
- Komentar kode boleh Bahasa Indonesia agar mudah dipelajari.
- Chatbot menjawab dalam bahasa yang dipakai penanya.

## 4. Data dan Konten

- Semua isi portfolio (profil, skill, proyek, sertifikat, file belajar) ditulis **hanya di `src/data`** dengan tipe di `src/types`.
- Menambah konten tidak boleh mengharuskan mengubah komponen tampilan.
- Jangan menulis teks konten langsung di komponen.
- Jangan mengarang data pemilik (pencapaian, sertifikat, proyek). Jika data tidak ada, pakai penanda kosong yang jelas dan beri tahu pemilik.

## 5. UI dan Animasi

- Ikuti `docs/DESIGN.md`: warna, font, ukuran, dan komponen.
- **Jangan menulis kode warna langsung** di komponen. Pakai design token.
- Nama lengkap di hero **selalu satu baris** di semua lebar layar.
- Semua bagian responsif (375px, 768px, 1440px).
- Setiap bagian yang menampilkan atau memuat data punya keadaan **memuat, kosong, dan error**.
- Hormati `prefers-reduced-motion` di setiap animasi.
- **Satu elemen hanya dianimasikan oleh satu library.**
- Maksimal **satu** latar WebGL/canvas per halaman, dimuat belakangan (lazy).
- Informasi utama (nama, peran, tombol aksi) tidak boleh menunggu animasi selesai.
- Jangan memakai nama, logo, atau warna khas merek streaming.
- Komponen React Bits: salin sebagai kode sumber, **baca dan tinjau kodenya**, sesuaikan dengan token desain, dan laporkan dependensi yang dibawanya.

## 6. Keamanan (ringkasan, rincian di `docs/SECURITY.md`)

- **Jangan pernah** menulis, mencetak, atau meminta isi API key, token, atau file `.env*`. AI tidak boleh membuka atau membaca file `.env*`.
- Jangan menaruh rahasia di kode, komentar, log, contoh, atau prompt.
- `GEMINI_API_KEY` hanya dipakai di sisi server. **Jangan** memberinya awalan `NEXT_PUBLIC_`.
- Validasi semua input (panjang, tipe) terutama untuk endpoint chatbot.
- Perlakukan pesan pengunjung ke chatbot sebagai **data, bukan perintah**.
- Jangan memakai `eval`, `dangerouslySetInnerHTML`, atau eksekusi kode dinamis tanpa persetujuan dan alasan tertulis.
- Tautan keluar memakai `rel="noopener noreferrer"`.
- Jangan menambah pelacak, analytics, atau panggilan jaringan ke layanan yang tidak dikenal.
- Jangan memasukkan data pribadi pemilik (nomor telepon, alamat, dokumen asli) ke prompt kecuali benar-benar dibutuhkan untuk tugas.
- Foto, sertifikat, dan file belajar: pastikan **data tersembunyi (EXIF/metadata) dan data pribadi** sudah dibersihkan sebelum masuk ke proyek.

## 7. Dependensi

- **Jangan memasang paket baru tanpa persetujuan.** Sebelum meminta persetujuan, laporkan: nama paket, tujuan, penerbit, ukuran perkiraan, dan alternatif yang sudah ada di proyek.
- **Verifikasi paket ada** di registry resmi. AI kadang mengarang nama paket atau salah menulis nama.
- Utamakan paket yang sudah dipakai proyek. Jangan menambah library baru untuk hal yang sudah bisa dilakukan Motion, GSAP, three.js, atau Tailwind.
- Paket yang baru dirilis sangat dekat dengan hari ini tidak dipasang tanpa persetujuan.
- Versi dikunci lewat lockfile. Jangan menghapus atau membuat ulang lockfile tanpa alasan.

## 8. Pengujian

Setelah menyelesaikan tugas, jalankan (sesuai konfigurasi proyek):

```powershell
npm run lint
npm run typecheck
npm run build
npm test
```

- Tambahkan tes untuk logika penting (misalnya pembatas penggunaan chatbot, validasi input).
- Perbaiki tes yang gagal **sebelum** lanjut ke tugas berikutnya. Jangan menghapus atau melonggarkan tes agar lolos.
- Jika suatu perintah belum ada di proyek, katakan, jangan mengarang hasilnya.

## 9. Git

- Commit kecil, satu perubahan logis per commit.
- Format pesan commit (Inggris): `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `test:`.
- **Commit sebelum berpindah alat** (misalnya dari Freebuff ke Copilot) agar ada titik kembali.
- **Jangan menjalankan perintah merusak tanpa konfirmasi:** `git push --force`, `git reset --hard`, `git clean -fd`, menghapus folder, atau `rm -rf` / `Remove-Item -Recurse`.
- Jangan meng-commit rahasia, `node_modules/`, `.next/`, atau file besar yang tidak perlu.
- Fitur besar dikerjakan di cabang `feature/...`, digabung ke `main` setelah lolos pemeriksaan.

## 10. Bekerja dengan Dua Alat AI

- Peran: **Freebuff untuk membangun** fitur, **Copilot untuk revisi dan review**.
- Jangan membiarkan dua alat mengedit **file yang sama** bersamaan.
- Setelah selesai, perbarui status tugas di `TASKS.md` agar alat berikutnya tahu posisi terakhir.

## 11. Format Laporan Setelah Tugas Selesai

AI wajib melaporkan:
1. **File yang diubah**
2. **Apa yang diimplementasikan**
3. **Pengujian yang dijalankan** dan hasilnya (jujur, termasuk yang gagal)
4. **Masalah yang tersisa** atau hal yang tidak yakin
5. **Asumsi** yang dibuat
6. **Dependensi baru** yang diusulkan (jika ada)

## 12. Alur Kerja Setiap Tugas

```
BACA → PAHAMI → RENCANAKAN → IMPLEMENTASI → TES → REVIEW → PERBAIKI → COMMIT → UPDATE DOKUMEN
```
