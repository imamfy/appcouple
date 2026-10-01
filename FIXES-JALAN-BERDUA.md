# Perbaikan Jalan Berdua

Perubahan:
- Memperbaiki kontras hero mode gelap desktop dengan surface, border, judul, dan teks khusus dark mode.
- Menghapus implementasi tab lama di `product.css` yang sudah digantikan `trip-tabs-2026.css`.
- Menghapus selector mati `.trip-itinerary-wide`.
- Mempertahankan perilaku sticky topbar desktop tanpa konflik `position: relative`.
- Menampilkan logo kompak pada topbar mobile sehingga tidak ada area header kosong.
- Menambahkan scrollbar tipis sebagai petunjuk bahwa daftar tab mobile dapat digeser.
- Menambahkan nama aksesibel untuk tombol Simpan rencana dan Simpan Excel.

Validasi:
- Seluruh stylesheet berhasil diparse oleh PostCSS.
- Tidak ada lagi referensi `.trip-itinerary-wide`.
- Blok tab Jalan Berdua lama telah dihapus dari `product.css`.
- Konflik selector-konteks terkait perjalanan turun dari 90 menjadi 86; konflik properti turun dari 46 menjadi 42. Konflik tersisa merupakan lapisan responsif/upgrade yang masih aktif dan diperlukan oleh komponen.

## Tambahan perbaikan Pengeluaran mobile
- Form Pengeluaran berubah menjadi satu kolom pada layar hingga 540 px.
- Label, input, select, dan tombol dibatasi ke lebar container dengan `min-width: 0`, `max-width: 100%`, dan `box-sizing: border-box`.
- Ringkasan biaya memakai kolom `minmax(0, 1fr)` dan nominal panjang dapat membungkus tanpa mendorong container.
- Baris pengeluaran serta nominal aktual dibatasi agar tidak menimbulkan horizontal overflow.
- Divalidasi pada viewport 390×844: seluruh kontrol berada pada x=40–335 px dan horizontal overflow dokumen = 0.
- Mode terang/gelap telah diperiksa pada mobile dan desktop.

## Audit dan perbaikan fitur Peta
- Mengganti tile jalan OpenStreetMap yang mengembalikan halaman 403 dengan Esri World Street Map.
- Mengganti tile gelap CARTO yang menampilkan `API KEY REQUIRED` dengan Esri World Dark Gray Base.
- Peta otomatis memakai basemap gelap saat tema aplikasi gelap dan basemap terang saat tema terang.
- Pilihan pengguna untuk layer satelit atau medan tetap dipertahankan ketika tema berubah.
- Perencana rute mobile diuji pada 390×844: seluruh panel dan kontrol berada di dalam viewport, horizontal overflow = 0.
- Tile terang dan gelap masing-masing diuji: 15/15 tile berhasil dimuat, 0 gagal.

## Perbaikan lanjutan layout Peta mobile
- Memisahkan tiga lapisan bawah: tombol Abadikan Jejak di bottom 7 px, Lokasi Berdua di bottom 75 px, dan kartu Rute Kalian di bottom 131 px.
- Menambahkan jarak aman antarkomponen sehingga tidak saling menutup pada layar 390×844.
- Menyamakan dark mode untuk header, tombol header, pemilih layer, dock aksi, tombol lokasi, kartu pasangan, kartu rute, pencarian, panel live, drawer album, dan preview kenangan.
- Tombol layer aktif pada dark mode sekarang memakai gradien ungu, bukan latar putih.

## Perbaikan Keuangan — Transaksi Baru
- Menyelesaikan dark mode panel Pembaca Transfer & Struk, tombol Galeri/Kamera, area preview, progress scan, tombol tutup, dan sticky action bar.
- Mengubah warna judul dan deskripsi pembaca struk agar terbaca pada permukaan gelap.
- Menaikkan kontras label TRANSAKSI BARU dari 4.11:1 menjadi di atas 4.5:1.
- Diuji pada mobile 390×844 dan desktop 1280×720 tanpa horizontal overflow.
- Audit WCAG pada modal setelah perbaikan: 0 pelanggaran.

## Perbaikan tombol transaksi Keuangan
- Menambahkan dark surface khusus kartu Pengeluaran dan Pemasukan.
- Pengeluaran memakai aksen rose gelap; Pemasukan memakai aksen hijau gelap.
- Judul, deskripsi, label, hover, dan keyboard focus diselaraskan dengan mode gelap.
- Diuji pada mobile 390×844 dan desktop 1280×720 tanpa horizontal overflow.
- Audit WCAG pada panel Transaksi Baru: 0 pelanggaran.

