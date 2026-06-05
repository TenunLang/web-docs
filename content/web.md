# modul-web

Core HTTP framework untuk bahasa [Tenun](https://github.com/TenunLang/Tenun) — rasa Express. Routing semua method + `:param`, middleware, CORS, cookie, parsing query/form/JSON, response helper, static file. Ditulis sepenuhnya dengan Tenun (pakai first-class functions).

## Pasang

```
tenun add web
```

## Contoh

```tenun
impor "web";

fungsi beranda(): teks { kembali web_teks("Halo!"); }
fungsi user(): teks { kembali web_json("{\"id\":\"" + web_param("id") + "\"}"); }
fungsi buat(): teks { web_status(201); kembali web_json("{\"nama\":\"" + web_json_teks("nama") + "\"}"); }

web_cors("*");            // CORS + auto preflight OPTIONS
web_pakai(web_log);       // middleware

web_get("/", beranda);
web_get("/user/:id", user);
web_post("/user", buat);

fungsi tangani(metode: teks, jalur: teks, badan: teks): teks {
    kembali web_tangani(metode, jalur, badan);
}
layani(8080);
```

Handler adalah `fungsi(): teks` — kembalikan badan respons; akses request lewat helper di bawah.

## Routing

- `web_get/web_post/web_put/web_hapus/web_patch/web_opsi(pola, handler)` — daftar rute.
- Pola mendukung parameter: `"/user/:id"` → `web_param("id")`.
- `web_tangani(metode, jalur, badan)` — dispatcher; panggil dari `tangani`.

## Request

- `web_param(nama)` — parameter rute (`:id`).
- `web_kueri(nama)` — query string (`?a=1`).
- `web_form(nama)` — field form urlencoded.
- `web_json_teks(kunci)` / `web_json_angka(kunci)` — field JSON body.
- `web_header(nama)` — header masuk. `web_cookie(nama)` — cookie. `web_badan_mentah()`.

## Response

- `web_teks(s)` / `web_html(s)` / `web_json(s)` — set Content-Type + badan.
- `web_status(kode)`, `web_redirect(url)`, `web_404()`.
- `web_berkas(path)` — sajikan satu file (Content-Type dari ekstensi).

## Berkas statis

- `web_statik(dir)` — sajikan berkas statis dari `dir` untuk request yang tidak cocok rute mana pun. Mis. `web_statik("publik")` → `GET /gaya.css` melayani `publik/gaya.css`, `GET /` melayani `publik/index.html`. Aman dari path traversal (`..` ditolak).

## Cookie

- `web_set_cookie(nama, nilai)`, `web_set_cookie_opsi(nama, nilai, opsi)`, `web_hapus_cookie(nama)`.

## CORS & Middleware

- `web_cors(origin)` — aktifkan CORS untuk semua rute + auto-jawab preflight OPTIONS (204).
- `web_pakai(mw)` — daftar middleware `fungsi(): teks` (jalan berurutan sebelum handler). Kembalikan `""` untuk lanjut, atau teks untuk menghentikan (short-circuit). `web_log` tersedia bawaan.

## Belum di sini (modul terpisah / menyusul)

- **SSL/HTTPS server** — butuh builtin server TLS (`layani` saat ini HTTP).
- **WebSocket** — `modul-websocket` (perlu upgrade soket mentah).
- **gRPC** — `modul-grpc`.

## Struktur

```
modul-web/
  tenun.json
  src/
    web.tenun          entry: konteks request, dispatcher, CORS
    rute.tenun         tabel rute + :param matching
    permintaan.tenun   query/form/JSON/header/cookie
    respons.tenun      teks/html/json/status/redirect/file
    cookie.tenun       set/hapus cookie
    middleware.tenun   web_pakai + web_log
  contoh/contoh.tenun
```

## Lisensi

MIT.
