# Ryusei Event Management System

Sistem manajemen event lengkap untuk Ryusei dengan fitur publik, admin, juri, dan dokumentasi fitur. Dibangun menggunakan HTML/CSS/JavaScript dengan Firebase Realtime Database.

## 🚀 Fitur Utama

### 👥 Halaman Publik (`index.html`)
- **Landing Page**: Hero section dengan gambar/icon animasi yang dapat dikustomisasi
- **Daftar Event**: Tampilan event aktif dengan detail lengkap
- **Pendaftaran Peserta**: Form pendaftaran dengan validasi real-time
- **Cek Status**: Verifikasi status pendaftaran dengan kode unik
- **Leaderboard**: Tampilan hasil kompetisi (hanya jika dipublikasikan admin)
- **Terms & Fitur**: Halaman informasi `tos.html` dan `fitur.html` untuk dokumentasi publik
- **Tema Dark/Light**: Toggle tema dengan penyimpanan lokal
- **Responsive Design**: Optimal di desktop dan mobile

### 🛡️ Dashboard Admin (`admin/index.html`)
- **Role-Based Access Control**:
  - **Super Admin**: Akses penuh ke semua fitur (Firebase config, CMS, Settings)
  - **Staff Admin**: Akses terbatas (event, peserta, juri, skor)
- **Dashboard Real-time**: Counter peserta, event, juri, dan pending approval
- **Manajemen Event**: Tambah/edit/hapus event dengan upload gambar
- **Manajemen Peserta**: Approve/reject pendaftaran, lihat detail
- **Manajemen Juri**: Buat undangan juri dengan URL yang benar
- **Leaderboard Admin**: Lihat skor real-time dengan kontrol publikasi
- **CMS Landing Page**: Edit hero image, teks, dan konten landing
- **Settings**: Konfigurasi sistem dan manajemen admin
- **Firebase Config**: Setup konfigurasi Firebase (hanya super admin)

### ⚖️ Panel Juri (`judge/index.html`)
- **Login Juri**: Autentikasi dengan Judge ID dan password
- **Penilaian Peserta**: Form scoring dengan 5 kriteria (Interactivity, Immersion, Creativity, Improvisation, Costume)
- **Leaderboard Juri**: Lihat hasil penilaian real-time
- **Navigasi Event**: Pilih event yang akan dinilai
- **Responsive UI**: Interface yang mudah digunakan

## 🔧 Fitur Teknis

### Kontrol Publikasi Leaderboard
- Admin dapat mengontrol kapan leaderboard ditampilkan ke publik
- Tombol "Publikasikan Hasil" / "Tarik Publikasi" di dashboard admin
- Pesan "Leaderboard belum dipublikasikan" jika belum diaktifkan

### Role-Based Security
- Peran super admin dan staff dikelola melalui Firebase settings
- Staff admin dapat dibuat melalui undangan token dari admin utama
- Akses Firebase config dan CMS dibatasi hanya untuk super admin

### Real-time Updates
- Semua data update otomatis tanpa reload halaman
- Listener Firebase untuk perubahan real-time
- Cleanup listener untuk mencegah memory leak

### Firebase Integration
- Realtime Database untuk penyimpanan data
- Authentication untuk admin dan juri
- Rules security yang ketat
- Base64 image storage untuk gambar event dan hero

### UI/UX Features
- Tailwind CSS untuk styling modern
- Animasi dan transisi smooth
- Modal pop-up untuk konfirmasi dan detail
- Toast notifications untuk feedback user
- Loading states dan error handling

## 📁 Struktur File

```
ryusei/
├── index.html              # Landing page publik
├── admin/
│   └── index.html          # Dashboard admin
├── judge/
│   └── index.html          # Panel juri
├── css/
│   └── tailwind.min.css    # Styling framework
├── assets/
│   └── fav/                # Favicon files
│       ├── apple-touch-icon.png
│       ├── favicon-32x32.png
│       ├── favicon-16x16.png
│       └── site.webmanifest
├── firebase-config.json    # Template config Firebase
├── firebase.rules.json     # Firebase security rules
├── fitur.html              # Halaman fitur publik
├── tos.html                # Halaman terms of service
└── README.md               # Dokumentasi ini
```

