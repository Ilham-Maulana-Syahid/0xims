# Design System

> DESIGN.md = BAGAIMANA web ini terlihat dan terasa.
> Aturan teknis ada di `ARCHITECTURE.md`. Apa yang dibangun ada di `PRD.md`.
> Semua halaman wajib mengikuti dokumen ini agar tampilannya konsisten.

## 1. Konsep dan Suasana

- **Konsep:** "Serial" bertema cyber security. Pemilik adalah bintang serialnya.
- **Suasana:** gelap, sinematik, dengan cahaya violet neon. Mewah tapi tetap cepat dan terbaca.
- **Istilah antarmuka:** proyek disebut "Episode", sertifikat disebut "Arsip", bagian file belajar disebut "Belajar Bareng".
- **Dihindari:** nama, logo, dan warna merah khas merek streaming mana pun. Tidak ada label seperti "Match" atau "HD" yang meniru merek.

## 2. Warna

Semua warna didefinisikan sebagai design token (variabel CSS dan konfigurasi Tailwind). Jangan menulis kode warna langsung di komponen.

| Token | Nilai | Dipakai untuk | Kontras di latar utama |
|---|---|---|---|
| `bg` | `#0A0A0F` | Latar halaman | |
| `surface` | `#14141C` | Kartu dan panel | |
| `border` | putih 8% | Garis tipis kartu | |
| `text` | `#F4F4F8` | Judul dan isi | 18,0 |
| `muted` | `#A1A1B5` | Keterangan dan label | 7,8 |
| `primary` | `#8B5CF6` | Cahaya, garis, ikon, elemen dekoratif besar | 4,66 |
| `primary-strong` | `#7C3AED` | Latar tombol utama (teks putih, kontras 5,7) | |
| `primary-light` | `#A78BFA` | Teks aksen kecil dan tautan | 7,26 |
| `primary-soft` | `#C4B5FD` | Sorotan | 10,7 |

**Aturan:**
- Teks putih **tidak boleh** di atas `primary` (kontras hanya 4,23). Pakai `primary-strong` untuk tombol.
- Teks kecil berwarna aksen memakai `primary-light`, bukan `primary`.
- Kontras diukur dengan rumus WCAG. Ukur ulang jika ada warna baru.

## 3. Tipografi

| Peran | Font | Pemakaian |
|---|---|---|
| Judul | Space Grotesk | Nama, judul bagian, angka besar |
| Isi | Inter | Paragraf, tombol, menu |
| Label | JetBrains Mono | Tag teknologi, label kategori, penomoran episode, teks ala terminal |

- Font dipasang lewat fitur font bawaan Next.js, dengan cadangan font sistem.
- Label (JetBrains Mono) memakai huruf kapital kecil dengan jarak antar huruf sedikit lebih lebar.
- Isi memakai ukuran minimal 16px di layar kecil.

## 4. Hero (Halaman Depan)

**Tata letak desktop:** teks di kiri, foto di kanan. **Mobile:** teks lebih dulu, foto di bawahnya, atau foto di atas dengan ukuran terbatas supaya nama dan tombol tetap terlihat tanpa scroll.

**Isi hero:**
- **Nama lengkap: "Ilham Maulana Syahid", SELALU SATU BARIS.** Nama tidak boleh dipecah menjadi dua baris di lebar layar mana pun.
  - Ukuran huruf fluid (mengecil otomatis mengikuti lebar layar) dan tidak boleh membungkus baris (`white-space: nowrap`).
  - Target awal: sekitar 24 sampai 26px di layar 375px, dan jauh lebih besar di desktop (sekitar 48 sampai 64px). Angka akhir ditentukan dengan menguji langsung di 375px, 768px, dan 1440px.
  - Jika masih terlalu lebar, kecilkan ukuran huruf atau rapatkan sedikit jarak antar huruf. Jangan memecah baris.
  - Efek Split Text tetap boleh dipakai, asalkan hasil akhirnya tetap satu baris.
- **Peran:** "Cyber Security Enthusiast", memakai efek teks dekripsi.
- **Ringkasan** satu sampai dua kalimat, jujur bahwa masih aktif belajar.
- **Dua tombol:** "Hubungi" (utama) dan "Unduh CV" (sekunder).
- **Foto:** potongan tanpa latar (cutout) dengan lingkaran cahaya violet di belakangnya.
- **Kartu kaca melayang (2 sampai 3):** misalnya peran, status belajar, dan teknologi utama. Isinya berasal dari `src/data`.

**Gerak:**
- Foto bergeser sedikit mengikuti kursor (efek kedalaman). Dimatikan di layar sentuh dan saat pengguna memilih mengurangi gerakan.
- Kartu melayang bergerak pelan.
- **Efek scanning pada foto:** garis pindai tipis berwarna `primary-soft` dengan cahaya violet menyapu foto dari atas ke bawah.
  - Garis hanya terlihat **di atas siluet foto** (dibatasi bentuk cutout), tidak melebar ke latar.
  - Berjalan satu kali penuh saat pembukaan hero, lalu **berulang pelan** (sekitar sekali tiap 6 sampai 8 detik).
  - Wajah tetap terlihat jelas. Garis tipis dan transparan, tidak menutupi mata atau mulut.
  - Tidak boleh berkedip cepat (tidak lebih dari tiga kali kilatan per detik).
  - Dibuat dengan satu cara saja (CSS atau GSAP, bukan keduanya) dan hanya menganimasikan transform dan opacity.
  - Opsional: label kecil berfont mono di dekat foto, misalnya "SCAN: COMPLETE" setelah satu putaran selesai.
  - Dimatikan atau diganti tampilan statis saat pengguna memilih mengurangi gerakan, dan dihentikan saat foto tidak terlihat di layar.

