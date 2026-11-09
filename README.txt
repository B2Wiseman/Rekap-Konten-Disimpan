CONTENT HUB - SETUP

FILE:
- Code.gs: backend Google Apps Script, scanner Drive, API website, rename file on status edit.
- Index.html: website UI.

PENTING - PERBAIKI ROOT_FOLDER_ID
Pada Code.gs, pastikan ROOT_FOLDER_ID adalah ID folder "Konten Utama" yang benar.
ID yang Anda kirim di percakapan: 1NbnIjx0gHNq3fj6kqKcFN29Hoc2jQhHD
Jika berbeda, ganti dengan ID folder yang benar.

CARA MEMASANG
1. Buka Google Spreadsheet yang menjadi sumber data.
2. Pilih Extensions > Apps Script.
3. Ganti isi Code.gs dengan file Code.gs dari paket ini.
4. Klik tombol + > HTML, beri nama persis: Index. Tempel isi Index.html.
5. Pastikan akun yang menjalankan Apps Script memiliki akses Editor ke folder Drive dan spreadsheet.
6. Di CONFIG, isi ALLOWED_EMAILS jika ingin membatasi akses berdasarkan email. Contoh:
   ALLOWED_EMAILS: ["tim1@gmail.com", "tim2@gmail.com"]
   Catatan: Session.getActiveUser().getEmail() dapat kosong pada beberapa jenis deployment/akun pribadi.
7. Simpan. Jalankan updateRekapKonten dari editor Apps Script satu kali, lalu berikan izin akses.
8. Jalankan pasangTriggerOtomatis satu kali untuk sinkronisasi Drive -> Sheets setiap 15 menit.
9. Deploy > New deployment > Select type: Web app.
   Execute as: Me.
   Who has access: pilih opsi paling terbatas yang sesuai (misalnya akun dalam organisasi atau pengguna Google yang diizinkan).
10. Deploy, salin URL Web App, lalu buka URL tersebut.

CARA KERJA
- File baru yang dimasukkan ke folder Drive akan masuk ke spreadsheet saat trigger berjalan (maksimal sekitar 15 menit), atau setelah fungsi updateRekapKonten dijalankan manual.
- Website membaca spreadsheet saat dibuka, tombol Refresh ditekan, dan otomatis memuat ulang setiap 60 detik.
- Saat dropdown status diubah, Apps Script mengganti awalan nama file Drive:
  Belum Disetor = tanpa simbol
  Disetor = ✈️
  Direvisi = 🔄
  Dipost = ✅
- Setelah rename, spreadsheet dibangun ulang dari Drive. Website kemudian mengambil data terbaru.

CATATAN PENTING
- Kolom "File ID" ditambahkan sebagai kolom H dan disembunyikan. Ini adalah identitas stabil untuk mengubah file yang tepat, walaupun nama file berubah.
- Kolom spreadsheet menjadi:
  A Bulan, B Tanggal / Folder, C Brand, D Nama Konten, E Status,
  F Link File, G Terakhir Diubah, H File ID.
- Bulan untuk folder dengan awalan ddmmyy (misalnya 071026) dibaca dari angka bulan. Untuk folder tanpa awalan tanggal, bulan diambil dari tanggal terakhir file diubah.
- Script mendukung video serta ekstensi gambar yang tercantum di isSupportedFile_.
- Trigger waktu Apps Script tidak selalu berjalan tepat pada detik/menit yang ditentukan.
- Website ini adalah Google Apps Script Web App; jangan membuka Index.html langsung dari komputer karena google.script.run hanya tersedia saat dijalankan melalui Apps Script.
- Jika spreadsheet Anda menggunakan header/kolom lain, backup terlebih dahulu. Script updateRekapKonten membersihkan isi sheet "Rekap Konten" dan menulis ulang tabel.
- Jika Anda mengganti Code.gs atau Index.html setelah deployment, buka Deploy > Manage deployments > Edit > pilih New version > Deploy agar perubahan tampil di URL Web App.
