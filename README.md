# E-ABSENSI SMPBP PAITON — PWA Wrapper

Wrapper PWA untuk aplikasi absensi berbasis Google Apps Script.
Dapat di-host gratis di **GitHub Pages**.

## 📁 Struktur File

```
e-absensi-pwa/
├── index.html              ← halaman wrapper (iframe Apps Script + SW registration)
├── manifest.json           ← manifest PWA
├── sw.js                   ← service worker (caching offline)
├── README.md               ← file ini
└── icons/
    ├── source.png          ← ikon sumber (referensi)
    ├── icon-192.png        ← ikon 192x192 (any)
    ├── icon-512.png        ← ikon 512x512 (any)
    ├── icon-192-maskable.png   ← ikon maskable 192x192
    ├── icon-512-maskable.png   ← ikon maskable 512x512
    ├── apple-touch-icon.png   ← 180x180 untuk iOS
    ├── favicon-32.png
    ├── favicon-16.png
    └── favicon.ico         ← favicon multi-size
```

## 🚀 Cara Deploy ke GitHub Pages

### Langkah 1 — Buat Repository Baru

1. Buka https://github.com/new
2. Repository name: `e-absensi-pwa` (atau apa punama)
3. **Public** (wajib untuk GitHub Pages gratis)
4. Centang **Add a README file**
5. Klik **Create repository**

### Langkah 2 — Upload Semua File

#### Opsi A: Upload via Web (paling mudah)

1. Di repo, klik **Add file → Upload files**
2. Drag semua isi folder `e-absensi-pwa/` (termasuk subfolder `icons/`)
3. Commit changes

#### Opsi B: Upload via Git CLI

```bash
git clone https://github.com/USERNAME/e-absensi-pwa.git
cd e-absensi-pwa
# Salin semua file dari folder e-absensi-pwa ke sini
git add .
git commit -m "Initial PWA wrapper"
git push origin main
```

### Langkah 3 — Aktifkan GitHub Pages

1. Buka tab **Settings** di repo
2. Menu kiri → **Pages**
3. Section **Build and deployment**
   - **Source**: Deploy from a branch
   - **Branch**: `main` / `(root)` / **Save**
4. Tunggu 1-2 menit, lalu refresh halaman
5. URL Anda akan muncul, formatnya:

   ```
   https://USERNAME.github.io/e-absensi-pwa/
   ```

### Langkah 4 — Verifikasi

1. Buka URL GitHub Pages di browser
2. Buka DevTools → **Application** tab
   - **Manifest**: harus terdeteksi
   - **Service Workers**: status "activated and is running"
3. Test install: di Chrome Android, menu → **Install app** / **Add to Home screen**
4. Test offline: DevTools → **Network** → **Offline** → reload — wrapper tetap muncul (Apps Script butuh online, tapi shell PWA tetap tampil)

## ⚙️ Konfigurasi

### Ganti URL Apps Script

Jika URL Apps Script berubah, edit **`index.html`** baris:

```html
<iframe
  ...
  src="https://script.google.com/macros/s/AKfycbx.../exec"
  ...
></iframe>
```

Dan edit **`sw.js`** konstanta `APPS_SCRIPT_URL`:

```js
const APPS_SCRIPT_URL = "https://script.google.com/.../exec";
```

### Ganti Versi Cache

Setiap kali mengubah `sw.js` atau `index.html`, naikkan versi cache
agar browser menghapus cache lama:

```js
// di sw.js
const CACHE_VERSION = "v1.0.1";  // ← ubah ini
```

### Ganti Warna Tema

- `manifest.json` → `"theme_color"` dan `"background_color"`
- `index.html` → `<meta name="theme-color">` dan CSS `.splash`

Warna saat ini: `#0d47a1` (blue 900 Material).

## 🔍 Fitur PWA yang Sudah Aktif

| Fitur | Status |
|-------|--------|
| Installable (Add to Home Screen) | ✅ |
| Splash screen kustom | ✅ |
| Service worker dengan caching | ✅ |
| Bisa dibuka offline (shell only) | ✅ |
| Maskable icon (Android adaptive) | ✅ |
| Apple touch icon (iOS) | ✅ |
| Auto-update SW | ✅ |
| Install banner kustom | ✅ |
| Fallback UI bila iframe gagal | ✅ |

## ⚠️ Catatan Penting

1. **HTTPS wajib** — GitHub Pages sudah HTTPS by default ✅
2. **Service Worker butuh HTTPS atau localhost** — terpenuhi ✅
3. **Scope SW** = folder deploy. Jika app di `github.io/user/repo/`,
   `start_url: "./"` sudah benar.
4. **Apps Script iframe butuh internet** — PWA shell bisa offline,
   tapi isi iframe tetap perlu koneksi.
5. **Cache size** — Apps Script sering return halaman besar.
   Cache akan tumbuh seiring waktu. SW otomatis bersihkan cache lama
   saat versi naik.

## 🛠️ Troubleshooting

### SW tidak terdaftar
- Cek DevTools → Application → Service Workers
- Pastikan tidak ada SW lama yang masih jalan → **Unregister**
- Hard refresh: `Ctrl + Shift + R`

### Icon tidak muncul di install prompt
- Pastikan **kedua** icon 192 dan 512 ada di manifest
- Pastikan `purpose` benar (`any` dan `maskable`)

### "Add to Home screen" tidak muncul
- Chrome butuh **engagement criteria** (minimal interaksi)
- Pastikan SW **active** dan **manifest** valid
- Test di Chrome DevTools → Application → Manifest → **Generate** error report

### Iframe blank / error CORS
- Apps Script kadang reject embed. Pastikan `X-Frame-Options` tidak di-set
  di response Apps Script (default Google tidak masalah)
- Coba buka URL Apps Script langsung — harus load tanpa error

## 📞 Bantuan

Jika ada masalah, cek:
1. Console browser (F12 → Console)
2. DevTools → Application → Manifest (cek warning)
3. DevTools → Application → Service Workers (cek error log)

---
© E-ABSENSI SMPBP PAITON
