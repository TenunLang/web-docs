# modul-redis

Klien Redis untuk bahasa [Tenun](https://github.com/TenunLang/Tenun), ditulis sepenuhnya dengan Tenun. Memakai protokol RESP.

## Pasang

```
tenun add redis
```

## Pakai

```tenun
impor "redis";

biar r: bulat = redis_sambung("127.0.0.1", 6379);
redis_perintah(r, "SET nama Budi");
cetak(redis_perintah(r, "GET nama"));     // Budi
cetak(redis_perintah(r, "INCR hitung"));  // 1
redis_tutup(r);
```

## Fungsi

- `redis_sambung(inang: teks, port: bulat): bulat` — buka koneksi.
- `redis_tutup(soket: bulat): kosong`
- `redis_perintah(soket: bulat, cmd: teks): teks` — jalankan perintah (dipisah spasi), kembalikan balasan sebagai teks.
- `redis_jalan(soket: bulat, args: []teks): teks` — versi argumen larik (aman untuk nilai berisi spasi).

Balasan RESP dipetakan ke teks: simple string / integer / bulk apa adanya, `nil` menjadi `""`, array digabung dengan baris baru (`\n`).

## Struktur

```
modul-redis/
  tenun.json
  src/redis.tenun
  contoh/contoh.tenun
  .github/workflows/ci.yml
```

## Catatan

- `terima` membaca dalam satu panggilan; untuk balasan sangat besar mungkin perlu pembacaan bertahap (menyusul).

## Lisensi

MIT.
