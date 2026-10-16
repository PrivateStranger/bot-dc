# Discord Bot — Timer, Tanya AI, Lofi 24/7

Bot Discord berisi tiga fitur:

- `/timer` — join voice channel, beri beep setelah waktu habis, lalu keluar.
- `/tanya` — tanya jawab dengan model AI lokal via Ollama.
- `/lofi` — putar live stream lofi dari YouTube terus-menerus (auto-reconnect).

## Persyaratan

- Node.js 20 atau lebih baru (disarankan 22.12+).
- [Ollama](https://ollama.com) berjalan di komputer yang sama (untuk `/tanya`).
- Internet (untuk `/lofi`).

## Setup

1. Buat aplikasi + bot di https://discord.com/developers/applications, copy token.
2. Isi `.env` (minimal `BOT_TOKEN`):

   ```
   BOT_TOKEN=token_anda
   GUILD_ID=                       # opsional: id server Anda, command langsung muncul tanpa nunggu propagasi global
   OLLAMA_URL=http://localhost:11434
   OLLAMA_MODEL=gemma4
   LOFI_URL=https://www.youtube.com/watch?v=jfKfPfyJRdk
   ```

   Semua selain `BOT_TOKEN` punya default dan boleh dikosongkan.

3. Install dan jalankan:

   ```powershell
   npm install
   npm start
   ```

4. Invite bot dengan scope `bot` + `applications.commands`:

   ```
   https://discord.com/oauth2/authorize?client_id=APP_ID&permissions=3148800&scope=bot+applications.commands
   ```

   Ganti `APP_ID` dengan Application ID dari Developer Portal.

## Commands

| Command | Fungsi |
|---------|--------|
| `/timer minutes:<1+>` | Timer di voice channel Anda; beep lalu keluar saat selesai. |
| `/stop` | Batalkan timer yang berjalan. |
| `/tanya pertanyaan:<teks> [model]` | Tanya ke Ollama; maksimal 3 permintaan per menit per user. |
| `/lofi start [url] [volume:0-200]` | Putar lofi di voice channel Anda, putus otomatis disambung ulang. |
| `/lofi stop` | Hentikan lofi dan keluar dari voice channel. |

## Catatan

- Satu sesi voice per server: `/timer` dan `/lofi` tidak bisa jalan bersamaan.
- `/tanya` sekali jalan tanpa riwayat percakapan; model harus sudah di-pull (`ollama pull gemma4`).
- `/lofi` memakai yt-dlp + ffmpeg yang di-bundel lewat npm. Streaming konten YouTube ke Discord berpotensi melanggar ToS YouTube — risiko ditanggung pemakai, terutama di server publik.
- Kalau stream lofi mati atau URL-nya berubah, ganti `LOFI_URL` atau pakai opsi `url` saat `/lofi start`.
- Ctrl+C akan menghentikan bot dengan bersih (keluar dari voice channel).

## Deploy

Bot ini dirancang jalan di komputer sendiri. Kalau di-host: `/tanya` butuh akses ke Ollama (tunnel/proxy), dan `/lofi` sering diblokir YouTube untuk IP datacenter.
