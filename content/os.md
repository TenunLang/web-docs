# modul-os

Informasi sistem operasi untuk bahasa [Tenun](https://github.com/TenunLang/Tenun): nama OS, arsitektur, hostname, pengguna, jumlah CPU, direktori, variabel lingkungan.

## Pasang

```
tenun add os
```

## Pakai

```tenun
impor "os";

cetak(os_nama());        // "windows" | "linux" | "macos"
cetak(os_arch());        // "x86_64" | "aarch64"
cetak(os_host());        // nama komputer
cetak(os_pengguna());    // user
cetak(keTeks(os_cpu())); // jumlah core
cetak(os_cwd());         // direktori kerja
cetak(os_env("PATH"));   // variabel lingkungan

kalau os_windows() { cetak("di Windows"); }
```

## Fungsi

- `os_nama(): teks` — nama OS.
- `os_arch(): teks` — arsitektur CPU.
- `os_host(): teks` — hostname.
- `os_pengguna(): teks` — username.
- `os_cpu(): bulat` — jumlah core.
- `os_cwd(): teks` — direktori kerja.
- `os_temp(): teks` — direktori temporer.
- `os_env(nama: teks): teks` — variabel lingkungan.
- `os_windows(): bool` / `os_linux(): bool`.
- `os_info(): peta` — ringkasan semua info sebagai peta.

## Lisensi

MIT.
