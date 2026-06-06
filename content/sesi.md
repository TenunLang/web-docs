# modul-sesi

Manajemen sesi server dan bantu cookie untuk bahasa [Tenun](https://github.com/TenunLang/Tenun). Data sesi disimpan persisten lewat penyimpanan kunci-nilai bawaan Tenun. Ditulis sepenuhnya dengan Tenun.

## Pasang

```
tenun add sesi
```

## Pakai

```tenun
impor "sesi";

// Login: buat sesi, tanam cookie.
biar id: teks = sesi_buat("{\"pengguna\":\"taqin\"}");
headerKan("Set-Cookie", sesi_set_cookie(id));

// Permintaan berikutnya: baca dari cookie.
biar id2: teks = sesi_dari_cookie();
kalau sesi_ada(id2) {
    cetak("halo " + sesi_field(id2, "pengguna"));
}

// Logout.
sesi_hapus(id2);
headerKan("Set-Cookie", sesi_hapus_cookie());
```

## Fungsi

- `sesi_buat(data: teks): teks` — buat sesi (data JSON), kembalikan ID acak.
- `sesi_ambil(id: teks): teks` — data JSON sesi (`""` bila tidak ada).
- `sesi_ada(id: teks): bool`
- `sesi_simpan(id: teks, data: teks): kosong` — perbarui data.
- `sesi_hapus(id: teks): kosong` — logout.
- `sesi_field(id: teks, kunci: teks): teks` — ambil satu field JSON dari data sesi.
- `sesi_set_cookie(id: teks): teks` — header Set-Cookie (HttpOnly, SameSite=Lax).
- `sesi_hapus_cookie(): teks` — Set-Cookie penghapus (Max-Age=0).
- `sesi_dari_cookie(): teks` — baca ID sesi dari cookie permintaan masuk.

### Lanjutan

- `sesi_mulai(): teks` — ambil ID dari cookie atau buat sesi kosong baru.
- `sesi_regenerasi(idLama): teks` — ganti ID sesi (cegah session fixation; panggil setelah login).
- `sesi_set_cookie_opsi(id, maxAgeDetik, secure): teks` — Set-Cookie dengan Max-Age & Secure.
- `sesi_flash_set(id, pesan)` / `sesi_flash_ambil(id)` — pesan flash sekali-pakai.

## Keamanan

- ID sesi 24 byte acak (base64url), aman dipakai di cookie/URL.
- Cookie default `HttpOnly` + `SameSite=Lax` (mitigasi XSS/CSRF dasar). Tambah `Secure` saat memakai HTTPS.

## Struktur

```
modul-sesi/
  tenun.json
  src/sesi.tenun
  contoh/contoh.tenun
```

## Lisensi

MIT.
