# modul-socketio

Socket.IO (Engine.IO v4) untuk bahasa [Tenun](https://github.com/TenunLang/Tenun) — realtime berbasis event di atas WebSocket. emit/on + broadcast. Ditulis dengan Tenun, butuh [modul-websocket](https://github.com/TenunLang/modul-websocket).

## Pasang

```
tenun add socketio
```

(otomatis menarik `websocket` sebagai dependensi)

## Contoh (chat broadcast)

```tenun
impor "socketio";

fungsi chat(soket: bulat, data: teks): kosong {
    io_siar("chat", data);                 // broadcast ke semua klien
}
fungsi sambut(soket: bulat, data: teks): kosong {
    io_emit_teks(soket, "sistem", "halo");  // ke satu klien
}

io_pada("koneksi", sambut);                 // event saat klien connect
io_pada("chat", chat);

fungsi koneksi(soket: bulat): kosong { io_tangani(soket); }
layaniSoket(3000);
```

Klien (socket.io-client v4, transport websocket):

```js
const s = io("http://localhost:3000", { transports: ["websocket"] });
s.on("chat", (m) => console.log(m));
s.emit("chat", "halo semua");
```

## Fungsi

- `io_pada(event, handler)` — daftar handler `fungsi(soket: bulat, data: teks): kosong`. Event khusus: `"koneksi"` (connect), `"putus"` (disconnect).
- `io_emit(soket, event, data)` — kirim ke satu klien (`data` = JSON mentah).
- `io_emit_teks(soket, event, pesan)` — kirim string (dikutip JSON otomatis).
- `io_siar(event, data)` / `io_siar_teks(event, pesan)` — broadcast ke semua klien.
- `io_tangani(soket)` — jalankan handshake + loop event (panggil dari `koneksi`).

## Protokol

Engine.IO v4 transport websocket: handshake OPEN (`0{...}`) → CONNECT (`40`) → event (`42["nama",data]`), ping/pong (`2`/`3`) otomatis. Kompatibel dengan `socket.io-client` v4 memakai `transports: ["websocket"]`.

## Catatan

- Broadcast (`io_siar`) memakai registry soket runtime + builtin `siarkan`. Pengiriman lintas-koneksi berjalan di Linux (target deploy). Di Windows (dev) ada keterbatasan afinitas thread-soket pada pengiriman lintas-koneksi.
- `ws://` (tanpa TLS); untuk `wss://` terminasi TLS di reverse proxy.
- Long-polling transport tidak didukung — klien harus pakai `transports: ["websocket"]`.

## Struktur

```
modul-socketio/
  tenun.json          (butuh: websocket)
  src/socketio.tenun
  contoh/contoh.tenun
```

## Lisensi

MIT.
