# web-docs

Dokumentasi resmi [Tenun](https://github.com/TenunLang/Tenun) — sumber kebenaran berbentuk **markdown** di `content/`. Disajikan apa adanya (text/plain) agar ramah LLM/agen, dan menjadi sumber untuk [tenun-mcp](https://github.com/TenunLang/tenun-mcp) (MCP server untuk Claude Code).

## Isi

`content/*.md` — dokumen lengkap:

- `index`, `bahasa`, `grammar`, `builtin`
- modul: `web`, `rute`, `tampilan`, `json`, `websocket`, `socketio`, `orm`, `mysql`, `postgres`, `redis`, `auth`, `sesi`, `mail`
- `contoh-toko` — contoh e-commerce
- `daftar.json` — manifest dokumen

## Server (opsional)

Ditulis dengan Tenun (`modul-web`). Menyajikan markdown sebagai text/plain.

```
tenun add web
tenun run src/server.tenun      # http://localhost:8080
```

Endpoint:

- `GET /` — ringkasan (index.md)
- `GET /daftar` — daftar nama dokumen (JSON)
- `GET /<nama>` — isi dokumen markdown (mis. `/bahasa`, `/web`, `/orm`)

## Akses langsung (tanpa server)

File markdown bisa diambil langsung dari GitHub raw:

```
https://raw.githubusercontent.com/TenunLang/web-docs/main/content/bahasa.md
```

## Lisensi

MIT.
