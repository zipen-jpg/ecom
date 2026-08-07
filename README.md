# âš¡ Toko Online (Vanilla JS + LocalStorage)

Aplikasi e-commerce ringan yang berjalan 100% di browser â€” **tanpa server backend, tanpa `npm install`**. Cocok dijalankan langsung dari **Acode di Android**, cukup buka `index.html` dengan browser.

## âœ¨ Fitur

**Sisi Pelanggan (`index.html`)**
- Navigasi dengan logo, search bar, dan tombol keranjang bercounter
- Katalog produk berbentuk grid responsif
- Pencarian & filter kategori secara real-time
- Modal keranjang belanja: ubah qty, lihat total, dan checkout
- Checkout otomatis mengurangi stok produk

**Sisi Admin (`admin.html`)**
- Login admin (username `admin`, password `admin123`), status login disimpan di `sessionStorage` â€” otomatis diarahkan ke form login jika belum masuk atau setelah logout
- Sidebar navigasi responsif dengan 4 menu: Dashboard, Data Produk, Laporan Penjualan, Grafik
- Dashboard: total produk, total transaksi, total pendapatan, pendapatan hari ini, dan produk terlaris
- Data Produk: CRUD (Tambah, Ubah, Hapus) lengkap dengan nama, harga, kategori, stok, dan gambar
- Laporan Penjualan: tabel transaksi (Tanggal, ID, Total, Jumlah Produk, Status) dengan filter rentang tanggal dan modal detail per transaksi
- Grafik: grafik garis pendapatan 7 hari terakhir dan grafik pie produk terlaris (Chart.js)
- Tombol logout dan reset data contoh di sidebar

## ðŸ§± Tech Stack

| Bagian | Teknologi |
|---|---|
| Struktur & Style | HTML5 + [Tailwind CSS via CDN](https://cdn.tailwindcss.com) |
| Logika | Vanilla JavaScript (tanpa framework) |
| Grafik | [Chart.js via CDN](https://www.chartjs.org/) (khusus halaman Grafik di admin) |
| "Database" | `localStorage` bawaan browser |
| Font | Google Fonts (Sora, Plus Jakarta Sans, JetBrains Mono) |

Tidak ada proses build. Tidak ada dependensi npm. File langsung bisa dibuka di browser apa pun.

## ðŸ“‚ Struktur File

```
toko-online/
â”œâ”€â”€ index.html   # Halaman toko (pelanggan)
â”œâ”€â”€ admin.html   # Panel kelola produk (admin)
â””â”€â”€ README.md
```

## ðŸ—„ï¸ Cara Kerja LocalStorage

Semua data disimpan di `localStorage` milik browser, dalam 3 key berikut:

| Key | Isi | Diisi/diubah oleh |
|---|---|---|
| `db_produk_tokohijau` | Array daftar produk (`id, nama, harga, kategori, stok, gambar, warna`) | Admin (tambah/ubah/hapus), Pelanggan (stok berkurang saat checkout) |
| `db_transaksi_tokohijau` | Array riwayat transaksi (`id, tanggal, items, total`). Tiap item transaksi juga menyimpan `kategori` produk saat transaksi terjadi, agar grafik "Penjualan per Kategori" tetap akurat meski produk diubah/dihapus kemudian | Pelanggan (saat checkout) |
| `keranjang` | Array isi keranjang belanja pelanggan (`id, qty`) | Pelanggan |

Karena `index.html` dan `admin.html` membaca **key `localStorage` yang sama**, perubahan stok/produk di satu halaman otomatis terlihat saat halaman lain dibuka atau saat direfresh. Kedua halaman juga mendengarkan event `storage`, sehingga jika keduanya dibuka di dua tab browser sekaligus, tampilan ikut memperbarui diri secara otomatis.

Saat pertama kali dijalankan (localStorage kosong), aplikasi otomatis mengisi 8 produk contoh (`seedData()`) agar toko tidak terlihat kosong.

> **Catatan penting:** `localStorage` tersimpan per-**origin** browser. Jika kamu membuka file langsung lewat `file://` (klik dua kali file), beberapa browser (terutama Chrome di desktop) memperlakukan setiap file `.html` sebagai origin terpisah sehingga data **tidak akan sinkron** antara `index.html` dan `admin.html`. Agar sinkron dengan andal, jalankan lewat server lokal â€” lihat langkah di bawah.

## ðŸ“± Cara Menjalankan di Acode (Android)

1. Buat folder proyek di Acode, lalu salin `index.html`, `admin.html`, dan `README.md` ke dalamnya.
2. Pasang plugin **"Acode Live Preview"** (atau plugin server lokal sejenis) dari Acode Plugins jika belum ada.
3. Buka `index.html`, lalu jalankan lewat tombol **Run/Preview** di Acode (bukan cukup dibuka langsung sebagai file) agar server lokal aktif dan `localStorage` konsisten antar halaman.
4. Uji coba: tambah produk lewat halaman Admin â†’ kembali ke halaman Toko â†’ produk baru langsung muncul.

## ðŸ’» Cara Menjalankan di Komputer / GitHub Pages

- **Lokal:** jalankan server statis sederhana di folder proyek, misalnya `python3 -m http.server`, lalu buka `http://localhost:8000`.
- **GitHub Pages:** push folder ini ke repo GitHub, aktifkan GitHub Pages dari Settings â†’ Pages, pilih branch & folder root. Aplikasi otomatis online tanpa server tambahan.

## âš ï¸ Batasan

- Data hanya tersimpan di browser perangkat itu sendiri â€” tidak ada sinkronisasi antar perangkat/pengguna lain.
- Menghapus cache/data situs di browser akan menghapus seluruh data toko.
- Login admin (`admin` / `admin123`) di-hardcode di JavaScript sisi klien dan disimpan sebagai flag di `sessionStorage` (otomatis "keluar" saat tab ditutup). Ini hanya cocok untuk demo/prototipe lokal â€” siapa pun yang membuka DevTools bisa melihat kredensialnya, dan **tidak boleh dipakai untuk data sungguhan** tanpa autentikasi backend yang layak.
- Cocok untuk demo, latihan, atau prototipe â€” bukan untuk transaksi produksi sungguhan (belum ada pembayaran nyata atau backend).

## ðŸ”§ Ide Pengembangan Selanjutnya

- Autentikasi login sederhana untuk halaman admin
- Upload gambar produk (base64) alih-alih hanya URL
- Ekspor/impor data produk & transaksi sebagai file JSON
- Migrasi ke backend nyata (misalnya Firebase) jika butuh sinkron multi-perangkat
# ecom