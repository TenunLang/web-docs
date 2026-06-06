# Contoh Kode Tenun

Cuplikan singkat per fitur. Semua jalan via `tenun run <file>`. Contoh lengkap di repo Tenun folder `examples/`.

## FizzBuzz

```tenun
untuk i dari 1 sampai 21 {
    kalau i % 15 == 0 { cetak("FizzBuzz"); }
    lain { kalau i % 3 == 0 { cetak("Fizz"); }
    lain { kalau i % 5 == 0 { cetak("Buzz"); }
    lain { cetak(keTeks(i)); } } }
}
```

## peta dinamis (objek JSON bersarang)

```tenun
biar user: peta = peta{
    "nama": "Taqin", "umur": 27,
    "hobi": ["tenun", "kode"],
    "alamat": peta{ "kota": "Solo" }
};
biar al: peta = user["alamat"];
cetak(al["kota"]);          // Solo
```

## First-class function + higher-order

```tenun
fungsi tambah(a: bulat, b: bulat): bulat { kembali a + b; }
fungsi terapkan(f: fungsi, x: bulat, y: bulat): dinamis { kembali f(x, y); }
cetak(keTeks(terapkan(tambah, 6, 7)));   // 13
```

## coba/tangkap (error handling)

```tenun
coba {
    biar x: bulat = 10 / 0;
} tangkap (e) {
    cetak("galat: " + e);
}
```

## cocok (switch)

```tenun
cocok hari {
    1 { cetak("Senin"); }
    2 { cetak("Selasa"); }
    lain { cetak("lainnya"); }
}
```

## for-each + statistik

```tenun
biar data: []bulat = [12, 7, 25, 3];
biar total: bulat = 0;
untuk x dari data { total += x; }
cetak(keTeks(total));
```

## bitwise + flag

```tenun
tetap BACA: bulat = 0b001;
tetap TULIS: bulat = 0b010;
biar izin: bulat = BACA | TULIS;
cetak(keTeks(izin & TULIS));   // 2
```

## map/filter/reduce (modul larik)

```tenun
impor "larik";
fungsi genap(x: bulat): bool { kembali x % 2 == 0; }
cetak(larik_saring([1, 2, 3, 4], genap));   // [2, 4]
```

## Server HTTP (modul web)

```tenun
impor "web";
fungsi beranda(): teks { kembali web_teks("Halo!"); }
fungsi user(): teks { kembali web_json("{\"id\":\"" + web_param("id") + "\"}"); }
web_get("/", beranda);
web_get("/user/:id", user);
fungsi tangani(m: teks, j: teks, b: teks): teks { kembali web_tangani(m, j, b); }
layani(8080);
```

## Aplikasi penuh

E-commerce MVC lengkap (web + orm + tampilan + auth + sesi + mail): lihat dokumen `contoh-toko` atau repo `github.com/TenunLang/toko-tenun`.
