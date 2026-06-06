# modul-berkas

Operasi berkas & direktori untuk bahasa [Tenun](https://github.com/TenunLang/Tenun).

## Pasang

```
tenun add berkas
```

## Pakai

```tenun
impor "berkas";

dir_buat("data");
berkas_tulis("data/catatan.txt", "halo\n");
berkas_tambah("data/catatan.txt", "baris kedua\n");
cetak(berkas_baca("data/catatan.txt"));

untuk nama dari dir_daftar("data") { cetak(nama); }
cetak(keTeks(berkas_ukuran("data/catatan.txt")));
```

## Fungsi

Berkas: `berkas_baca`, `berkas_tulis`, `berkas_tambah`, `berkas_ada`, `berkas_ukuran`, `berkas_hapus`.

Direktori: `dir_daftar(path): []teks`, `dir_buat`, `dir_hapus`, `dir_ada(path): bool`, `dir_baca_semua(path): peta` (nama→isi tiap berkas).

## Lisensi

MIT.
