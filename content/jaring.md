# modul-jaring

HTTP client + parser HTML/XML untuk bahasa [Tenun](https://github.com/TenunLang/Tenun) — **axios + cheerio dalam satu paket** untuk scraping. TLS (https) otomatis.

## Pasang

```
tenun add jaring
```

## Scraping cepat

```tenun
impor "jaring";

biar html: teks = jaring_get("https://example.com");   // https = TLS otomatis
cetak(html_judul(html));                                 // Example Domain
cetak(html_teks(html_tag(html, "p")));                   // teks paragraf
untuk t dari html_tautan(html) { cetak(t); }             // semua href
```

## HTTP (axios-like)

- `jaring_get(url): teks` — GET (TLS otomatis).
- `jaring_post(url, body): teks` — POST JSON.
- `jaring_get_ua(url, ua): teks` — GET dengan User-Agent kustom.
- `jaring_minta(metode, url, header, body): teks` — permintaan umum (header: baris `Nama: Nilai` dipisah `\n`).

## Parser HTML/XML (cheerio-like)

- `html_teks(html): teks` — buang semua tag, ambil teks polos.
- `html_tag(html, tag): teks` — isi elemen pertama `<tag>...</tag>` (jalan untuk XML juga).
- `html_semua(html, tag): []teks` — isi semua elemen `<tag>`.
- `html_atribut(html, nama): teks` — nilai atribut (mis. `href`).
- `html_semua_atribut(html, nama): []teks` — semua nilai atribut.
- `html_tautan(html): []teks` — semua `href`. `html_gambar(html): []teks` — semua `src`.
- `html_judul(html): teks` — `<title>`.
- `html_antara(html, awal, akhir): teks` — substring antara dua penanda.

## Catatan

Parser berbasis teks (cepat & ringan, cocok untuk scraping praktis), bukan DOM penuh / CSS selector. Untuk XML, pakai `html_tag`/`html_semua` dengan nama tag XML.

## Lisensi

MIT.
