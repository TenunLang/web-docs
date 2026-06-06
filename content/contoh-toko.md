# Contoh: Toko Tenun & Batik (e-commerce)

Aplikasi e-commerce lengkap dengan kerangka **Jala** (MVC): [github.com/TenunLang/toko-tenun](https://github.com/TenunLang/toko-tenun).

## Fitur
- Etalase: katalog, filter kategori, pencarian, detail produk.
- Keranjang per-sesi, checkout, halaman sukses.
- Akun: daftar/masuk/keluar (sandi PBKDF2 via modul `auth`).
- Admin: dashboard (omzet), kelola produk & pesanan (DataTables), live chat.
- Live chat WebSocket (widget pelanggan + panel admin).
- UI: Bootstrap 5 + jQuery + DataTables, responsif.

## Jalankan
```
tenun add jala
tenun add websocket
tenun                 # http://localhost:8080
tenun chat.tenun      # live chat ws://localhost:3000 (proses terpisah)
```
Admin: `admin@toko.id` / `admin123`.

## Struktur (ala Laravel)
```
index.tenun  chat.tenun  Routes.tenun
App/{Config,Helper}.tenun
App/Controllers/{Home,Produk,Keranjang,Checkout,Auth,Admin}.tenun
App/Models/{Produk,Keranjang,Pengguna,Pesanan}.tenun
Views/  (Layout + Partials + Home/Keranjang/Checkout/Auth/Admin)
Public/ (style.css, app.js)
```

## Skala besar (10rb–1jt+ pengguna)
Worker **stateless** di belakang load balancer; state (sesi, keranjang, cache, rate-limit) di **Redis**; data di **MySQL/Postgres** (modul `orm`) dengan index + read-replica; aset via CDN; live chat fan-out via Redis Pub/Sub. Tanpa state lokal -> scaling horizontal aman (mis. k8s HPA).
