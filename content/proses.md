# modul-proses

Jalankan perintah sistem (exec) dari bahasa [Tenun](https://github.com/TenunLang/Tenun).

## Pasang

```
tenun add proses
```

## Pakai

```tenun
impor "proses";

cetak(proses_keluaran("echo halo"));     // "halo"
biar baris: []teks = proses_baris("ls"); // tiap baris output
untuk b dari baris { cetak(b); }
```

## Fungsi

- `proses_jalankan(perintah: teks): teks` — stdout mentah.
- `proses_keluaran(perintah: teks): teks` — stdout, dipangkas.
- `proses_baris(perintah: teks): []teks` — stdout dipecah per baris.

Perintah dijalankan lewat shell (`sh -c` di Unix, `cmd /c` di Windows).

## Lisensi

MIT.
