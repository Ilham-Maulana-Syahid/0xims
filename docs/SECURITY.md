# Security Requirements

> Web milik orang security harus aman, dan keamanan tidak ditambahkan lima menit sebelum deploy.
> Dokumen ini berprioritas **tertinggi** di antara dokumen proyek. Jika bertentangan dengan dokumen lain, dokumen ini yang menang.

## 1. Prinsip

1. **Minim rahasia, dan rahasia hanya di server.**
2. **Anggap semua input sebagai tidak tepercaya**, termasuk pesan chatbot dan isi paket pihak ketiga.
3. **Kurangi permukaan serangan.** Tidak ada unggahan, tidak ada admin web, tidak ada database di MVP.
4. **Aman secara bawaan.** Jika ragu, pilih opsi yang lebih ketat.
5. **Dapat diperiksa.** Kode yang disalin (React Bits) dan paket yang dipasang harus ditinjau.

## 2. Aset dan Ancaman

| Aset | Ancaman utama | Pengendalian |
|---|---|---|
| `GEMINI_API_KEY` | Bocor lewat kode, Git, log, atau prompt AI | Hanya di server, `.gitignore`, pindai rahasia, rotasi jika bocor |
| Kuota chatbot | Dihabiskan penyalahguna | Batas per pengunjung dan batas global, pesan sopan saat habis |
| Instruksi sistem chatbot | Prompt injection membocorkannya | Instruksi ketat, pesan pengunjung diperlakukan sebagai data, uji penyalahgunaan |
| Data pribadi pemilik | Bocor lewat EXIF, metadata PDF, sertifikat, riwayat Git | Bersihkan aset, email noreply, audit sebelum Public |
| Pengunjung | XSS dari keluaran chatbot atau data | Tampilkan sebagai teks, tanpa HTML mentah |
| Rantai pasokan | Paket jahat atau karangan AI | Verifikasi paket, kunci versi, audit |
| Akun GitHub dan Vercel | Pengambilalihan akun | Autentikasi dua langkah, token berhak akses minimal |
| Kontak pemilik | Spam dan pengikisan alamat | Jalur kontak yang tidak terbaca mudah oleh pengikis |

## 3. Rahasia dan Environment

- Semua rahasia hanya lewat environment variable. Lokal: `.env.local`. Production dan preview: pengaturan Vercel.
- `.env*` ada di `.gitignore` sejak commit pertama.
- `.env.example` hanya berisi **nama** variabel tanpa isi.
- `GEMINI_API_KEY` **tidak boleh** berawalan `NEXT_PUBLIC_` (variabel berawalan itu ikut terkirim ke browser).
- Tidak ada rahasia di kode, komentar, log, pesan error, contoh, tangkapan layar, atau prompt ke AI.
- Respons server tidak boleh memuat key, instruksi sistem, atau detail internal.
- Preview dan production boleh memakai nilai yang berbeda. Jangan memakai key production di lingkungan uji.

## 4. Keamanan Chatbot

**Batas input dan output:**
- Panjang pesan maksimum, jumlah pesan riwayat maksimum, dan panjang jawaban maksimum.
- Validasi tipe data di server. Tolak permintaan yang bentuknya tidak sesuai.

**Batas penggunaan:**
- Batas per pengunjung dan batas global harian karena kuota gratis dipakai bersama.
- Saat batas tercapai atau kuota habis: pesan sopan, bukan error mentah.
- Mekanisme penyimpanan hitungan batas adalah keputusan terbuka (lihat `DECISIONS.md`). Sebelum diputuskan, jangan menganggap batas dalam memori server sebagai jaminan.

**Perlindungan dari prompt injection:**
- Instruksi sistem membatasi ruang lingkup: hanya isi portfolio.
- Pesan pengunjung dan data konteks diperlakukan sebagai **data**, bukan perintah.
- Bot tidak boleh membocorkan instruksi internal, key, atau variabel environment.
- Bot menolak topik di luar portfolio dengan sopan.
- Bot tidak boleh mengarang data pemilik. Jika tidak ada di data, jawab bahwa tidak tahu.
- Tidak ada alat atau kemampuan tambahan untuk bot (tanpa akses internet, file, atau perintah).
- Konteks yang dikirim ke Gemini hanya data portfolio yang memang publik.

**Privasi:**
- Panel menampilkan catatan bahwa permintaan di paket gratis dapat dipakai penyedia untuk meningkatkan produknya, dan pengunjung tidak boleh mengetik data pribadi.
- Pesan pengunjung tidak dicatat penuh. Log hanya berisi data teknis minimum, tanpa isi pesan atau rahasia.

## 5. Penanganan Input dan Output

- Validasi semua input di server (panjang, tipe, bentuk).
- Keluaran chatbot ditampilkan sebagai **teks biasa**. Jika memakai format (misalnya Markdown), gunakan penyaring yang aman dan tanpa HTML mentah.
- Dilarang memakai `dangerouslySetInnerHTML`, `eval`, atau eksekusi kode dinamis tanpa persetujuan pemilik dan alasan tertulis.
- Tautan keluar memakai `rel="noopener noreferrer"`.
- Jangan membangun alamat URL dari input pengguna tanpa validasi.

## 6. Header Keamanan dan CSP

Dipasang sebelum production:

- `Content-Security-Policy` (mulai dari mode laporan saja, lalu diketatkan bertahap)
- `X-Content-Type-Options: nosniff`
- `Referrer-Policy: strict-origin-when-cross-origin`
- `Permissions-Policy` yang membatasi fitur perangkat yang tidak dipakai
- Perlindungan dari penyematan di situs lain (`frame-ancestors`)
- HTTPS di semua alamat

