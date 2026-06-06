# Builtin Tenun

Fungsi bawaan (tanpa impor). Tersedia di `tenun run` (VM/interp). Codegen native (`tenun build`) hanya mendukung subset matematika/teks/larik.

## Inti

- `cetak(x)` — tampilkan nilai apa pun + baris baru
- `panjang(larik): bulat` — jumlah elemen larik
- `argumen(): []teks` — argumen CLI (setelah path) dari `tenun run file arg...`

## Teks

- `panjangTeks(teks): bulat`
- `potong(teks, mulai: bulat, jumlah: bulat): teks` — substring
- `cari(teks, sub: teks): bulat` — indeks kemunculan, -1 bila tidak ada
- `ganti(teks, dari: teks, ke: teks): teks`
- `pisah(teks, pemisah: teks): []teks`
- `gabung(bagian: []teks, pemisah: teks): teks`
- `mulaiDengan(teks, awalan: teks): bool`
- `akhiriDengan(teks, akhiran: teks): bool`
- `keBulat(teks): bulat`
- `keTeks(bulat): teks`
- `keDesimal(teks): desimal`
- `pangkas(teks): teks` — buang spasi di ujung (trim)
- `keBesar(teks): teks` / `keKecil(teks): teks` — huruf besar/kecil (ASCII)
- `tipeKonten(namaFile: teks): teks` — MIME dari ekstensi

## Waktu & acak

- `waktu(): bulat` — unix timestamp (detik)
- `waktuMili(): bulat` — unix timestamp (milidetik)
- `tanggal(ts: bulat, offsetJam: bulat): teks` — format ts jadi "YYYY-MM-DD HH:MM:SS" pada offset zona waktu (jam). Mis. `tanggal(waktu(), 7)` = waktu WIB.
- `acakAngka(min: bulat, max: bulat): bulat` — bilangan acak [min, max)
- `acak(n: bulat): teks` — n byte acak (CSPRNG, lihat Kripto)

## Angka

- `akar(desimal): desimal`, `pangkat(desimal, desimal): desimal`, `mutlak(desimal): desimal`, `bulatkan(desimal): bulat`

## Larik

- `dorong(larik, item): larik` — tambah elemen (polimorfik, untuk tipe larik apa pun)

## Peta (map teks->teks)

- `petaPunya(m: peta, k: teks): bool`
- `petaKunci(m: peta): []teks`
- `petaHapus(m: peta, k: teks): kosong`

## Berkas

- `bacaFile(path: teks): teks`
- `tulisFile(path: teks, isi: teks): kosong`
- `adaFile(path: teks): bool`

## JSON (top-level)

- `jsonTeks(json: teks, kunci: teks): teks`
- `jsonAngka(json: teks, kunci: teks): bulat`
- `jsonBool(json: teks, kunci: teks): bool`

## HTTP klien

- `ambil(url: teks): teks` — HTTP GET (TLS otomatis)
- `httpKirim(metode, url, header, body: teks): teks` — HTTP umum (POST/PUT/DELETE/PATCH/GET), TLS otomatis untuk https. Header: baris "Nama: Nilai" dipisah `\n`.

## Server

- `layani(port: bulat): kosong` — server HTTP; butuh `fungsi tangani(metode, jalur, badan): teks`
- `layaniSoket(port: bulat): kosong` — server TCP mentah; butuh `fungsi koneksi(soket: bulat): kosong` (untuk WebSocket/protokol kustom)
- `statusKan(kode: bulat): kosong` — set status respons
- `headerKan(nama: teks, nilai: teks): kosong` — set header respons
- `headerMasuk(nama: teks): teks` — header permintaan
- `cookie(nama: teks): teks` — cookie permintaan
- `kueri(url: teks, kunci: teks): teks` — parse query string
- `form(body: teks, kunci: teks): teks` — parse form urlencoded
- `siarkan(data: teks): kosong` — broadcast byte ke semua koneksi layaniSoket aktif

## Soket TCP (klien)

- `sambung(inang: teks, port: bulat): bulat` — TCP connect, handle atau -1
- `kirim(soket: bulat, data: teks): kosong`
- `terima(soket: bulat, n: bulat): teks` — baca hingga n byte
- `terimaPasti(soket: bulat, n: bulat): teks` — baca tepat n byte
- `tutup(soket: bulat): kosong`

## Soket TLS (klien)

- `sambungAman(inang: teks, port: bulat): bulat` — TCP + handshake TLS
- `kirimAman(soket: bulat, data: teks): kosong`
- `terimaAman(soket: bulat, n: bulat): teks`
- `tutupAman(soket: bulat): kosong`

## KV store (persisten, file, aman multi-thread)

- `simpan(kunci: teks, nilai: teks): kosong`
- `muat(kunci: teks): teks`
- `hapus(kunci: teks): kosong`

## Kripto

- `sha256/sha1/md5(teks): teks` (hex), `hmacSha256(kunci, data: teks): teks` (hex)
- `sha256Raw/sha1Raw(teks): teks` (byte mentah), `hmacSha256Raw(kunci, data: teks): teks`
- `pbkdf2(sandi, salt: teks, iterasi: bulat): teks` (PBKDF2-HMAC-SHA256, byte mentah)
- `base64(teks): teks`, `dariBase64(teks): teks`
- `acak(n: bulat): teks` — n byte acak (CSPRNG)
- `xor(a: teks, b: teks): teks` — XOR byte (panjang sama)

## Biner

- `keByte(nilai: bulat, lebar: bulat, besar: bool): teks` — encode int ke byte (lebar 1-8, big/little endian)
- `bacaInt(data: teks, offset: bulat, lebar: bulat, besar: bool): bulat`
- `bacaFloat(data: teks, offset: bulat, lebar: bulat): teks` — decode IEEE-754 (lebar 4/8)
