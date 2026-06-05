# web-docs

Situs dokumentasi resmi [Tenun](https://github.com/TenunLang/Tenun) — **dibangun dan disajikan dengan Tenun sendiri**. Server pakai [modul-web](https://github.com/TenunLang/modul-web), template pakai [Batik](https://github.com/TenunLang/modul-tampilan).

## Jalankan

```
tenun add web
tenun add tampilan
tenun run src/server.tenun     # http://localhost:8080
```

Jalankan dari folder ini (template `view/*.batik` dan aset `publik/` dibaca relatif terhadap direktori kerja).

## Halaman

- `/` — beranda (fitur, pasang, perintah)
- `/bahasa` — referensi bahasa (tipe, fungsi, peta, first-class function, server)
- `/modul` — katalog modul resmi
- `/gaya.css` — gaya (disajikan via `web_berkas`)

## Struktur

```
web-docs/
  tenun.json          (butuh: web, tampilan)
  src/server.tenun    rute + handler + katalog modul
  view/
    tata.batik        layout (nav + {{konten}} + footer)
    beranda.batik
    bahasa.batik
    modul.batik
  publik/gaya.css
```

## Produksi

Server berjalan HTTP. Untuk HTTPS, terminasi TLS di reverse proxy (Caddy/nginx):

```
docs.tenun.dev {
    reverse_proxy localhost:8080
}
```

## Lisensi

MIT.
