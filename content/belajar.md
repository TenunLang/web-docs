# modul belajar — pembelajaran mesin

Pustaka machine learning murni Tenun. Aljabar linear, regresi, jaringan saraf (MLP + backprop), k-NN, k-means. Berjalan di VM (`tenun run`). Tanpa ketergantungan eksternal.

```
tenun add belajar
```

```tenun
impor "belajar";
```

## MLP belajar XOR

```tenun
impor "belajar";

biar X: [][]desimal = [[0.0,0.0], [0.0,1.0], [1.0,0.0], [1.0,1.0]];
biar Y: [][]desimal = [[0.0], [1.0], [1.0], [0.0]];

biar lapisan: []bulat = [2, 6, 1];
biar bobot: [][][]desimal = jar_bobot_baru(lapisan, 1.0);
biar bias: [][]desimal = jar_bias_baru(lapisan);

jar_latih(bobot, bias, X, Y, 4000, 0.5);

untuk i dari 0 sampai 4 {
    cetak(jar_prediksi(bobot, bias, X[i])[0]);   // ~0, ~1, ~1, ~0
}
```

## Regresi linear

```tenun
biar model: []desimal = reg_model_baru(2);
reg_linear_latih(model, X, y, 2000, 0.02);
cetak(reg_linear_prediksi(model, [5.0, 5.0]));
```

## Regresi logistik (klasifikasi biner)

```tenun
biar model: []desimal = reg_model_baru(2);
reg_logistik_latih(model, X, y, 3000, 0.1);   // y berisi 0.0 / 1.0
cetak(reg_logistik_prediksi(model, x));        // peluang [0,1]
```

## Simpan & muat model

```tenun
jar_latih(bobot, bias, X, Y, 3000, 0.5);
jar_simpan(bobot, bias, "model.txt");                          // latih sekali

biar bobot: [][][]desimal = jar_muat_bobot("model.txt");        // muat ulang
biar bias: [][]desimal = jar_muat_bias("model.txt");
cetak(stat_argmaks(jar_prediksi(bobot, bias, contoh)));
```

## OCR digit

`data_dari_pola` mengubah pola piksel jadi masukan classifier; muat model terlatih untuk mengenali.

```tenun
biar tiga: []desimal = data_dari_pola([
    "####.", "....#", "....#", ".###.", "....#", "....#", "####."
]);
cetak(stat_argmaks(jar_prediksi(bobot, bias, tiga)));   // -> 3
```

Lihat `examples/ocr_latih.tenun` & `examples/ocr_kenali.tenun`.

## k-NN

```tenun
cetak(knn_klasifikasi(X_latih, y_latih, x, 3));   // suara mayoritas 3 tetangga
```

## k-means

```tenun
biar pusat: [][]desimal = kmeans(X, 2, 10);        // 2 klaster, 10 iterasi
biar tugas: []desimal = klaster_tugas(pusat, X);   // label klaster tiap titik
```

## Daftar fungsi

- **Aljabar linear**: `vek_nol`, `vek_isi`, `vek_acak`, `mat_nol`, `mat_acak`, `vek_dot`, `vek_tambah`, `vek_kurang`, `vek_skalar`, `vek_hadamard`, `vek_jumlah`, `mat_kali_vek`, `mat_kali`, `mat_transpos`
- **Aktivasi**: `akt_sigmoid`, `akt_relu`, `akt_tanh`, `akt_softmax` (+ turunan `akt_d_*`)
- **Rugi**: `rugi_mse`, `rugi_mse_grad`, `rugi_silang_biner`, `rugi_silang`
- **Jaringan saraf**: `jar_bobot_baru`, `jar_bias_baru`, `jar_prediksi`, `jar_latih_satu`, `jar_latih`
- **Regresi**: `reg_model_baru`, `reg_linear_prediksi`, `reg_linear_latih`, `reg_logistik_prediksi`, `reg_logistik_latih`
- **k-NN / k-means**: `knn_klasifikasi`, `kmeans`, `klaster_terdekat`, `klaster_tugas`
- **Statistik**: `stat_rata`, `stat_varian`, `stat_stdev`, `stat_min`, `stat_maks`, `stat_argmaks`, `stat_normalisasi`, `stat_minmax`, `stat_akurasi`
- **Data**: `data_satu_panas`, `data_jarak`, `data_acak_urut`, `data_batas_latih`

## Catatan

- Aktivasi MLP: sigmoid tiap lapisan, rugi MSE.
- Larik bertipe rujukan -> fungsi latih memperbarui bobot di tempat.
- Backend native (`tenun build`) belum mendukung builtin matematika; pakai `tenun run`.
