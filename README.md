# AppCouple Together 31.0 — Matus 2026

Upgrade penuh antarmuka Matus: pengalaman Live dan Chat yang lebih tenang, status AI transparan (siap, mendengarkan, berpikir, berbicara), starter kontekstual, menu konteks multimodal, indikator izin data, composer mobile 16 px, dan pengaturan privacy-first yang dikelompokkan. Tampilan telah diuji pada desktop 1440×1000 dan mobile 390×844 tanpa overflow horizontal.

# AppCouple Together 30.2 — Activity Journal

Upgrade dari 29.8: desain lebih tenang, navigasi konsisten, tema light/dark terpusat, dan perbaikan alur galeri–peta. Paket ini belum diterapkan ke situs produksi.

## Upgrade Aktivitas 30.2
- Timeline, ringkasan saldo, kartu cerita dan filter ditata ulang untuk light/dark serta mobile/desktop.
- Pencarian judul/cerita/nama, filter Kamu/Pasangan, kategori Chat dan Mood, serta urutan terbaru/terlama.
- Pengelompokan tanggal dan jam WIB; catatan duplikat berdasarkan ID dan tanggal tidak valid disaring.
- Tampilkan bertahap 20 cerita. Sumber backend tetap dibatasi 80 catatan terbaru; ini bukan seluruh arsip akun.
- Status memuat/gagal/dimuat dibedakan; Muat ulang memasang ulang watcher aktivitas. Data yang sebelumnya diterima tetap terlihat saat pembaruan gagal.
- Saldo pembukuan terakhir dipertahankan meskipun periodenya lama. Data saldo yang belum ada ditulis Belum tersedia, bukan Rp0. Pengeluaran bulan ini hanya memakai ringkasan periode saat ini; data lama tidak dianggap pengeluaran bulan ini.
- Hapus cerita tetap harus dikonfirmasi, hanya cerita milik pengguna. Hapus yang belum selesai diblokir dari klik ganda. Respons nol baris bukan sukses palsu. Menghapus cerita tidak menghapus transaksi/foto/pesan sumber.
- Retensi 24 jam tetap seperti versi sebelumnya. Penghapusan kedaluwarsa dan aturan akses backend tidak diperluas. Tidak ada migrasi database baru.

QA: `qa/activity-v302.json` (8 kombinasi lebar/tema, demo + fixture lokal) dan `qa/activity-delete-v302.json` (mock pembatasan hapus). Uji browser: jalankan server Python dari root proyek pada port 4193 lalu `node tests/activity-v302.cjs`. Uji hapus: `node tests/activity-delete-v302.cjs`. Playwright + Chromium diperlukan; `CHROMIUM_PATH` dapat diatur.

Batas verifikasi: sinkronisasi dua akun produksi, RLS dan layanan eksternal belum diuji langsung. Lingkungan lokal tidak dapat mengambil SDK Supabase eksternal; pengujian menggunakan demo dan fixture, bukan login produksi. Laporan versi sebelumnya di qa adalah riwayat, bukan tes ulang semua fitur pada 30.2. Perbaikan chat 30.1 tetap disertakan.

## Perbaikan chat 30.1
- Input pesan dan pencarian chat 16 px untuk menghindari pemicu auto-zoom Safari; zoom manual tidak dikunci.
- Tinggi chat mengikuti Visual Viewport saat keyboard dibuka/ditutup; landscape sempit mendapat layout ringkas. Tidak ada trik mengubah meta viewport atau memaksa reset skala zoom pengguna.
- Header, bubble, kotak pesan, emoji dan status pengiriman mengikuti light/dark Together.
- Draf tidak dihapus sebelum pengiriman terkonfirmasi. Klik kirim ganda saat request berlangsung diblokir. Draf baru yang sedang diketik tidak ditimpa oleh respons pengiriman lama.
- Gagal mencatat aktivitas tidak lagi dianggap sebagai kegagalan pesan yang sebenarnya sudah terkirim.
- Ctrl/Cmd+Enter mengirim; Enter tetap baris baru. Pesan baru tidak memaksa scroll ke bawah jika sedang membaca pesan lama; gunakan tombol Pesan terbaru.
- Notifikasi pop-up tidak menutupi header ketika percakapan sedang terbuka.

**Batas:** draf dipertahankan di input pada sesi aktif, bukan disimpan offline setelah reload. Jika koneksi putus setelah server menerima pesan, status bisa belum terkonfirmasi; cek percakapan sebelum kirim ulang. Tidak ada perubahan kontrak backend atau janji exactly-once pada retry jaringan.

