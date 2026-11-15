# Product Requirements Document (PRD)

> PRD = APA yang dibangun dan KENAPA.
> Cara membangunnya ada di `ARCHITECTURE.md`. Tampilannya ada di `DESIGN.md`.

## 1. Produk

**Nama proyek:** 0xims

**Deskripsi singkat:** Web portfolio pribadi seorang mahasiswa informatika yang sedang belajar cyber security. Web ini menjadi satu "rumah" online untuk profil, skill, pencapaian, proyek, sertifikat, dan file belajar, dibuat sekali dan dipakai jangka panjang sampai lulus.

## 2. Masalah

Pencapaian, profil, skill, proyek, dan sertifikat pemilik masih tersebar di banyak tempat dan belum punya satu wadah online yang rapi dan meyakinkan.

## 3. Pengguna

**Pengguna utama:** Rekruter dan HRD yang menilai kandidat magang atau kerja.

**Pengguna tambahan:** Dosen, pemberi beasiswa atau lomba, sesama developer dan komunitas, serta masyarakat umum.

**Implikasi:** Rekruter hanya punya beberapa detik. Halaman depan harus langsung menunjukkan siapa pemilik, peran, skill utama, dan tombol aksi tanpa perlu banyak scroll.

## 4. Tujuan

Rekruter terkesan oleh web-nya, yakin pada kemampuan pemilik, lalu tertarik menghubungi.

**Aksi yang diharapkan (urut prioritas):**
1. Menghubungi pemilik
2. Mengunduh CV
3. Melihat proyek di GitHub
4. Melihat sertifikat sebagai bukti kemampuan

**Arti "luar biasa bagus":** tampilan rapi dan konsisten, cepat dibuka, nyaman di HP maupun laptop, informasi penting langsung terlihat.

## 5. Identitas dan Konsep

- **Identitas:** Cyber Security Enthusiast, yang jujur bahwa masih aktif belajar.
- **Konsep:** "Serial" bertema cyber security. Pemilik adalah bintang serialnya, pengunjung adalah penontonnya.
- **Terinspirasi dari** portfolio bergaya streaming: hero dengan foto dan kartu kaca melayang, kartu skill miring yang bisa digeser, proyek sebagai "episode", sertifikat sebagai rak arsip.
- **Bukan meniru mentah-mentah:** tidak memakai nama, logo, atau warna khas merek lain. Aksen visual bernuansa cyber (gelap, neon, label ala terminal). Detail ada di `DESIGN.md`.

## 6. Prinsip Produk

1. **Tahan lama.** Semua isi (profil, proyek, sertifikat, file belajar) disimpan sebagai data terstruktur, bukan ditulis mati di halaman, agar mudah ditambah tanpa membongkar kode tampilan.
2. **Animasi maksimal tapi terkontrol.** Banyak animasi di titik yang penting (hero, scroll, kartu, tombol, transisi), tetapi tetap cepat dan informasi penting selalu terbaca. Tersedia dukungan untuk pengunjung yang mengurangi animasi di perangkatnya.
3. **Jujur.** Pemilik masih pemula. Tampilkan proyek yang sudah jadi, perjalanan belajar, dan arah ke depan, tanpa klaim berlebihan.
4. **Aman sejak awal.** Web milik orang security harus aman: tidak ada rahasia di kode, form dan chatbot tidak mudah disalahgunakan.

## 7. Fitur MVP

Semua bagian berikut wajib ada di versi pertama.

