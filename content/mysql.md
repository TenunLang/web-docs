# modul-mysql

Klien MySQL/MariaDB untuk bahasa [Tenun](https://github.com/TenunLang/Tenun), ditulis sepenuhnya dengan Tenun. Mendukung autentikasi `mysql_native_password` dan eksekusi query.

## Pasang

```
tenun add mysql
```

(mengkloning repo ini ke `./tenun_modul/mysql`)

## Pakai

```tenun
impor "mysql";

biar db: bulat = mysql_sambung("127.0.0.1", 3306, "root", "");
kalau db < 0 {
    cetak("gagal login");
} lain {
    biar h: teks = mysql_perintah(db, "SELECT id, nama FROM pengguna");
    biar baris: bulat = 0;
    selama baris < mysql_jumlah_baris(h) {
        cetak(mysql_ambil(h, baris, 0) + " - " + mysql_ambil(h, baris, 1));
        baris = baris + 1;
    }
    mysql_tutup(db);
}
```

## Fungsi

Koneksi:

- `mysql_sambung(inang: teks, port: bulat, pengguna: teks, sandi: teks): bulat`
  Menyambung + autentikasi (`mysql_native_password`). Mengembalikan handle soket, atau `-1` bila gagal.
- `mysql_gunakan(soket: bulat, db: teks): bool` — pilih basis data aktif.
- `mysql_tutup(soket: bulat): kosong` — tutup koneksi.

Eksekusi:

- `mysql_perintah(soket: bulat, sql: teks): teks` — jalankan SQL, kembalikan respons mentah (untuk SELECT, ini "hasil" yang dibaca fungsi di bawah).
- `mysql_galat(resp: teks): teks` — pesan galat dari respons (kosong bila bukan galat).

Membaca hasil SELECT:

- `mysql_jumlah_kolom(hasil: teks): bulat`
- `mysql_jumlah_baris(hasil: teks): bulat`
- `mysql_ambil(hasil: teks, baris: bulat, kolom: bulat): teks` — nilai sel (0-indeks); `NULL` menjadi `""`.
- `mysql_nilai(hasil: teks): teks` — pintasan untuk sel pertama (query skalar).

Nama fungsi memakai gaya `snake_case` dengan awalan `mysql_`.

## Prepared statement (berparameter, anti-injeksi)

Placeholder `?`; nilai dikirim terpisah lewat protokol biner (COM_STMT_PREPARE/EXECUTE).

```tenun
biar h: teks = mysql_siapkan_jalan(db, "SELECT id, nama FROM pengguna WHERE umur >= ?", ["18"]);
biar i: bulat = 0;
selama i < mysql_bin_baris(h) {
    cetak(mysql_bin_ambil(h, i, 1));
    i = i + 1;
}
```

- `mysql_siapkan_jalan(soket, sql, params: []teks): teks` — prepare + execute, kembalikan hasil biner.
- `mysql_bin_kolom(hasil): bulat`, `mysql_bin_baris(hasil): bulat`, `mysql_bin_ambil(hasil, baris, kolom): teks`.

Tipe didukung: integer (TINY/SHORT/LONG/LONGLONG) dan teks/varchar/blob/decimal. FLOAT/DOUBLE belum.

## Struktur

```
modul-mysql/
  tenun.json         manifest (nama, versi, deskripsi, berkas, butuh)
  src/
    mysql.tenun      koneksi, auth, query teks
    lenenc.tenun     helper integer length-encoded
    prepared.tenun   prepared statement (protokol biner)
  contoh/
    contoh.tenun     contoh pemakaian
  .github/workflows/ci.yml
  README.md
```

Entry modul ditunjuk oleh `berkas` di `tenun.json` (`src/mysql.tenun`).

## Catatan

- Saat ini `mysqlPerintah` mengembalikan paket respons mentah; parser hasil (baris/kolom) menyusul.
- Mendukung sandi kosong maupun `mysql_native_password`.

## Lisensi

MIT.