**QA 30.1:** `qa/chat-v301.json` dan `qa/chat-send-v301.json`. Lima ukuran/orientasi × dua tema, fokus, kirim, pencarian, tombol kembali, simulasi keyboard mengecil dan pulih. Belum diuji pada iPhone/Safari fisik atau pengiriman realtime dua akun produksi.

Untuk menjalankan tes chat, jalankan server lokal pada port **4191**, lalu `node tests/chat-v301.cjs` dan `node tests/chat-send-v301.cjs` (Playwright + Chromium; `CHROMIUM_PATH` dapat diatur). Tes lama di README memakai port 4189.

Setelah deploy: tutup dan buka ulang aplikasi/PWA agar versi 30.1 aktif. Jika tab lama sudah terlanjur membesar, reload sekali atau kembalikan zoom browser ke 100% secara manual. Uji mengetik, kirim, tutup keyboard, rotasi layar, serta pinch-zoom di perangkat Anda.

## Mulai di sini
1. Simpan cadangan versi produksi dan data terlebih dahulu.
2. Gunakan seluruh isi folder ini sebagai proyek Netlify, bukan hanya `index.html`. Pertahankan environment variables pada situs Netlify yang sudah ada.
3. Deploy melalui alur Netlify yang memproses `netlify/functions` (Git atau Netlify CLI). Upload statis saja tidak menjalankan backend galeri.
4. Pastikan halaman login menampilkan **Build 30.2.19 · Together — Activity Journal**. Tutup/buka ulang PWA atau refresh jika versi lama masih tersimpan.
5. Login dan periksa Drive, unggah foto berkoordinat, lihat pin peta, edit transaksi, lalu verifikasi akun pasangan.

Tidak ada perubahan skema database untuk upgrade UI ini. Migrasi yang sebelumnya diperlukan tetap tersedia di `docs/`; jangan menjalankan seluruh SQL lama secara membabi buta. Baca `docs/START-HERE-V29.md` dan `docs/DEPLOY-CURRENT.md` untuk prasyarat backend.

## Perubahan utama
- Beranda: hero lebih ringkas, aksi harian didahulukan, dekorasi/parallax berlebihan dikurangi.
- Navigasi: sidebar lebih terstruktur, kontrol tema cepat, status navigasi untuk pembaca layar, pencarian Ctrl/Cmd+K, dan skip link.
- Keuangan: metrik, form, toolbar dan tab lebih konsisten. Tab keyboard dipasangkan berdasarkan identitas, bukan urutan HTML.
- Kenangan: filter, pengurutan, grid/ringkas, detail foto dan tombol tindakan mobile diperbaiki. Detail foto ditutup sebelum membuka peta.
- Peta: sumber Drive dan jejak lama tetap digabung; pembaruan jejak lama tidak menghapus foto Drive; pin tidak dibuat dari koordinat kosong.
- Couple Space dan Matus memakai palet surface, border, teks dan aksen yang sama dengan halaman utama.
- Zoom browser tidak lagi dikunci. Preferensi reduced motion dihormati.

## Struktur
- `index.html`: dokumen utama.
- `src/styles/tokens.css`: warna light/dark, tipografi dan alias kompatibilitas.
- `src/styles/components.css`: fondasi komponen lama dalam urutan cascade asli; aturan identik dibersihkan.
- `src/styles/product.css`: tampilan Together, aturan responsif dan dialog.
- `src/*.js`: pengendali fitur yang sudah ada; tidak membuat pengendali tema/navigasi paralel.
- `netlify/functions/`: backend dan aturan akses; dipertahankan.
- `docs/`: panduan teknis dan enam berkas migrasi SQL.
- `tests/`: pengujian regresi dan browser.
- `qa/`: laporan audit dan hasil pengujian. Laporan bernomor versi lama adalah sejarah pengujian, bukan hasil v30.

## Pembersihan yang benar-benar dilakukan
16 stylesheet digabung menjadi satu fondasi dan file asal dihapus. 237 aturan CSS identik dibuang. Tiga pengendali efek gerak lama dihapus; fungsi navigasi/bagikan galeri dipindah ke pengendali galeri. Delapan hasil pengujian/log lama dibersihkan. Audit hash tidak menemukan file identik; audit referensi aktif tidak menemukan stylesheet/modul/cache lokal yang hilang.

Ini bukan penghapusan semua selector legacy. Aturan yang mungkin dipakai oleh markup dinamis dipertahankan untuk mencegah fitur rusak. Nama berkas JS historis yang masih aktif bukan file duplikat. Dependensi vendor, asset, server dan migrasi tidak dihapus hanya karena tidak terlihat pada layar utama.