| No | Bagian | Isi dan aturan |
|---|---|---|
| 1 | **Hero** | Nama, peran (Cyber Security Enthusiast), ringkasan singkat, foto beranimasi, kartu melayang, tombol aksi (Hubungi, Unduh CV) |
| 2 | **Profil** | Siapa pemilik secara singkat |
| 3 | **Skill** | Kartu skill per kategori dengan tag teknologi |
| 4 | **Pencapaian** | Daftar pencapaian, termasuk perjalanan belajar |
| 5 | **Proyek** | Kartu bergaya "episode", tiap proyek punya deskripsi singkat, teknologi, dan tautan GitHub |
| 6 | **Sertifikat** | Rak "arsip", dikelompokkan per kategori, sertifikat paling relevan di depan, gambar kecil dulu lalu besar saat diklik, tautan verifikasi jika tersedia |
| 7 | **Belajar Bareng** | Daftar file belajar per kategori (judul, deskripsi, tombol unduh). Hanya pemilik yang menambah file, pengunjung tidak bisa mengunggah |
| 8 | **Kontak dan CV** | Jalur kontak yang tidak mengundang spam, dan tombol unduh CV |
| 9 | **Chatbot portfolio** | Lihat bagian 8 |

### Aturan konten

- **Sertifikat:** periksa dan samarkan data pribadi (NIM, nomor sertifikat, kode QR sensitif) sebelum dipajang.
- **Belajar Bareng:** hanya file buatan sendiri atau yang memang boleh dibagikan. Jangan membagikan slide dosen, buku, atau soal ujian milik pihak lain tanpa izin. Pastikan isi dan metadata file tidak memuat data pribadi.
- **Berkas besar:** dimuat bertahap agar web tetap ringan.

## 8. Chatbot Portfolio

**Tujuan:** Resepsionis pribadi yang menjawab pertanyaan pengunjung seputar isi portfolio.

**Ruang lingkup:**
- Hanya menjawab hal yang ada di data portfolio (profil, skill, proyek, sertifikat, file belajar).
- Menolak sopan pertanyaan di luar topik.
- Menggunakan Gemini API (paket gratis).

**Aturan wajib:**
- API key hanya disimpan di server, tidak pernah ada di kode browser atau di Git.
- Batas jumlah pertanyaan per pengunjung, karena kuota gratis dipakai bersama semua pengunjung.
- Pesan yang sopan saat kuota habis, tanpa membuat web terlihat rusak.
- Catatan privasi singkat di jendela chat: jangan mengetik data pribadi.
- Terhadap upaya prompt injection: bot tidak boleh membocorkan instruksi internal atau keluar dari ruang lingkup.
- Web harus tetap berfungsi penuh jika chatbot tidak tersedia.

## 9. Tidak Masuk Versi Pertama (Out of Scope)

- Halaman admin untuk mengedit konten lewat web
- Dua bahasa
- Komentar atau buku tamu
- Pembayaran atau jual jasa
- Aplikasi mobile
- Blog atau artikel
- Halaman khusus lab dan tools keamanan (belum ada isinya)

## 10. Kriteria Sukses

Pengunjung dapat:
1. Melihat nama, peran, dan tombol aksi di halaman depan tanpa scroll
2. Menghubungi pemilik
3. Mengunduh CV
4. Membuka proyek dan tautan GitHub-nya
5. Melihat dan memperbesar sertifikat
6. Mengunduh file di Belajar Bareng
7. Bertanya ke chatbot dan mendapat jawaban yang sesuai isi portfolio

Kualitas yang harus terpenuhi:
- Nyaman dipakai di lebar layar 375px, 768px, dan 1440px
- Cepat dibuka, animasi tidak membuat web berat
- Tidak ada rahasia (API key, file `.env`) di repository
- Chatbot punya batas penggunaan dan penanganan kuota habis

## 11. Asumsi dan Hal yang Masih Terbuka

- Jalur kontak yang dipakai (email, LinkedIn, atau lainnya) belum ditentukan.
- Pemilik menyiapkan konten sendiri: foto, CV, daftar sertifikat beserta gambarnya, file belajar.
- Batas gratis Gemini harus dicek langsung di Google AI Studio.
- Hosting direncanakan di Vercel, akan dipastikan di `ARCHITECTURE.md`.
- Domain pribadi bersifat opsional.
