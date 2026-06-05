# modul-tampilan — Batik

**Batik**, template engine teks untuk bahasa [Tenun](https://github.com/TenunLang/Tenun). Format berkas `.batik`. Generik (bukan khusus HTML) — cocok untuk view web, email, config, atau codegen. Data berupa `peta`. Ditulis sepenuhnya dengan Tenun.

## Pasang

```
tenun add tampilan
```

## Sintaks `.batik`

```
{{kunci}}                          substitusi nilai
{{#jika kunci}}...{{/jika kunci}}  tampil bila kunci "truthy"
{{#kecuali k}}...{{/kecuali k}}    tampil bila kunci "falsy"
{{! komentar }}                    dibuang saat render
```

"Truthy" = bukan `""`, `"0"`, `"salah"`, `"false"`. Penutup seksi menyertakan nama kunci (`{{/jika nama}}`) agar seksi berbeda bisa bersarang.

## Pakai

```tenun
impor "tampilan";

// Render string.
biar out: teks = batik_render("Halo {{nama}}", peta{ "nama": "Taqin" });

// Render berkas .batik.
biar html: teks = batik("view/halaman.batik", peta{ "nama": "Sri", "masuk": "1" });

// Loop daftar baris.
biar daftar: []peta = [
    peta{ "judul": "Kopi", "harga": "20000" },
    peta{ "judul": "Teh", "harga": "10000" }
];
biar baris: teks = batik_daftar("- {{judul}} (Rp{{harga}})\n", daftar);
```

Contoh berkas `halaman.batik`:

```
<h1>Halo {{nama}}</h1>
{{#jika pesan}}<p>{{pesan}}</p>{{/jika pesan}}
{{#kecuali masuk}}<a href="/masuk">Masuk</a>{{/kecuali masuk}}
```

## Fungsi

- `batik(berkas: teks, data: peta): teks` — render berkas `.batik`.
- `batik_render(tmpl: teks, data: peta): teks` — render string.
- `batik_daftar(itemTmpl: teks, daftar: []peta): teks` — render & gabung tiap baris.
- `tampilan_render` / `tampilan_render_berkas` / `tampilan_daftar` — nama panjang (sama isinya).
- `tampilan_render_aman(tmpl, data): teks` — render dengan nilai di-escape HTML (cegah XSS).
- `tampilan_tata(layout, konten): teks` — sisipkan `konten` ke `{{konten}}` di layout.
- `tampilan_escape(s): teks` — escape karakter HTML (opsional).

API lama tetap ada: `tampilan`, `isi`, `isi_mentah`.

## Dengan server

```tenun
impor "tampilan";

fungsi tangani(metode: teks, jalur: teks, badan: teks): teks {
    headerKan("Content-Type", "text/html; charset=utf-8");
    kembali batik("view/index.batik", peta{ "nama": "Dunia" });
}
layani(8080);
```

## Struktur

```
modul-tampilan/
  tenun.json
  src/tampilan.tenun
  contoh/contoh.tenun
  contoh/halaman.batik
```

## Lisensi

MIT.
