# modul citra — pemrosesan gambar

Muat gambar mentah (PGM/PPM), pra-pemrosesan, dan segmentasi. Dipadu `modul belajar` jadi pipeline OCR end-to-end. Murni Tenun, jalan di VM.

```
tenun add citra
```

```tenun
impor "citra";
```

## Format

- **PNG** 8-bit via `citra_png` (builtin `bacaGambar`: zlib inflate + unfilter).
- **PGM** (P2) / **PPM** (P3) via `citra_muat_pgm` / `citra_muat_ppm`.
- JPG belum didukung -> konversi ke PNG: `magick ktp.jpg ktp.png`.

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

Pipeline: PNG -> grayscale -> balik -> ambang -> segmentasi kolom -> bbox per glyph -> resize 12x16 -> MLP (modul belajar). Model dilatih pada dataset digit cetak **multi-font** (19 font Windows). Tenun = engine: latih sekali, muat, inferensi.

```
$ tenun run examples/ocr_baca.tenun nik.png
nik.png
terbaca: 3674072257025006
```

Dataset terpercaya (MNIST) / model eksternal: latih di luar, ekspor ke format model Tenun (`tools/latih_mnist.py`), lalu `jar_muat_*`. Untuk teks CETAK seperti KTP, korpus multi-font lebih cocok daripada MNIST (tulisan tangan).

## Fungsi

- **Muat**: `citra_muat_pgm`, `citra_muat_ppm`, `citra_simpan_pgm`
- **Akses**: `citra_lebar`, `citra_tinggi`, `citra_piksel`, `citra_set`, `citra_baru`, `citra_ke_vektor`, `citra_tampil`
- **Proses**: `citra_ambang`, `citra_ambang_auto`, `citra_balik`, `citra_ubah_ukuran`, `citra_potong`
- **Segmentasi**: `citra_proyeksi_x`, `citra_proyeksi_y`, `segmen_kolom`, `segmen_baris`
