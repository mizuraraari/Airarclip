# AI AUTO EDITOR — Web App (PWA)

Versi browser dari project Android AI AUTO EDITOR v1.2. Backend FastAPI lama dipertahankan dan ditambah endpoint one-click `/v1/web-auto-edit`. Frontend responsif bisa dipasang sebagai PWA di Android/desktop.

## Fitur
- Upload video dari HP/PC, preview, dan drag-drop.
- AI menganalisis audio/transkrip serta sampel frame untuk memilih highlight (opsional).
- Subtitle otomatis dari transkripsi (opsional; timing mengikuti segmen transkrip).
- Preset Auto, Cinematic, Energetic, Clean, Anime, Gaming, Vlog.
- Subtitle White Bold, Yellow Bold, Minimal, Pop Cyan, Compact.
- Smart Face Follow berbasis OpenCV Haar (best effort; bukan pengenalan identitas).
- Analisis beat audio (RMS onset heuristic) untuk memperkirakan BPM; dipakai sebagai petunjuk tempo musik instrumental sintetis sederhana (opsional; bukan musik generatif profesional atau katalog lagu).
- Render MP4 vertikal 1080x1920 dan download.

## Jalankan lokal
1. Install Python 3.11+, FFmpeg + FFprobe.
2. Dari folder `AutoVideoEditor`: `python -m venv .venv` lalu aktifkan environment.
3. `pip install -r backend/requirements.txt`
4. Set environment variable `OPENAI_API_KEY` dan `WEB_ACCESS_PASSWORD` (buat password panjang untuk akses website; jangan pernah masukkan API key ke frontend).
5. `uvicorn app.main:app --app-dir backend --host 0.0.0.0 --port 8000`
6. Buka `http://localhost:8000`.

## Deploy gratis (dengan batasan)
1. Buat repo GitHub private/public dan upload isi folder project ini.
2. Buat akun Render dan pilih **New → Blueprint**, hubungkan repo; Render membaca `render.yaml`. Alternatif: New Web Service → Docker, root directory `AutoVideoEditor`.
3. Tambahkan `OPENAI_API_KEY` dan `WEB_ACCESS_PASSWORD` sebagai secret environment variable di dashboard Render. Buat password website yang kuat; pengguna harus memasukkannya sebelum mengedit. Jangan commit file `.env` atau API key.
4. Deploy lalu buka URL `https://...onrender.com`. Endpoint `/health` memeriksa backend.

**Penting tentang gratis:** free hosting tidak menjadikan proses AI gratis. Render free web services dapat sleep saat idle, punya batas CPU/RAM/durasi, dan video processing berat bisa gagal atau lambat. Transkripsi/vision API OpenAI memerlukan saldo/penagihan sesuai tarif akun. Kuota, ukuran upload, dan kebijakan free tier penyedia dapat berubah. Untuk demo, gunakan video pendek (maksimal awal 40 MB) dan file kecil.

## Keamanan sebelum dibuka publik
- Versi awal memakai satu password bersama (`WEB_ACCESS_PASSWORD`), bukan akun individual; belum ada antrean pekerjaan, rate limiting, maupun kuota pengguna. Jangan publikasikan sebagai layanan umum tanpa autentikasi individual, pembatasan request, proteksi abuse, dan kebijakan privasi/retensi.
- File sementara dihapus setelah respons selesai; jika proses terputus/worker mati, OS/container dapat membersihkan saat restart. Jangan unggah video sensitif.
- Set `CORS_ORIGINS` ke origin yang spesifik bila frontend/API dipisah.
- Simpan `OPENAI_API_KEY` hanya di secret manager server.

## Batasan teknis yang perlu diketahui
- AI memilih highlight berdasarkan maksimal 10 sampel frame dan transkrip; hasil perlu diperiksa, tidak memahami setiap frame.
- Subtitle dibuat dari timestamp segmen transkripsi; bukan forced alignment kata-per-kata.
- Face follow menggunakan deteksi wajah Haar, dapat gagal pada profil, gelap, tertutup, atau wajah kecil.
- Preset adalah filter warna FFmpeg, bukan efek generatif. Musik adalah loop sintetis sederhana.
- Render dilakukan di server; koneksi upload dan batas CPU/RAM hosting sangat memengaruhi performa.

## Struktur
- `web/` — UI responsive, PWA manifest dan service worker.
- `backend/app/main.py` — API lama + web one-click workflow.
- `Dockerfile` — Docker build dari root project (web + backend).
- `render.yaml` — konfigurasi awal Render Blueprint.
