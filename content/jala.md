# modul jala — kerangka web MVC

**Jala** kerangka kerja web MVC untuk Tenun (rasa Laravel/CodeIgniter, istilah Indonesia). Di atas modul `web` + `tampilan` (Batik).

```
Permintaan → Rute → Kontroler → (Model) → Tampilan → Respons
```

## Mulai cepat

```
tenun add jala                                  # ambil Jala (+ CLI)
tenun tenun_modul/jala/cli/jala.tenun baru Blog # scaffold proyek
cd Blog
tenun add jala                                  # pasang dependensi
tenun                                           # jalankan index.tenun -> http://localhost:8080
```

`tenun <file.tenun>` jalan langsung (tanpa `run`); `tenun` tanpa argumen menjalankan `index.tenun`. CLI scaffold = program Tenun (`cli/jala.tenun`) di dalam modul, bukan compiler inti.

Struktur hasil scaffold (folder berhuruf besar ala Laravel, kode Indonesia):

```
index.tenun              titik masuk (`tenun` menjalankan ini)
nakhoda.tenun            CLI proyek (generator)
Routes.tenun             rute -> controller
App/
  Config.tenun
  Controllers/Home.tenun
  Models/Example.tenun
Views/
  Layout.batik           layout ({{konten}})
  Home.batik  About.batik
  Partials/Header.batik  ({{> Partials/Header}})
Public/style.css         statis
```

## Inti

```tenun
// App/Controllers/Home.tenun
fungsi home_index(): teks {
    kembali jala_tampil_tata("Layout", "Home", peta{ "judul": "Beranda", "pesan": "Halo!" });
}
// Routes.tenun
jala_get("/", home_index);
jala_get("/pos/:id", pos_lihat);
// index.tenun
fungsi tangani(m: teks, j: teks, b: teks): teks { kembali jala_tangani(m, j, b); }
jala_layani(8080);
```

## API

- **Rute**: `jala_get/post/put/hapus/patch`, `jala_grup(prefix, daftar)`, `jala_param`, `jala_namai`/`jala_url`.
- **Permintaan**: `jala_input`, `jala_kueri`, `jala_form`, `jala_json_teks/angka`, `jala_header`, `jala_metode/jalur/badan`. Validasi: `jala_perlu`, `jala_minimal`, `jala_ada_galat`, `jala_galat_pesan`.
- **Respons**: `jala_teks/html/json`, `jala_status`, `jala_redirect`, `jala_redirect_rute`, `jala_json_data(peta)`, `jala_json_peta`.
- **Tampilan (Batik)**: `jala_tampil(nama, data)`, `jala_tampil_tata(tata, nama, data)`, `jala_render`, `jala_daftar`, `jala_atur_tampilan(dir)`. Folder default `views/`. Sintaks: `{{kunci}}`, `{{#jika k}}…{{/jika k}}`, `{{> partials/nama}}`.
- **Aplikasi**: `jala_tangani`, `jala_layani(port)`, `jala_statik(dir)`, `jala_cors`, `jala_pakai(mw)`.
- **Konfig**: `jala_atur`, `jala_konfig`.
- **Keamanan**: `jala_terautentikasi(rahasia)`, `jala_wajib_masuk(rahasia)` (401), `jala_pengguna()` (JWT Bearer via modul auth); CSRF: `jala_csrf_field(id)`, `jala_csrf_cek(id)`.

## Generator (ala artisan make:)

Lewat `nakhoda.tenun` di proyek:

```
tenun nakhoda.tenun controller Produk   # App/Controllers/Produk.tenun
tenun nakhoda.tenun model Produk        # App/Models/Produk.tenun
tenun nakhoda.tenun view Produk         # Views/Produk.batik
```

## Model

Pakai `orm` (MySQL/Postgres) atau kunci-nilai bawaan. Contoh todo lengkap ada di `contoh/`.