**Aturan:**
- Nama (satu baris), peran, dan tombol harus terlihat dan bisa diklik **sebelum** animasi selesai.
- Kartu melayang **tidak boleh menutupi** tombol atau teks penting.
- Foto: format PNG atau WebP tanpa latar, ukuran file kecil, **data EXIF dihapus** sebelum dimasukkan ke proyek.

## 5. Animasi

**Tempo: campuran berlapis.**

| Konteks | Durasi | Gaya |
|---|---|---|
| Pembukaan hero | Sekitar 0,8 sampai 1,2 detik | Sinematik, berurutan, **hanya sekali per kunjungan** |
| Kartu, tombol, menu, hover | Sekitar 0,2 sampai 0,5 detik | Gesit dan tajam |
| Muncul saat scroll | Sekitar 0,4 sampai 0,6 detik | Halus, sekali saja per elemen |

**Aturan:**
1. Pembukaan hero **bisa dilewati** (klik atau tombol "Lewati") dan tidak diulang pada kunjungan yang sama.
2. Hormati pengaturan sistem untuk mengurangi gerakan: ganti dengan tampilan statis atau perubahan opacity sederhana.
3. Satu elemen hanya dianimasikan oleh satu library.
4. Animasi hanya memakai properti ringan (transform dan opacity) bila memungkinkan.
5. Maksimal **satu** latar WebGL atau canvas per halaman. Dimuat belakangan, dan diganti tampilan statis jika perangkat lemah.
6. Animasi tidak boleh menghalangi membaca atau menekan tombol.

**Pembagian library:** Motion untuk komponen, GSAP untuk urutan dan scroll, three.js hanya jika dibutuhkan (lihat `ARCHITECTURE.md`).

## 6. Komponen dan Bentuk

**Bentuk dasar:**
- Kartu: sudut membulat 16px, latar `surface`, garis `border`, bayangan lembut. Saat disorot, muncul cahaya violet tipis.
- Tombol: sudut 12px, tinggi minimal 44px untuk sentuhan.
  - **Utama:** latar `primary-strong`, teks putih.
  - **Sekunder:** garis `primary-light`, latar transparan.
- Tag teknologi: sudut 8px, font label, garis violet tipis.
- Fokus keyboard: cincin 2px `primary-light` dengan jarak, selalu terlihat.

**Komponen React Bits yang dipilih** (disalin sebagai kode sumber, ditinjau dulu, lihat `ARCHITECTURE.md`):

| Bagian | Komponen | Status |
|---|---|---|
| Latar hero (pilih satu) | Dark Veil atau Faulty Terminal | Diputuskan setelah melihat demo |
| Peran di hero | Decrypted Text | Dipakai |
| Nama di hero | Split Text | Dipakai |
| Kartu skill | Tilted Card | Dipakai |
| Kartu proyek ("Episode") | Spotlight Card | Dipakai |
| Rak sertifikat ("Arsip") | Masonry | Dipakai |
| Kursor | Target Cursor | Opsional, hanya desktop |
| Angka ringkas | Count Up | Opsional |

Komponen disesuaikan dengan token warna dan font di dokumen ini agar tidak terlihat seperti template.

## 7. Susunan Halaman dan Menu

**Menu atas:** Home, About, Skills, Projects, Certificates, Belajar Bareng, Contact, dan tombol Unduh CV.

**Urutan bagian:**
1. Hero
2. Profil (About)
3. Skill: kartu miring yang bisa digeser, dengan tag teknologi
4. Pencapaian dan perjalanan belajar
5. Proyek: kartu "Episode" bernomor (misalnya S01 E01)
6. Sertifikat: rak "Arsip" per kategori, sertifikat paling relevan di depan
7. Belajar Bareng: kartu file per kategori dengan tombol unduh
8. Kontak
9. Chatbot: tombol mengambang di sudut layar

## 8. Chatbot (Tampilan)

- Tombol mengambang di sudut kanan bawah, tidak menutupi tombol penting di layar kecil.
- Panel obrolan bergaya kaca gelap dengan aksen violet.
- Catatan privasi singkat di bagian atas panel (jangan mengetik data pribadi).
- **Keadaan yang wajib ada:** memuat jawaban, kosong (saran pertanyaan awal), kuota habis (pesan sopan), dan error.

## 9. Persyaratan UX

- Responsif di lebar 375px, 768px, dan 1440px.
- Ada keadaan memuat, kosong, dan error untuk setiap bagian yang mengambil atau menampilkan data.
- Formulir dan kontrol bisa diakses: label jelas, bisa dioperasikan dengan keyboard, urutan fokus logis.
- Semua gambar punya teks alternatif. Gambar sertifikat dimuat kecil dulu, versi besar saat diklik.
- Kontras teks memenuhi WCAG AA.
- Informasi utama terlihat tanpa scroll di hero.
- Nama lengkap tampil satu baris di semua lebar layar, tanpa terpotong atau meluber keluar layar.

## 10. Hal yang Harus Dihindari

- Warna merah dan nama atau logo merek streaming.
- Lebih dari satu latar WebGL per halaman.
- Elemen melayang yang menutupi tombol atau teks.
- Animasi yang tidak bisa dilewati atau dimatikan.
- Menulis kode warna atau ukuran langsung di komponen, di luar token.
