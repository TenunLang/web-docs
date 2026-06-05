# modul-websocket

Server WebSocket (RFC 6455) untuk bahasa [Tenun](https://github.com/TenunLang/Tenun). Handshake + frame teks/biner/ping/close. Ditulis sepenuhnya dengan Tenun, di atas builtin `layaniSoket`.

## Pasang

```
tenun add websocket
```

## Contoh (echo server)

```tenun
impor "websocket";

fungsi koneksi(soket: bulat): kosong {
    kalau ws_terima_handshake(soket) {
        ws_kirim(soket, "halo");
        selama benar {
            biar pesan: teks = ws_baca(soket);
            kalau panjangTeks(pesan) == 0 { kembali; }   // ditutup
            ws_kirim(soket, "echo: " + pesan);
        }
    }
}

layaniSoket(8090);   // builtin: tiap koneksi -> fungsi koneksi(soket)
```

Browser:

```js
const ws = new WebSocket("ws://localhost:8090");
ws.onmessage = (e) => console.log(e.data);
ws.onopen = () => ws.send("tes");
```

## Fungsi

- `ws_terima_handshake(soket): bool` — baca permintaan upgrade, balas 101. `benar` bila sukses.
- `ws_baca(soket): teks` — baca satu pesan (data frame). `""` bila ditutup. Ping dibalas pong otomatis.
- `ws_kirim(soket, pesan): kosong` — kirim teks.
- `ws_kirim_biner(soket, data): kosong` — kirim biner.
- `ws_tutup(soket): kosong` — kirim frame close + tutup soket.

## Cara kerja

`layaniSoket(port)` (builtin Tenun) menjalankan server TCP, satu thread per koneksi, memanggil fungsi `koneksi(soket)`. Modul ini melakukan handshake (`Sec-WebSocket-Accept` = base64(SHA1(key + GUID))) lalu enkode/dekode frame. Frame klien yang ter-mask di-unmask via builtin `xor`.

## Catatan

- `ws://` (tanpa TLS). Untuk `wss://`, terminasi TLS di reverse proxy (Caddy/nginx) di depan server.
- Pesan teks kosong dianggap penanda tutup oleh `ws_baca` (sesuaikan bila perlu frame kosong).

## Struktur

```
modul-websocket/
  tenun.json
  src/websocket.tenun
  contoh/contoh.tenun
```

## Lisensi

MIT.
