# EasyKilo Laundry — Landing Page

Landing page produksi untuk EasyKilo Laundry (Bintaro, Tangerang Selatan).
Situs statis murni: **tanpa build step, tanpa dependencies, tanpa `node_modules`**.

## Kenapa statis, bukan Next.js?

Untuk landing page satu halaman + dua halaman legal, framework justru menambah beban tanpa manfaat nyata:

| | Situs ini | Next.js |
|---|---|---|
| Waktu deploy | ~5 detik | ~40–60 detik (build) |
| JS dikirim ke browser | ~0 KB | ~90 KB+ |
| Perlu Node.js untuk edit | Tidak | Ya |
| Dependency yang bisa rusak | Tidak ada | Puluhan |

Mau edit? Buka file HTML, ubah, simpan, refresh browser. Selesai.

## Struktur

```
.
├── index.html              # Halaman utama
├── privacy-policy.html     # /privacy-policy
├── terms-of-service.html   # /terms-of-service
├── styles.css              # Seluruh styling (design tokens di :root)
├── assets/
│   ├── logo.webp           # Logo
│   ├── storefront.webp     # Foto toko
│   └── og-image.jpg        # Preview saat di-share (1200×630)
├── vercel.json             # Clean URLs, cache headers, security headers
├── sitemap.xml
└── robots.txt
```

## ⚠️ Wajib diganti sebelum go-live

Domain `easykilolaundry.com` dipakai sebagai placeholder. Setelah domain final
terhubung di Vercel, ganti di **5 file** ini:

```bash
grep -rl "easykilolaundry.com" . --exclude-dir=.git
```

Ganti sekaligus (ganti `domainkamu.com` dengan domain asli):

```bash
grep -rl "easykilolaundry.com" . --exclude-dir=.git | xargs sed -i '' 's/easykilolaundry\.com/domainkamu.com/g'
```

Kalau tidak diganti, preview WhatsApp/Facebook akan menarik gambar dari domain yang salah dan `canonical` URL-nya keliru di mata Google.

## Preview lokal

Tidak perlu server — cukup buka `index.html` di browser.

Kalau ingin URL bersih (`/privacy-policy` tanpa `.html`) sama seperti produksi:

```bash
python3 -m http.server 8000
```

Lalu buka http://localhost:8000

## Deploy ke Vercel

1. Buka [vercel.com/new](https://vercel.com/new), login pakai GitHub
2. Import repository `easykilo`
3. Framework Preset: **Other** — kosongkan Build Command dan Output Directory
4. Klik **Deploy**

Setiap `git push` ke `main` otomatis deploy ulang.

## Menghubungkan domain sendiri

1. Di dashboard Vercel: **Settings → Domains → Add**
2. Masukkan domain kamu
3. Di penyedia domain, tambahkan record yang Vercel tampilkan:
   - Root (`domainkamu.com`) → record `A` ke `76.76.21.21`
   - `www` → record `CNAME` ke `cname.vercel-dns.com`
4. Tunggu propagasi DNS (biasanya <1 jam). SSL otomatis dari Vercel.

Setelah aktif, jalankan langkah **"Wajib diganti"** di atas lalu push lagi.

## Cara edit yang sering dibutuhkan

**Ubah harga** — cari `plan__amount` di `index.html`
**Ubah nomor WhatsApp** — cari & ganti semua `6281994382070`
**Ubah warna** — edit `:root` di bagian atas `styles.css`
**Tambah area layanan** — duplikat satu `<li class="area">` di section `#area`
**Ganti foto toko** — timpa `assets/storefront.webp` (pertahankan nama file)

## Catatan

- Halaman legal disusun sebagai dasar yang wajar untuk usaha laundry, bukan
  nasihat hukum. Sesuaikan bila ada ketentuan khusus di usaha kamu.
- Gambar disimpan lokal di `assets/`, tidak lagi menumpang CDN Manus — situs
  tetap jalan meski layanan Manus dihentikan.