## v31.6.37 — Otak AI Backup & Recovery Center
- Menambahkan backup ZIP/JSON terstruktur untuk data lokal aplikasi.
- Menambahkan validasi checksum SHA-256, preview isi backup, dan batas file 50 MB.
- Menambahkan tiga mode pemulihan: data hilang saja, gabungkan, dan ganti penuh.
- Membuat snapshot pemulihan otomatis sebelum restore serta tombol rollback.
- Menambahkan snapshot lokal harian dan tampilan responsif mode terang/gelap.
- Kredensial, token, sesi login, API key, dan media cloud tidak dimasukkan ke backup.

## v31.6.38 — Matus responsif dan sinkron tema
- Menyelaraskan tampilan Matus Live, Chat, dan Pengaturan pada desktop/mobile serta mode terang/gelap.
- Menambahkan tombol tema langsung di header Matus yang mengikuti sumber tema global AppCouple.
- Memoles kartu utama, karakter, tab, prompt pintar, chat, composer, dan panel pengaturan.
- Mengubah seluruh tombol Matus yang terlihat agar memakai ikon Font Awesome.
- Mempertahankan ikon notifikasi saat label status diperbarui secara dinamis.
- Memperbarui cache offline untuk stylesheet Matus dan Backup Center.
- Diuji pada 1280×800 dan 390×844 untuk terang/gelap tanpa overflow horizontal atau tombol sentuh di bawah 40 px.

## v31.6.39 — Sinkronisasi tombol bicara dan bubble chat
- Tombol Mulai bicara kini memakai gradien aksen dan teks putih pada mode terang maupun gelap.
- Bubble pengguna memakai gradien aksen konsisten; bubble Matus mengikuti surface terang/gelap dengan kontras teks yang sesuai.
- Diuji pada desktop dan mobile tanpa overflow horizontal.

## v31.6.40 — Kontras tab Live dan Chat
- Teks, ikon, dan subteks tab Live/Chat dibuat hitam pada mode terang dan gelap.
- Tab aktif memakai gradien pastel agar teks hitam tetap terbaca.
- Tab nonaktif memakai permukaan terang konsisten pada kedua tema.

## v31.6.41 — Itinerary ringkas per hari
- Menambahkan pemilih Hari 1, Hari 2, dan seterusnya dalam rail horizontal.
- Daftar hanya menampilkan agenda hari yang sedang dipilih sehingga tidak memanjang ke bawah.
- Form Tambah agenda disembunyikan sampai dibutuhkan.
- Pada desktop editor terbuka sebagai panel ringkas; pada mobile menjadi sheet layar dengan tombol tutup.
- Edit agenda otomatis membuka hari dan editor yang sesuai.
- Hari kosong menyediakan tombol Tambah agenda Hari ini.
- Diuji menambah agenda Hari 2 pada desktop/mobile tanpa overflow horizontal.

## v31.6.42 — Traffic-aware itinerary dengan Matus
- Menambahkan Analisis Hari tanpa mengubah Form Tambah Agenda.
- Menghitung rute berurutan dari asal perjalanan ke agenda pertama lalu dari agenda sebelumnya ke agenda berikutnya.
- Menampilkan jarak, durasi, rekomendasi berangkat, estimasi tiba, buffer, dan konflik jadwal.
- Menambahkan Google Routes traffic live melalui Netlify Function terautentikasi dan rate-limited.
- Menambahkan fallback OSRM yang jujur: tidak pernah mengklaim kemacetan tanpa data traffic live.
- Matus memakai data rute faktual untuk ringkasan dan tidak menebak kondisi jalan.
- Hasil tersimpan dan ikut sinkron dalam bundle Jalan Berdua.
- Tampilan desktop memakai grid dua kolom; mobile memakai rail horizontal agar tidak memanjang berlebihan.

## v31.6.43 — Klik lokasi membuka rute Google Maps
- Nama lokasi pada kartu agenda kini menjadi tautan Google Maps Directions.
- Asal rute agenda pertama menggunakan Dari mana; agenda berikutnya menggunakan lokasi sebelumnya.
- Moda Mobil/Motor, Jalan kaki, Sepeda, dan Transit dipetakan ke moda Google Maps yang sesuai.
- Tujuan pada hasil Analisis Matus juga membuka rute yang sama.
- Tombol Maps lama diselaraskan agar membuka petunjuk rute, bukan hanya hasil pencarian lokasi.

## v31.6.44 — Rute gratis tanpa API berbayar
- Menghapus kebutuhan Google Routes API, Geocoding API, dan environment key berbayar.
- Menggunakan Nominatim/OpenStreetMap untuk geocoding dan OSRM untuk rute serta estimasi durasi.
- Google Maps hanya dibuka melalui Directions URL ketika lokasi diklik; tidak memerlukan API key aplikasi.
- Tidak mengklaim data macet/lancar. Pengguna melihat traffic aktual setelah rute terbuka di Google Maps.
- Permintaan Nominatim dijalankan berurutan dan hasil geocoding disimpan sementara di perangkat.
