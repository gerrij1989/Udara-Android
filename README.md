# UDARA Android — Proyek awal offline

**UDARA — Update DAta apaRAtur**  
Dinas Lingkungan Hidup Daerah Provinsi Sulawesi Utara

Ini adalah proyek sumber Android Studio awal, bukan APK siap pasang. Aplikasi dirancang bekerja tanpa internet dan menggunakan database lokal Room/SQLite.

## Fitur yang ada pada proyek awal
- Database lokal SQLite melalui Room.
- Data aparatur: nomor, nama, NIP, tempat/tanggal lahir, jenis kelamin, agama, pendidikan, pangkat, golongan ruang, TMT pangkat, jenis jabatan, eselon, nama jabatan, TMT KGB terakhir.
- Umur dihitung dari tanggal lahir pada daftar.
- Nominatif A–Z dan DUK berdasarkan urutan teks golongan ruang, TMT pangkat, tanggal lahir, lalu nama.
- Perkiraan KGB dari TMT terakhir + 24 bulan.
- Perkiraan pemeriksaan pangkat dari TMT pangkat + 48 bulan (hanya penanda awal, bukan kelayakan resmi).
- Pengingat ulang tahun H-2, H-1, dan hari ini menggunakan WorkManager.
- Ekspor CSV dan backup/pemulihan JSON melalui pemilih berkas Android.
- Tema biru-hijau.

## Cara membuka dan membangun
1. Instal Android Studio versi stabil dan Android SDK Platform 35.
2. Ekstrak folder proyek `UDARA_Android`.
3. Buka folder tersebut dari Android Studio.
4. Tunggu Gradle Sync; komputer perlu internet saat pertama kali mengunduh dependensi. Setelah dependensi terpasang, pemakaian aplikasi tidak memerlukan internet.
5. Hubungkan HP Android dengan USB debugging atau gunakan emulator.
6. Pilih **Run** untuk memasang versi debug. Untuk membuat APK, gunakan **Build > Build APK(s)**.

## Catatan penting sebelum operasional
- Proyek ini belum dibangun atau diuji pada perangkat di lingkungan ini. Jalankan Gradle Sync dan pengujian di Android Studio sebelum digunakan.
- Belum ada login admin atau enkripsi database. Jangan simpan data kepegawaian resmi sebelum keamanan ditambahkan dan disetujui pengelola TI instansi.
- Impor JSON memulihkan data pegawai berdasarkan NIP; format CSV yang didukung mengikuti header/urutan yang dihasilkan ekspor aplikasi.
- Urutan DUK saat ini merupakan pengurutan sederhana berdasarkan teks golongan ruang, bukan pengurutan resmi yang telah diverifikasi. Untuk operasional, gunakan tabel peringkat golongan eksplisit dan aturan DUK yang disahkan instansi.
- Pengingat menggunakan pekerjaan periodik 24 jam; Android dapat menunda waktu eksekusi karena optimasi baterai. Izin notifikasi diperlukan pada Android 13+.
- Pengingat pangkat +48 bulan hanyalah perkiraan pemeriksaan awal. Jadwal KGB juga harus diverifikasi dengan SK dan aturan yang berlaku.
- Backup JSON hanya menyertakan data pegawai; jadwal dapat dihitung ulang dari data pegawai. Simpan backup di media aman dan uji pemulihan.
- Database saat ini memakai fallback destructive migration untuk pengembangan. Jangan melakukan upgrade versi tanpa migrasi yang menjaga data.

## Membuat APK melalui GitHub Actions (bisa dimulai dari HP)

File `.github/workflows/build-apk.yml` disediakan untuk membangun APK debug di runner GitHub, karena APK tidak dibangun langsung dalam paket sumber ini.

1. Buat akun GitHub atau masuk ke akun yang sudah ada.
2. Buat repository baru bernama `UDARA-Android`. Jika akan memasukkan data nyata kelak, jangan unggah database/data pegawai; proyek ini hanya berisi kode sumber.
3. Unggah seluruh isi folder proyek ini ke repository, termasuk folder tersembunyi `.github/workflows`.
4. Buka tab **Actions**, pilih **Build UDARA Android APK**, lalu tekan **Run workflow** (atau tunggu workflow berjalan setelah push ke `main`/`master`).
5. Tunggu proses selesai. Jika berhasil, buka run yang hijau, gulir ke bagian **Artifacts**, lalu unduh `UDARA-debug-APK`.
6. Ekstrak ZIP artifact dan ambil `app-debug.apk`. Kirim APK ke HP, buka file tersebut, lalu izinkan pemasangan dari sumber itu jika Android meminta.

Catatan:
- Ini menghasilkan **debug APK untuk uji coba**, bukan APK release yang ditandatangani untuk distribusi resmi.
- Build mungkin gagal dan memerlukan perbaikan jika ada masalah pada kode atau dependensi. Jangan anggap APK siap operasional sebelum build berhasil dan diuji di perangkat.
- Proyek ini belum memiliki login admin/enkripsi database dan masih memerlukan verifikasi aturan DUK, KGB, serta kenaikan pangkat.
