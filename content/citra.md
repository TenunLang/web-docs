# modul citra — pemrosesan gambar

Muat gambar mentah (PGM/PPM), pra-pemrosesan, dan segmentasi. Dipadu `modul belajar` jadi pipeline OCR end-to-end. Murni Tenun, jalan di VM.

```
tenun add citra
```

```tenun
impor "citra";
```

## Format

Memuat **PGM** (P2 grayscale) & **PPM** (P3 RGB). JPG/PNG dikonversi dulu: `magick ktp.jpg ktp.pgm`. Decode JPG/PNG belum ada di compiler.

## Representasi

Citra = `[]desimal`: `c[0]`=lebar, `c[1]`=tinggi, `c[2..]`=piksel 0..1 row-major.

## Pipeline dasar

```tenun
biar g: []desimal = citra_muat_pgm("angka.pgm");
biar b: []desimal = citra_ambang(g, 0.5);       // binarisasi
citra_tampil(b);                                 // pratinjau ASCII
biar kotak: []desimal = segmen_kolom(b);         // batas-x tiap karakter
biar kecil: []desimal = citra_ubah_ukuran(citra_potong(b, 0, 0, 5, citra_tinggi(b)), 5, 7);
biar vektor: []desimal = citra_ke_vektor(kecil); // -> masukan classifier
```

## OCR end-to-end

`examples/ocr_gambar.tenun` (citra + belajar): muat PGM digit -> segmentasi -> resize 7x5 -> MLP.

```
$ tenun run examples/ocr_gambar.tenun
teks terbaca:
357
```

## Fungsi

- **Muat**: `citra_muat_pgm`, `citra_muat_ppm`, `citra_simpan_pgm`
- **Akses**: `citra_lebar`, `citra_tinggi`, `citra_piksel`, `citra_set`, `citra_baru`, `citra_ke_vektor`, `citra_tampil`
- **Proses**: `citra_ambang`, `citra_ambang_auto`, `citra_balik`, `citra_ubah_ukuran`, `citra_potong`
- **Segmentasi**: `citra_proyeksi_x`, `citra_proyeksi_y`, `segmen_kolom`, `segmen_baris`
