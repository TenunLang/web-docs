# modul-rute

Router untuk bahasa [Tenun](https://github.com/TenunLang/Tenun). Cocokkan pola jalur seperti `/user/:id` dan ambil parameternya.

## Pasang

```
tenun add rute
```

## Pakai

```tenun
impor "rute";

fungsi tangani(metode: teks, jalur: teks, badan: teks): teks {
    kalau rute_cocok("/user/:id", jalur) {
        biar id: teks = rute_param("/user/:id", jalur, "id");
        kembali "Profil pengguna " + id;
    }
    statusKan(404);
    kembali "tidak ditemukan";
}
layani(8080);
```

## Fungsi

- `rute_cocok(pola: teks, jalur: teks): bool` — segmen `:nama` cocok dengan apa pun. Query string diabaikan.
- `rute_param(pola: teks, jalur: teks, nama: teks): teks` — nilai parameter, `""` bila tidak ada.
- `rute_jalur(jalur: teks): teks` — jalur tanpa query string.

## Lisensi

MIT.