CSP harus diuji agar tidak merusak font, animasi, WebGL, dan gambar. Sebelum diketatkan, uji dengan mode laporan saja.

## 7. Dependensi dan Rantai Pasokan

- Paket baru hanya dipasang dengan persetujuan pemilik, setelah diverifikasi di registry resmi: nama persis, penerbit, jumlah pemakai, tanggal rilis, tautan repository.
- Waspadai paket dengan nama mirip (typosquatting) dan paket karangan AI.
- Versi dikunci lewat lockfile. Pasang dari lockfile (`npm ci`) di lingkungan build.
- Jalankan audit dependensi sebelum deploy dan secara berkala.
- Tinjau skrip pemasangan (`postinstall` dan sejenisnya) dari paket baru.
- Paket yang baru dirilis diberi jeda sebelum dipakai.
- **Kode React Bits** yang disalin ditinjau sebelum dipakai: tidak ada panggilan jaringan, pengiriman data, atau skrip yang mencurigakan.

## 8. Aset dan Data Pribadi

- **Foto:** hapus EXIF (lokasi, perangkat, waktu) sebelum masuk ke proyek, lalu verifikasi hasilnya.
- **PDF dan dokumen:** bersihkan metadata (penulis, perangkat lunak, jalur file) dan periksa isinya.
- **Sertifikat:** periksa dan samarkan NIM, nomor sertifikat, kode QR, atau data lain yang tidak perlu.
- **File belajar:** hanya yang boleh dibagikan, bebas data pribadi.
- **Kontak:** jangan menulis alamat email atau nomor telepon polos di HTML. Pilih jalur yang tidak mudah dikikis, dan sadari bahwa penyamaran tidak menjamin keamanan penuh.
- **Riwayat Git:** commit memakai email noreply GitHub. Jangan meng-commit data pribadi lalu "menghapusnya" kemudian, karena tetap ada di riwayat.

## 9. Akun dan Akses

- Aktifkan autentikasi dua langkah di GitHub dan Vercel.
- Token akses hanya berhak minimal dan punya masa berlaku.
- Jangan membagikan token di chat, prompt, atau tangkapan layar.
- Batasi API key Gemini sesuai opsi yang tersedia di layanannya, dan rotasi jika ada kecurigaan bocor.
- Login GitHub saat `git push` dilakukan lewat jendela peramban, bukan dengan menempel kata sandi atau token ke terminal.

## 10. Git dan Repository

- Repository dibuat **Private** dan dijadikan Public hanya setelah daftar di bagian 13 lolos.
- Pindai rahasia di berkas **dan riwayat Git** dengan alat pemindai rahasia sebelum Public.
- Jangan meng-commit `.env*`, `node_modules/`, `.next/`, atau file besar yang tidak perlu.
- Jika rahasia pernah ter-commit: anggap **sudah bocor**. Cabut atau rotasi dulu, baru bersihkan riwayat.

## 11. Memakai AI untuk Menulis Kode

- Jangan memasukkan rahasia, isi `.env*`, atau data pribadi ke prompt.
- AI tidak boleh membuka atau membaca file `.env*`.
- Freebuff didanai iklan dan memakai model luar. Perlakukan apa pun yang masuk ke prompt sebagai bisa keluar dari komputermu.
- Tinjau setiap perubahan (diff) sebelum commit. Cari: key tertulis di kode, `eval`, HTML mentah, paket baru, panggilan jaringan baru.
- Perintah merusak (`git push --force`, `git reset --hard`, penghapusan folder) wajib persetujuan.
- Kode yang dihasilkan AI belum tentu aman hanya karena berhasil dijalankan.

## 12. Jika Terjadi Insiden

**Jika API key diduga bocor:**
1. **Cabut atau rotasi key** di penyedia layanan, segera.
2. Periksa penggunaan yang tidak wajar.
3. Perbarui key di Vercel dan `.env.local`, lalu deploy ulang.
4. Hapus dari kode dan riwayat Git jika perlu.
5. Catat kejadian dan penyebabnya di `DECISIONS.md` atau `MEMORY.md`.

**Jika chatbot disalahgunakan:** matikan sementara lewat konfigurasi, perketat batas, perbaiki, lalu aktifkan lagi. Web tetap berfungsi tanpa chatbot.

## 13. Daftar Periksa Sebelum Public dan Production

- [ ] Tidak ada rahasia di berkas maupun riwayat Git
- [ ] `.env*` tidak pernah ter-commit
- [ ] `GEMINI_API_KEY` tidak muncul di kode browser, lalu lintas jaringan browser, atau hasil build
- [ ] Tidak ada variabel rahasia berawalan `NEXT_PUBLIC_`
- [ ] Header keamanan dan CSP terpasang dan sudah diuji
- [ ] Audit dependensi tidak menyisakan masalah serius
- [ ] Semua paket di `package.json` dikenal dan disetujui
- [ ] Kode React Bits sudah ditinjau
- [ ] Foto, PDF, sertifikat, dan file belajar bersih dari metadata dan data pribadi
- [ ] Uji penyalahgunaan chatbot lolos (lihat `TEST_PLAN.md` bagian 10)
- [ ] Batas penggunaan chatbot aktif dan teruji
- [ ] Autentikasi dua langkah aktif di GitHub dan Vercel
- [ ] Jalur kontak tidak membuka alamat pribadi secara polos

## 14. Melaporkan Kerentanan

Jika kamu menemukan celah keamanan pada web ini, mohon laporkan secara pribadi lewat jalur kontak yang tertera di web (jalur kontak final ditentukan sebelum rilis), dan jangan mengumumkannya secara terbuka sebelum diperbaiki.
