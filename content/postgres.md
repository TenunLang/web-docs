# modul-postgres

Klien PostgreSQL untuk bahasa [Tenun](https://github.com/TenunLang/Tenun), ditulis sepenuhnya dengan Tenun. Protokol v3.

## Pasang

```
tenun add postgres
```

## Pakai

```tenun
impor "postgres";

biar db: bulat = pg_sambung("localhost", 5432, "postgres", "", "postgres");
kalau db < 0 {
    cetak("gagal terhubung");
} lain {
    biar h: teks = pg_kueri(db, "SELECT id, nama FROM pengguna");
    biar baris: bulat = 0;
    selama baris < pg_jumlah_baris(h) {
        cetak(pg_ambil(h, baris, 0) + " - " + pg_ambil(h, baris, 1));
        baris = baris + 1;
    }
    pg_tutup(db);
}
```

## Fungsi

- `pg_sambung(inang: teks, port: bulat, pengguna: teks, sandi: teks, db: teks): bulat`
  Sambung + autentikasi. Mengembalikan handle, atau `-1`.
- `pg_tutup(soket: bulat): kosong`
- `pg_kueri(soket: bulat, sql: teks): teks` — jalankan query, kembalikan respons mentah (untuk dibaca fungsi di bawah).
- `pg_jumlah_baris(hasil: teks): bulat`
- `pg_ambil(hasil: teks, baris: bulat, kolom: bulat): teks` — nilai sel (0-indeks); `NULL` → `""`.
- `pg_galat(hasil: teks): teks` — pesan galat (kosong bila tidak ada).

## Autentikasi

Didukung: `trust`, `cleartext`, `md5`, dan **`scram-sha-256`** (default PostgreSQL modern). Implementasi SCRAM (RFC 5802/7677) memakai builtin kripto `pbkdf2`/`hmacSha256Raw`/`sha256Raw`/`xor`/`base64` — ClientProof terverifikasi terhadap vektor uji RFC 7677.

## Struktur

```
modul-postgres/
  tenun.json
  src/
    postgres.tenun    koneksi, query (sederhana & berparameter), parser
    scram.tenun       autentikasi SCRAM-SHA-256
  contoh/contoh.tenun
  .github/workflows/ci.yml
```

## Lisensi

MIT.
