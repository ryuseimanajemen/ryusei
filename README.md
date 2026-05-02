# Ryusei CMS — Fixed v3

## Perubahan & Perbaikan

### 1. ✅ Modal Pop-up Pendaftaran Berhasil
- Setelah submit form pendaftaran, akan muncul **modal/pop-up** besar dengan:
  - Ucapan selamat dan nama pendaftar
  - Kode unik yang besar dan jelas
  - **Tombol "Salin Kode"** untuk copy ke clipboard
  - Instruksi untuk screenshot dan cek status berkala
  - Tombol "Cek Status" yang langsung scroll ke form cek status

### 2. ✅ Dashboard Admin Real-time
- Semua counter dashboard (Peserta, Event, Juri, Pending) kini **real-time** menggunakan `.on()` bukan `.once()`
- Update otomatis tanpa perlu reload halaman
- Jumlah peserta per event pada Event Aktif juga real-time

### 3. ✅ Panel Juri - Permission Denied Diperbaiki
- **Firebase Rules** diupdate: node `/judges` kini bisa dibaca tanpa auth (`.read: true`)
- Juri tidak perlu Firebase Authentication untuk login — hanya perlu Judge ID + password
- **WAJIB UPDATE RULES** di Firebase Console:
  1. Buka Firebase Console → Realtime Database → Rules
  2. Copy isi file `firebase.rules.json` dan paste, lalu Publish

### 4. ✅ Hero Icon/Gambar Animasi Bisa Diganti
- Di **Landing Page CMS** admin, sekarang ada:
  - Upload gambar untuk hero icon (max 2MB, disimpan sebagai base64 di Firebase)
  - Field URL gambar eksternal alternatif
  - Field teks icon fallback (default: 竜)
- Jika tidak ada gambar → tampil favicon `/ryusei/assets/fav/apple-touch-icon.png`
- Jika favicon gagal load → tampil teks/kanji default

### 5. ✅ Upload Gambar Event
- Di form Tambah/Edit Event, kini ada tombol upload gambar
- Gambar disimpan sebagai base64 di Firebase (max 2MB)
- Jika lebih dari 2MB, gunakan URL external

### 6. ✅ Favicon di Semua Halaman
- Semua halaman (index, admin, judge) kini menggunakan favicon dari `/ryusei/assets/fav/`

---

## LANGKAH WAJIB: Update Firebase Rules

Paste ini ke Firebase Console → Realtime Database → Rules:

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "events": { ".read": true, ".write": "auth != null" },
    "registrations": {
      ".read": "auth != null",
      ".write": true,
      "$regId": { ".read": true }
    },
    "judges": {
      ".read": true,
      ".write": "auth != null"
    },
    "scores": { ".read": true, ".write": true },
    "cms": { ".read": true, ".write": "auth != null" },
    "stats": { ".read": true, ".write": "auth != null" }
  }
}
```

## Struktur File
```
ryusei-cms/
├── index.html          → Landing page + form pendaftaran
├── admin/
│   └── index.html      → Dashboard admin
├── judge/
│   └── index.html      → Panel juri
├── css/
│   └── tailwind.min.css
├── assets/
│   └── fav/            → Favicon files (letakkan di sini)
│       ├── apple-touch-icon.png
│       ├── favicon-32x32.png
│       ├── favicon-16x16.png
│       └── site.webmanifest
└── firebase.rules.json → Rules Firebase (update manual)
```
