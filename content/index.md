# Tenun — Dokumentasi

Tenun adalah bahasa pemrograman dengan keyword Bahasa Indonesia, compiler ditulis Zig. Statis, cepat (bytecode VM + codegen native), dan siap untuk web.

Dokumen ini adalah titik masuk untuk membangun aplikasi dengan Tenun. Untuk LLM/agen: baca `bahasa` dan `builtin` dulu, lalu dokumen modul sesuai kebutuhan.

## Daftar dokumen

- `bahasa` — referensi bahasa lengkap (tipe, sintaks, fungsi, peta, first-class function)
- `grammar` — grammar EBNF
- `builtin` — semua fungsi builtin
- Modul web: `web` (HTTP framework), `rute` (router), `tampilan` (Batik template), `json`
- Realtime: `websocket`, `socketio`
- Database: `orm`, `mysql`, `postgres`, `redis`
- Layanan: `auth`, `sesi`, `mail`
- `contoh-toko` — contoh e-commerce lengkap (struktur ala Laravel)

## Pasang

```
# Linux / macOS
curl -fsSL https://raw.githubusercontent.com/TenunLang/Tenun/main/install.sh | bash
# Windows (PowerShell)
irm https://raw.githubusercontent.com/TenunLang/Tenun/main/install.ps1 | iex
```

## Perintah CLI

```
tenun run <file> [arg...]     jalankan program (VM). argumen lewat builtin argumen()
tenun run <file> --interp     jalankan via tree-walking interpreter
tenun build <file>            kompilasi ke executable native (<file>.exe)
tenun fmt <file>              rapikan format kode
tenun repl                    mode interaktif
tenun add <modul>             pasang modul dari GitHub (TenunLang/modul-<modul>)
tenun jalan <skrip>           jalankan "skrip" dari tenun.json (mirip npm/bun run)
```

## tenun.json (manifest)

```json
{
  "nama": "proyekku",
  "versi": "0.1.0",
  "berkas": "src/app.tenun",
  "butuh": { "web": "^0.1.0", "orm": "^0.1.0" },
  "skrip": { "layani": "run src/app.tenun", "migrasi": "run artisan.tenun migrasi" }
}
```

`tenun add <modul>` meng-clone repo `github.com/TenunLang/modul-<modul>` ke `tenun_modul/` dan otomatis memasang dependensinya.

## Hello World

```tenun
fungsi salam(nama: teks): teks {
    kembali "Halo, " + nama;
}
cetak(salam("Dunia"));
```

```
tenun run halo.tenun     # Halo, Dunia
```

## Server HTTP minimal

```tenun
fungsi tangani(metode: teks, jalur: teks, badan: teks): teks {
    kembali "Halo dari " + jalur;
}
layani(8080);
```

## Impor modul

```tenun
impor "web";              // modul (dari tenun_modul/web)
impor "./util.tenun";     // file lokal (relatif terhadap file ini)
```

Semua fungsi top-level dari file/modul yang diimpor tersedia secara global (di-hoist).