## QA & batas verifikasi
Lihat `qa/responsive-v30.json`, `qa/workflows-v30.json`, `qa/dependencies-v30.json` dan `qa/cleanup-v30.json`.

Browser lokal diuji pada lebar 320, 390, 768 dan 1440, light/dark: lima halaman utama dan peta. Alur diuji dengan demo dan watcher non-demo tiruan. Pengujian backend memakai mock, bukan akun produksi.

Belum diverifikasi langsung: OAuth Google, isi Drive produksi, sinkronisasi lintas perangkat nyata, GPS perangkat fisik, provider AI, WebGL di semua perangkat, dan RLS produksi. Foto tanpa koordinat valid tidak bisa diletakkan pada peta; tambahkan alamat pada Edit cerita. Gambar peta/jaringan eksternal dan suara dapat diblokir lingkungan uji.

## Menjalankan tes
Butuh Node, Playwright dan Chromium. Jalankan server lokal dari folder proyek:

```sh
python3 -m http.server 4189 --bind 127.0.0.1
```

Di terminal terpisah:

```sh
node tests/security-v29.cjs
node tests/auto-folder-v296.cjs
node tests/planner-concurrency.cjs
node tests/backend-mock.cjs
CHROMIUM_PATH=/path/to/chromium node tests/workflows-v30.cjs
CHROMIUM_PATH=/path/to/chromium node tests/responsive-v30.cjs
```

Pengujian browser menghasilkan laporan di `qa/`. Jangan memasukkan client secret, service-account private key, atau refresh token ke sumber frontend.


## Location history privacy — v31.1.1
- Stores only one current location-history row per person.
- Re-sharing within 24 hours updates the same row instead of adding duplicates.
- Rows older than 24 hours are hidden and deleted automatically on use.
- Apply `docs/LOCATION-HISTORY-24H.sql` for database-level enforcement.


## Otak AI simplification — v31.1.2
- Removed Tanya AI and Analisa from Otak AI.
- Otak AI now opens directly to private provider settings used by Matus.


## 31.3.0 — Jalan Berdua Pro
- Menyamakan data aplikasi dengan workbook Excel: pengeluaran aktual, metode, itinerary lengkap, checklist bersama, Maps, dan insight Matus.


## 31.3.1 — Link Maps Itinerary
- Nama lokasi wajib diisi pada itinerary dan otomatis menghasilkan link Google Maps di aplikasi serta Excel.


## 31.3.2 — Total Estimasi Itinerary
- Menjumlahkan seluruh estimasi biaya agenda dan menampilkannya di itinerary serta Excel.


## 31.3.3 — Editable Itinerary
- Agenda itinerary dapat diedit melalui form yang terisi otomatis, dibatalkan, dan disimpan ulang.
- Form diubah menjadi alur 3 langkah: waktu, agenda/tempat, dan detail biaya.


## 31.4.0 — App/Excel Alignment
- Dashboard, Rencana Budget Jalan Berdua, dan Checklist Perjalanan memakai struktur serta perhitungan yang sama di aplikasi dan Excel.


## 31.5.0 — Focused Tabs
- Menambahkan tab Jalan Berdua pada desktop dan mobile sehingga hanya satu bagian aktif ditampilkan.
- Menyederhanakan header dan hero mobile agar halaman lebih ringkas.


## 31.5.1 — Jalan Berdua Excel Repair
- Formula memiliki cached result agar preview mobile/Drive tidak kosong atau error.
- Belanja, Darurat/Cadangan, dan Lainnya masuk total aktual.
- Dashboard, Budget, Pengeluaran, Pembagian, Tabungan, Itinerary, Checklist, dan Matus konsisten.


## 31.6.0 — Complete Trip Excel + Previous Home
- Beranda dikembalikan ke tampilan AppCouple sebelumnya.
- Excel Jalan Berdua menjadi 13 sheet dan mengambil data rute, biaya, agenda, tabungan, checklist, Maps, kenangan, kontak, reservasi, serta riwayat aktivitas.
- Formula memiliki cached result dan total lintas sheet konsisten.

## 31.6.1 — Matus Daily Whisper
- Kontrol mood pasangan pada Couple Today dihapus.
- Matus menampilkan semangat, saran, dan pengingat kontekstual secara acak saat Beranda dibuka.
- Pesan berganti otomatis setiap 45 detik dan dapat dibuat ulang lewat tombol refresh di kartu Matus.
- Tampilan mendukung mobile, desktop, mode terang, dan mode gelap.
