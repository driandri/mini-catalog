# gzn Catalog

gzn Catalog adalah sebuah aplikasi katalog mini interaktif yang dibangun menggunakan Vue.js dan Vite. Aplikasi ini didesain dengan konsep desain modern premium (glassmorphism dan dark mode), sehingga memberikan pengalaman visual tersendiri tanpa mengandalkan library CSS eksternal seperti Tailwind CSS atau Bootstrap. 

Semua aksi CRUD (*Create, Read, Update, Delete*) terhubung langsung dengan **LocalStorage**, sehingga data Anda tidak akan hilang saat browser dimuat ulang *(reload)*.

## 🌟 Fitur

- **Modern & Premium UI**: Tampilan memukau dengan desain dark-theme, grid system yang dinamis, efek hover, dan border-radius modern.
- **Responsif**: Tampilan dapat menyesuaikan dengan berbagai ukuran layar *(mobile, tablet, desktop)* dengan sempurna.
- **CRUD Operations**:
  - **Create**: Tambahkan produk atau item ke katalog dengan form yang intuitif.
  - **Read**: Tampilkan list item katalog dalam bentuk kartu *(grid cards)* yang disajikan menarik.
  - **Update**: Edit rincian produk kapan saja dengan sekali klik.
  - **Delete**: Hapus item yang tidak diinginkan dari katalog.
- **Pencarian (Search)**: Cari produk berdasarkan nama secara real-time.
- **Data Persistence**: Semua perubahan akan secara otomatis disimpan di LocalStorage browser.

## 🚀 Cara Menjalanka   n

Aplikasi ini menggunakan [Vite](https://vitejs.dev/) untuk pengelolaan proyeknya. Pastikan Anda telah menginstal **Node.js** pada mesin Anda.

1. **Persiapan Dependensi**
   Buka terminal, arahkan ke direktori root proyek ini, dan instal dependensinya:
   ```bash
   npm install
   ```

2. **Menjalankan Development Server**
   Setelah semua dependensi terinstal, jalankan server pengembangan:
   ```bash
   npm run dev
   ```

3. **Membuka Aplikasi**
   Setelah server berjalan, biasanya aplikasi dapat diakses melalui browser pada URL berikut:
   ```
   http://localhost:5173/
   ```

## 🛠️ Teknologi yang Digunakan

- **Vue.js 3** (Composition API `<script setup>`)
- **Vite** (Build Tool)
- **Vanilla CSS** (Custom Premium Styling)
- **LocalStorage API** (Data Storage)
