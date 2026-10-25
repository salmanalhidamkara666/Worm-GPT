# Worm AI - OpenRouter Gemma Model (Vercel)

Ringkasan: Backend serverless yang menggunakan OpenRouter API dengan model Gemma gratis (`google/gemma-4-26b-a4b-it:free`). API key tidak terlihat di sisi client. Endpoint serverless sudah ditambahkan di `api/chat.js`.

## Model: Google Gemma 4 26B (Gratis via OpenRouter)

- **Model ID**: `google/gemma-4-26b-a4b-it:free`
- **Provider**: OpenRouter.ai
- **Cost**: Gratis
- **Persona**: Worm AI - asisten yang membantu dengan singkat, jelas, dan sopan

Langkah cepat untuk deploy di Vercel (GitHub):

1. Push repo ke GitHub dan hubungkan repository ke Vercel.
2. Di dashboard Vercel, buka project -> Settings -> Environment Variables.
3. Tambahkan variabel environment yang diperlukan (Production/Preview/Development sesuai kebutuhan):
   - `GEMINI_API_KEY` atau `GEMINI_API_KEYS`: API key Gemini; Gemini digunakan lebih dulu.
   - `OPENROUTER_API_KEY` atau `OPENROUTER_API_KEYS`: API key OpenRouter opsional sebagai fallback.
   - `ADMIN_USER` dan `ADMIN_PASS`: kredensial login admin.
   - `SESSION_SECRET`: secret acak yang panjang untuk menandatangani sesi.
   - `GEMINI_MODEL`, `OPENROUTER_MODEL`, dan `SITE_URL` bersifat opsional.
   - Lihat `env.example` untuk daftar variabel opsional lainnya.

4. Commit dan push perubahan. Vercel otomatis akan deploy dari branch yang terhubung.

Pengujian lokal:

1. Salin `env.example` menjadi `.env`, lalu isi `GEMINI_API_KEY` dan kredensial login (`ADMIN_USER`/`ADMIN_PASS`). Untuk menggunakan OpenRouter sebagai fallback, isi juga `OPENROUTER_API_KEY`.
2. Jalankan dev-server:

```bash
node dev-server.js
```

3. Buka di browser:
   - Chat Interface: `http://127.0.0.1:3007/test-chat.html`
   - API Endpoint: `POST http://127.0.0.1:3007/api/chat`

4. Test API dengan curl:

```bash
curl -X POST http://127.0.0.1:3007/api/chat \
  -H "Content-Type: application/json" \
  -d '{"contents":[{"role":"user","parts":[{"text":"Halo, siapa nama kamu?"}]}]}'
```

Catatan keamanan:

- Jangan menyimpan API key di file klien (seperti `index.html`).
- Pastikan semua API key hanya diset di environment server (`.env` lokal atau environment Vercel), bukan di kode sumber atau file klien.
- File `.env` sudah di-ignore oleh `.gitignore` untuk mencegah leak credential.
- Model dan provider yang tersedia bergantung pada konfigurasi API key dan model.

## Fitur Tambahan

### Chat Test Interface

- File: `test-chat.html`
- Interface testing sederhana tanpa perlu login
- Bisa di-akses di `http://127.0.0.1:3007/test-chat.html` saat development

### Dev Server

- File: `dev-server.js`
- Node.js server untuk development lokal
- Melayani static files (CSS, JS, HTML)
- Cascading port allocation (3006, 3007, 3008, dst)
- CORS support enabled
- Format conversion Gemini ↔ OpenRouter

### Frontend

- Login page: `login.html`
- Chat interface: `index.html`
- Database: PostgreSQL via Neon
- Authentication: Client-side (untuk demo)
