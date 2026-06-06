# modul jala — kerangka web MVC

**Jala** kerangka kerja web MVC untuk Tenun (rasa Laravel/CodeIgniter, istilah Indonesia). Di atas modul `web` + `tampilan` (Batik).

```
Permintaan → Rute → Kontroler → (Model) → Tampilan → Respons
```

## Mulai cepat

```
tenun baru blog        # scaffold proyek (mirip `laravel new`)
cd blog
tenun add jala         # pasang jala + web + tampilan
tenun run app.tenun    # http://localhost:8080
```

Struktur hasil scaffold (nama folder teknis/English, kode Indonesia):

```
app.tenun                bootstrap
routes.tenun             rute -> controller
app/
  config.tenun
  controllers/home.tenun
  models/example.tenun
views/
  layout.batik           layout ({{konten}})
  home.batik  about.batik
  partials/header.batik  ({{> partials/header}})
public/style.css         statis
```

## Inti

```tenun
// app/controllers/home.tenun
fungsi home_index(): teks {
    kembali jala_tampil_tata("layout", "home", peta{ "judul": "Beranda", "pesan": "Halo!" });
}
// routes.tenun
jala_get("/", home_index);
jala_get("/pos/:id", pos_lihat);
// app.tenun
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

## Model

Pakai `orm` (MySQL/Postgres) atau kunci-nilai bawaan. Contoh todo lengkap ada di `contoh/`.
