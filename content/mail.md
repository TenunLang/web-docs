# modul-mail

Modul email lengkap untuk bahasa [Tenun](https://github.com/TenunLang/Tenun): kirim (SMTP/SMTPS + Resend API) dan baca (IMAP/IMAPS). Ditulis sepenuhnya dengan Tenun.

## Pasang

```
tenun add mail
```

## Fitur

- Kirim: teks polos, HTML, multipart/alternative (teks+HTML), lampiran (multipart/mixed, base64).
- Transport: SMTP polos, SMTPS (TLS 465), dan **Resend REST API** (HTTPS) tanpa perlu TLS SMTP.
- Baca: IMAP polos (143) dan **IMAPS (TLS 993, mis. Gmail)** — login, pilih kotak, jumlah, fetch, header, search, flag, hapus.
- Validasi alamat email + transkrip/log percakapan untuk debug.

## Kirim email

```tenun
impor "mail";

// Teks polos (SMTP polos, mis. Mailpit/relay lokal).
mail_kirim("127.0.0.1", 1025, "", "", "no@situs.id", "user@x.id", "Subjek", "Isi");

// HTML.
mail_kirim_html(inang, port, user, sandi, pengirim, tujuan, subjek, "<h1>Halo</h1>");

// Teks + HTML (klien pilih).
mail_kirim_lengkap(inang, port, user, sandi, pengirim, tujuan, subjek, "teks", "<b>html</b>");

// Dengan lampiran (html=benar untuk badan HTML).
mail_kirim_lampiran(inang, port, user, sandi, pengirim, tujuan, subjek, "<p>lihat lampiran</p>", benar, ["faktur.pdf", "logo.png"]);
```

### SMTPS (TLS langsung, port 465)

Sama seperti di atas, akhiri nama fungsi dengan `_aman`: `mail_kirim_aman`, `mail_kirim_html_aman`, `mail_kirim_lampiran_aman`. Mis. Gmail:

```tenun
mail_kirim_html_aman("smtp.gmail.com", 465, "akun@gmail.com", "app-password",
    "akun@gmail.com", "tujuan@x.id", "Subjek", "<h1>Halo</h1>");
```

> Catatan: STARTTLS (port 587) belum didukung — pakai port TLS-langsung 465. Sebagian jaringan memblokir 465/587 keluar; bila begitu, pakai Resend API (port 443).

### Resend API (HTTPS, direkomendasikan)

Tidak perlu TLS SMTP — lewat REST API. Dapatkan API key di resend.com.

```tenun
biar resp: teks = mail_resend_html("re_xxx", "onboarding@resend.dev", "kamu@email.id",
    "Halo", "<h1>Dari Tenun</h1>");
kalau mail_resend_sukses(resp) { cetak("terkirim"); }
```

`mail_resend(apikey, pengirim, tujuan, subjek, teks)` untuk versi teks. Respons JSON mentah dikembalikan (berisi `id` bila sukses).

## Baca email (IMAP / IMAPS)

```tenun
// Gmail butuh IMAPS (TLS 993) + app password.
biar s: bulat = imap_sambung_aman("imap.gmail.com", 993);
kalau imap_login(s, "akun@gmail.com", "app password") {
    biar n: bulat = imap_pilih(s, "INBOX");        // jumlah pesan
    biar pesan: teks = imap_ambil_header(s, n);    // header pesan terakhir
    cetak(imap_header(pesan, "From"));
    cetak(imap_header(pesan, "Subject"));
}
imap_keluar(s);
```

Server tanpa TLS: pakai `imap_sambung(inang, 143)`.

Fungsi IMAP: `imap_sambung`, `imap_sambung_aman`, `imap_login`, `imap_pilih`, `imap_ambil`, `imap_ambil_header`, `imap_header`, `imap_cari`, `imap_tandai_dibaca`, `imap_hapus`, `imap_keluar`.

## Utilitas

- `mail_validasi(alamat: teks): bool` — cek format email.
- `mail_log(): teks` — transkrip percakapan terakhir (debug/observabilitas).

## Struktur

```
modul-mail/
  tenun.json
  src/
    mail.tenun     entry (impor util/mime/smtp/imap/resend)
    util.tenun     validasi + transkrip/log
    mime.tenun     badan MIME: teks/HTML/multipart/lampiran
    smtp.tenun     kirim via SMTP/SMTPS
    imap.tenun     baca via IMAP/IMAPS
    resend.tenun   kirim via Resend REST API
  contoh/contoh.tenun
```

## Catatan TLS

IMAPS/SMTPS/Resend memakai builtin TLS Tenun (`sambungAman`/`httpKirim`, std.crypto.tls + std.http.Client). STARTTLS (upgrade dari polos) belum didukung; gunakan port TLS-langsung (465/993) atau Resend API.

## Lisensi

MIT.
