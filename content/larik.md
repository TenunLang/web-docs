# modul-larik

Utilitas fungsional untuk larik di bahasa [Tenun](https://github.com/TenunLang/Tenun): map / filter / reduce dan kawan-kawan. Ditulis murni dengan Tenun (memanfaatkan first-class function + for-each + tipe `dinamis`).

## Pasang

```
tenun add larik
```

## Pakai

```tenun
impor "larik";

fungsi kali2(x: bulat): bulat { kembali x * 2; }
fungsi genap(x: bulat): bool { kembali x % 2 == 0; }
fungsi tambah(a: bulat, b: bulat): bulat { kembali a + b; }

biar n: []bulat = [1, 2, 3, 4, 5];
cetak(larik_peta(n, kali2));        // [2, 4, 6, 8, 10]
cetak(larik_saring(n, genap));      // [2, 4]
cetak(larik_lipat(n, 0, tambah));   // 15
```

## Fungsi

- `larik_peta(arr, f)` — map: terapkan `f(x)` ke tiap elemen.
- `larik_saring(arr, f)` — filter: simpan elemen yang `f(x)` benar.
- `larik_lipat(arr, awal, f)` — reduce: `f(akk, x)` jadi satu nilai.
- `larik_ada(arr, f): bool` — ada elemen yang memenuhi `f`?
- `larik_hitung(arr, f): bulat` — jumlah elemen yang memenuhi `f`.
- `larik_temu(arr, f, bawaan)` — elemen pertama yang cocok, atau `bawaan`.
- `larik_balik(arr)` — balik urutan.

Hasil bertipe `[]dinamis` (bisa di-assign ke `[]T`). Handler `f` adalah nilai fungsi.

## Lisensi

MIT.
