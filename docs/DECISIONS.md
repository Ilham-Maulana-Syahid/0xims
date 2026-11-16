# Architecture Decisions

> Keputusan PERMANEN yang sudah dikunci oleh pemilik proyek.
> Status proyek saat ini ada di `MEMORY.md` (dibuat belakangan), bukan di sini.

## Aturan Penggunaan

- AI **tidak boleh mengubah** keputusan di dokumen ini tanpa persetujuan pemilik.
- Jika ingin mengubah, tambahkan ADR baru yang menyebut ADR lama yang digantikan, lalu tandai ADR lama sebagai "Digantikan".
- Setiap keputusan teknis penting yang baru wajib dicatat di sini.

Bulan penetapan keputusan awal: November 2026.

---

## ADR-001: Kerangka web

**Keputusan:** Next.js (App Router) + TypeScript + Tailwind CSS.

**Alasan:** Satu proyek untuk tampilan dan endpoint chatbot, tanpa backend terpisah. Tailwind membuat gaya konsisten lewat token. TypeScript membantu menangkap kesalahan yang sering dibuat AI.

**Status:** Aktif

## ADR-002: Tanpa database di MVP

**Keputusan:** Semua isi portfolio disimpan sebagai data bertipe di `src/data` (konten panjang boleh MDX).

**Alasan:** Isi web cukup berupa file, lebih sederhana dan aman. Menambah konten cukup menambah entri, tanpa halaman admin.

**Status:** Aktif

## ADR-003: Chatbot lewat server dengan Gemini (paket gratis)

**Keputusan:** Chatbot masuk MVP, dikerjakan **terakhir** di dalam MVP. Dipanggil lewat endpoint server `/api/chat`, memakai Gemini API paket gratis. API key hanya di server.

**Alasan:** Key tidak boleh terlihat di browser. Kuota gratis dipakai bersama semua pengunjung, jadi butuh batas penggunaan dan penanganan kuota habis. Web harus tetap berfungsi tanpa chatbot.

**Catatan terbuka:** Cara menyimpan hitungan batas penggunaan di lingkungan serverless belum diputuskan (lihat `ARCHITECTURE.md` bagian 11).

**Status:** Aktif

## ADR-004: Library animasi

**Keputusan:** Motion (komponen), GSAP (urutan dan scroll), three.js (satu scene hero, terbatas). Popmotion dan mo.js **tidak dipakai**.

**Alasan:** Popmotion sudah tercakup Motion. mo.js dapat digantikan GSAP. Terlalu banyak library animasi membuat web berat dan saling bentrok.

**Konsekuensi:** Satu elemen hanya dianimasikan oleh satu library. Library baru hanya boleh ditambah dengan ADR baru.

**Status:** Aktif

## ADR-005: React Bits sebagai kode sumber yang disalin

**Keputusan:** Komponen React Bits dipakai dalam varian TypeScript + Tailwind, disalin ke `src/components/ui/react-bits/`, ditinjau sebelum dipakai, dan disesuaikan dengan token desain. Maksimal satu latar WebGL per halaman.

**Alasan:** Kode menjadi milik proyek dan bisa diaudit. Pembatasan menjaga performa dan mencegah tampilan terasa seperti template.

**Status:** Aktif

## ADR-006: Warna

**Keputusan:** Latar gelap dengan aksen violet neon. Token: `bg #0A0A0F`, `surface #14141C`, `text #F4F4F8`, `muted #A1A1B5`, `primary #8B5CF6`, `primary-strong #7C3AED`, `primary-light #A78BFA`, `primary-soft #C4B5FD`.

**Alasan:** Cocok dengan tema cyber dan efek cahaya di sekitar foto. Teks putih di atas `#8B5CF6` hanya berkontras 4,23, jadi tombol utama memakai `#7C3AED` (5,7). Merah dihindari agar tidak menyerupai merek streaming.

**Status:** Aktif

## ADR-007: Tipografi

**Keputusan:** Space Grotesk (judul), Inter (isi), JetBrains Mono (label).

**Alasan:** Modern, teknis, mudah dibaca, dan tahan lama.

**Status:** Aktif

## ADR-008: Gaya hero

**Keputusan:** Foto tanpa latar (cutout) dengan lingkaran cahaya violet, kartu kaca melayang, foto bergeser sedikit mengikuti kursor, dan efek scanning. Nama lengkap **selalu satu baris**. Tempo animasi campuran: pembukaan sinematik sekali dan bisa dilewati, bagian lain gesit.

**Alasan:** Kesan pertama sinematik yang personal, namun informasi utama tetap terlihat cepat untuk rekruter yang hanya punya beberapa detik.

**Status:** Aktif

## ADR-009: Bahasa

**Keputusan:** Isi web dalam Bahasa Indonesia. Kode, nama file, dan pesan commit dalam Bahasa Inggris. Chatbot menjawab dalam bahasa penanya.

**Alasan:** Pilihan pemilik. Dua bahasa dikeluarkan dari MVP.

**Status:** Aktif

## ADR-010: Hosting dan alur rilis

**Keputusan:** Vercel. Alur: lokal → preview → QA → production. Repository GitHub `0xims` dibuat **Private** dan dijadikan Public hanya setelah audit rahasia lolos.

**Alasan:** Terintegrasi dengan Next.js. Preview mencegah production menjadi tempat uji pertama. Repository Public baru aman jika tidak ada rahasia di kode maupun riwayat Git.

**Status:** Aktif

## ADR-011: Alat kerja

**Keputusan:** Pengembangan lewat vibe coding di Windows dengan PowerShell. Freebuff sebagai pembangun, GitHub Copilot (VS Code) sebagai penyunting dan reviewer. Commit sebelum berpindah alat.

**Alasan:** Gratis atau terjangkau untuk mahasiswa, dan dua alat saling memeriksa. Freebuff didanai iklan dan memakai model luar, jadi data pribadi dan rahasia tidak boleh dimasukkan ke prompt.

**Status:** Aktif

## ADR-012: Di luar cakupan versi pertama

**Keputusan:** Tidak dibuat di versi pertama: halaman admin, dua bahasa, komentar atau buku tamu, pembayaran atau jual jasa, aplikasi mobile, blog, dan halaman khusus lab dan tools.

**Alasan:** Mencegah proyek melebar. Sebagian bisa ditambah nanti lewat ADR baru.

**Status:** Aktif

## ADR-013: Belajar Bareng hanya dikelola pemilik

**Keputusan:** Pengunjung hanya bisa mengunduh. Tidak ada unggahan dari pengunjung. Hanya file buatan sendiri atau yang boleh dibagikan, bebas data pribadi dan metadata. File kecil di `public/belajar`, file besar lewat tautan eksternal.

**Alasan:** Menghindari risiko keamanan unggahan, masalah hak cipta, dan kebocoran data pribadi.

**Status:** Aktif

## ADR-014: Identitas Git

**Keputusan:** Commit memakai alamat email noreply GitHub, bukan email pribadi.

**Alasan:** Alamat email di riwayat commit terlihat publik jika repository menjadi Public.

**Status:** Aktif

## ADR-015: Kebijakan dependensi

**Keputusan:** Paket baru hanya dipasang dengan persetujuan pemilik setelah diverifikasi di registry resmi (nama, penerbit, pemakai, tanggal rilis). Versi dikunci lewat lockfile. Paket yang baru dirilis diberi jeda sebelum dipakai.

**Alasan:** AI kadang mengarang nama paket, dan paket jahat bisa meniru nama yang mirip.

**Status:** Aktif