## 🚀 Instalasi & Setup

### 1. Clone Repository
```bash
git clone https://github.com/ryuseimanajemen/ryusei.git
cd ryusei
```

### 2. Setup Firebase
1. Buat project baru di [Firebase Console](https://console.firebase.google.com)
2. Enable Realtime Database
3. Copy konfigurasi Firebase ke `firebase-config.json`
4. Update Firebase Rules di Console dengan isi `firebase.rules.json`

### 3. Deploy
Upload semua file ke web hosting atau gunakan local server:
```bash
python -m http.server 8000
# Akses di http://localhost:8000
```

### 4. Konfigurasi Awal
1. Buka `admin/index.html`
2. Klik ikon Ryusei 5x untuk buka modal Firebase config
3. Upload atau paste `firebase-config.json`
4. Login sebagai super admin pertama

## 🔐 Firebase Rules (WAJIB Update)

Paste ke Firebase Console → Realtime Database → Rules:

```json
{
  "rules": {
    ".read": false,
    ".write": false,
    "events": {
      ".read": true,
      ".write": "auth != null",
      "$eventId": {
        "published": { ".write": "auth != null" }
      }
    },
    "registrations": {
      ".read": "auth != null",
      ".write": true,
      "$regId": { ".read": true }
    },
    "judges": {
      ".read": true,
      ".write": "auth != null"
    },
    "scores": {
      ".read": true,
      ".write": true
    },
    "cms": {
      ".read": true,
      ".write": "auth != null"
    },
    "settings": {
      ".read": "auth != null",
      ".write": "auth != null"
    },
    "stats": {
      ".read": true,
      ".write": "auth != null"
    }
  }
}
```

## 🎯 Cara Penggunaan

### Untuk Admin
1. Login ke `admin/index.html`
2. Setup Firebase config jika belum
3. Buat event baru di menu Events
4. Kelola pendaftaran di menu Participants
5. Undang juri di menu Judges
6. Lihat dan publikasikan skor di menu Scores

### Untuk Juri
1. Buka link undangan dari admin
2. Login dengan Judge ID dan password
3. Pilih event yang akan dinilai
4. Berikan skor untuk setiap peserta

### Untuk Peserta
1. Buka halaman utama
2. Pilih event dan daftar
3. Simpan kode unik untuk cek status
4. Pantau leaderboard setelah dipublikasikan

## 🔄 Changelog Terbaru

### v4.0 - Role-Based Security & Publication Control
- ✅ Role-based admin access (super admin vs staff)
- ✅ Leaderboard publication control
- ✅ Fixed judge invite URL generation
- ✅ Enhanced Firebase config security
- ✅ Real-time publication status
- ✅ Improved admin user management
- ✅ Public documentation pages: `fitur.html` dan `tos.html`

### v3.0 - Real-time Dashboard & Judge Permissions
- ✅ Real-time admin dashboard counters
- ✅ Fixed judge login permissions
- ✅ Hero image upload in CMS
- ✅ Event image upload
- ✅ Favicon implementation

### v2.0 - Modal Success & Real-time Updates
- ✅ Registration success modal
- ✅ Real-time participant counts
- ✅ Improved form validation

## 🐛 Troubleshooting

### Judge tidak bisa login
- Pastikan Firebase Rules sudah diupdate
- Cek Judge ID dan password benar

### Leaderboard tidak muncul
- Pastikan admin sudah "Publikasikan Hasil"
- Cek koneksi internet dan Firebase config

### Gambar tidak upload
- Pastikan ukuran < 2MB
- Gunakan format JPG/PNG

## 📞 Support

Untuk pertanyaan atau bug report, buat issue di repository GitHub atau hubungi tim IT Ryusei.

---

**Ryusei Event Management System** © 2026. Built with ❤️ for seamless event management.
