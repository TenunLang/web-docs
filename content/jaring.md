# modul-jaring

HTTP client + parser HTML/XML untuk bahasa [Tenun](https://github.com/TenunLang/Tenun) — **axios + cheerio dalam satu paket** untuk scraping. TLS (https) otomatis.

## Pasang

```
tenun add jaring
```

## Scraping + traversal (cheerio-style)

```tenun
impor "jaring";

biar html: teks = jaring_get("https://toko.contoh/produk");   // https = TLS

// pilih semua elemen .produk, lalu drill ke anak tiap elemen
untuk kartu dari html_kelas(html, "produk") {
    biar nama: teks = html_tag(kartu, "h2");        // anak <h2>
    biar harga: teks = html_tag(kartu, "span");     // anak <span>
    biar link: teks = html_atribut(kartu, "href");  // atribut
    cetak(nama + " - " + harga + " -> " + link);
}
```

## HTTP (axios-like)

- `jaring_get(url): teks` — GET (TLS otomatis).
- `jaring_post(url, body): teks` — POST JSON.
- `jaring_get_ua(url, ua): teks` — GET dengan User-Agent kustom.
- `jaring_minta(metode, url, header, body): teks` — permintaan umum.

## Selector (CSS & XPath)

```tenun
// CSS selector (tag, .class, #id, tag.class, kombinator keturunan)
untuk a dari html_pilih(html, ".list li a") {
    cetak(html_teks(a) + " -> " + html_atribut(a, "href"));
}
biar app: teks = html_pilih_satu(html, "#app");      // pertama
biar item: []teks = html_pilih(html, "li.item");     // tag + class

// XPath subset
biar n: []teks = html_xpath(html, "//a[@href='/b']");
```

- `html_pilih(html, css): []teks` — CSS selector. Dukung: `tag`, `.class`, `#id`, `tag.class`, `tag#id`, dan kombinator keturunan (dipisah spasi, mis. `div.produk h2 a`).
- `html_pilih_satu(html, css): teks` — hasil pertama.
- `html_xpath(html, xp): []teks` — `//tag` dan `//tag[@atribut='nilai']`.

## Parser & traversal (cheerio-like)

**Elemen (outer, sadar nesting — bisa di-drill ke anak):**
- `html_elemen(html, tag): teks` — elemen luar `<tag>...</tag>` pertama.
- `html_semua_elemen(html, tag): []teks` — semua elemen luar bertag.
- `html_kelas(html, kelas): []teks` — semua elemen yang punya class itu.
- `html_id(html, id): teks` — elemen dengan id itu.

**Isi & atribut:**
- `html_tag(html, tag): teks` — isi dalam elemen pertama.
- `html_semua(html, tag): []teks` — isi semua elemen bertag.
- `html_teks(html): teks` — buang tag, ambil teks polos.
- `html_atribut(html, nama): teks` / `html_semua_atribut(html, nama): []teks`.
- `html_tautan(html): []teks` (href), `html_gambar(html): []teks` (src).
- `html_judul(html): teks`, `html_antara(html, awal, akhir): teks`.

Karena fungsi traversal menerima & mengembalikan string HTML, kamu bisa **bersarang**: ambil elemen induk lalu jalankan query lagi di dalamnya (seperti `$(el).find(...)` di cheerio). Jalan juga untuk XML.

## Lisensi

MIT.
