# modul-json

Bangun & baca JSON untuk bahasa [Tenun](https://github.com/TenunLang/Tenun).

## Pasang

```
tenun add json
```

## Pakai

```tenun
impor "json";

biar j: teks = json_objek(["nama", "kota"], ["Budi", "Jakarta"]);
// {"nama":"Budi","kota":"Jakarta"}

cetak(json_ambil(j, "nama"));      // Budi
cetak(json_larik(["a", "b"]));      // ["a","b"]
```

## Fungsi

Membangun:

- `json_str(s: teks): teks` — string JSON ber-kutip (di-escape).
- `json_escape(s: teks): teks` — escape isi string JSON.
- `json_objek(kunci: []teks, nilai: []teks): teks` — objek dari pasangan (nilai teks).
- `json_larik(nilai: []teks): teks` — larik teks.

Membaca (field top-level):

- `json_ambil(j: teks, kunci: teks): teks`
- `json_angka(j: teks, kunci: teks): bulat`
- `json_bool(j: teks, kunci: teks): bool`

## Catatan

- Pembacaan saat ini untuk field top-level (skalar). Objek/larik bersarang menyusul (memerlukan map/struct di bahasa inti).

## Lisensi

MIT.
