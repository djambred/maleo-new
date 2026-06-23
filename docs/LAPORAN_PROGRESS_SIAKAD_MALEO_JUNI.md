# LAPORAN PROGRESS PENGEMBANGAN
# SIAKAD MALEO — Sistem Informasi Akademik

**Tanggal:** 16 Juni 2026  
**Tim:** Canavaro (Frontend) & Valdi (Backend)  
**Status:** Final Sprint  

---

## 1. RINGKASAN EKSEKUTIF

SIAKAD Maleo adalah platform Sistem Informasi Akademik berbasis web yang dirancang khusus untuk memodernisasi pengelolaan administrasi, kegiatan belajar mengajar (KBM), absensi, serta sistem penilaian di Sekolah Maleo. Tujuan utama proyek ini adalah menyediakan satu pusat data terintegrasi yang dapat diakses dengan mudah oleh semua pemangku kepentingan sekolah, mulai dari staf administrasi hingga orang tua murid.

Sistem ini ditujukan untuk lima pengguna utama, yaitu Administrator (operator sekolah), Kepala Sekolah (pimpinan), Guru (tenaga pendidik), Siswa (peserta didik), dan Wali Murid (orang tua). Dengan adanya portal khusus untuk masing-masing pengguna, proses komunikasi dan pemantauan akademik dapat berjalan secara real-time dan transparan.

Saat ini, pengembangan sistem telah memasuki fase **Final Sprint** (Tahap Akhir) dengan persentase penyelesaian keseluruhan mencapai sekitar **90%**. Seluruh fungsi inti untuk operasional harian sekolah telah selesai dikembangkan dan diuji secara internal untuk memastikan stabilitas sistem.

Pencapaian terbesar dalam siklus pengembangan ini adalah penyelesaian modul **Rencana Pelaksanaan Pembelajaran (RPS)** yang terstruktur, sistem **Kalkulasi Nilai Akhir Otomatis** berdasarkan bobot yang dinamis, serta mekanisme **Data Locking (Penguncian Nilai)** yang menjamin integritas data nilai dari perubahan atau manipulasi yang tidak sah setelah dikonfirmasi oleh guru.

---

## 2. FITUR YANG SUDAH SELESAI

Berdasarkan pemeriksaan langsung pada kode program backend (Express.js) dan tampilan frontend (Next.js), berikut adalah daftar fitur yang telah selesai dibangun, dikelompokkan berdasarkan modul utama:

### 🔐 Sistem Login & Keamanan
- **Login Multi-Peran (Multi-Role Login):** Sistem login satu pintu yang secara otomatis mendeteksi peran pengguna (Admin, Guru, Siswa, Wali, atau Kepala Sekolah) dan mengarahkan mereka ke tampilan yang sesuai. (Status: ✅ Selesai)
- **Pembatasan Hak Akses (Route Guarding & RBAC):** Keamanan ketat di mana pengguna tidak dapat membuka halaman milik peran lain (misal: siswa tidak bisa mengakses halaman nilai admin). (Status: ✅ Selesai)
- **Paksa Ganti Password Pertama Kali (Force Change Password):** Fitur yang mewajibkan pengguna baru untuk mengganti password bawaan sistem saat pertama kali masuk demi menjaga kerahasiaan akun. (Status: ✅ Selesai)
- **Sistem Sesi Aman (JWT Authentication):** Menggunakan token digital aman untuk menjaga pengguna tetap masuk selama sesi aktif tanpa harus mengetik password berulang kali. (Status: ✅ Selesai)
- **Lupa Password Sistem (Forgot Password):** Memungkinkan pengguna memulihkan akses akun secara mandiri menggunakan kode login unik mereka. (Status: ✅ Selesai)

### 👨‍💼 Portal Admin
- **Manajemen Data Guru & Siswa:** Panel untuk menambah, mengubah, atau menonaktifkan data profil guru dan siswa secara cepat. (Status: ✅ Selesai)
- **Manajemen Wali Murid (Guardians):** Menghubungkan data wali murid dengan siswa yang bersangkutan agar pemantauan orang tua berjalan selaras. (Status: ✅ Selesai)
- **Pengaturan Tahun Ajaran & Kelas:** Menentukan tahun ajaran aktif serta membuat kelas-kelas baru beserta penunjukan wali kelasnya. (Status: ✅ Selesai)
- **Pengaturan Mata Pelajaran:** Mendaftarkan mata pelajaran baru beserta jumlah jam pelajaran per minggu. (Status: ✅ Selesai)
- **Penyusunan Jadwal Pelajaran (KBM):** Memetakan hari, jam pelajaran, ruang kelas, guru pengampu, dan mata pelajaran secara teratur. (Status: ✅ Selesai)
- **Setup Bobot Penilaian:** Menentukan persentase kontribusi nilai UTS (PPTS) dan UAS (PSAS) terhadap nilai akhir rapor untuk setiap mata pelajaran. (Status: ✅ Selesai)
- **Pusat Pengumuman Sekolah:** Membuat dan mempublikasikan pengumuman sekolah yang ditargetkan kepada kelompok tertentu (misal: hanya untuk orang tua). (Status: ✅ Selesai)

### 👨‍🏫 Portal Guru (Maleo Hub)
- **Dashboard Mengajar:** Menampilkan jadwal mengajar pada hari tersebut, pengumuman terbaru, serta ringkasan statistik kelas bimbingan. (Status: ✅ Selesai)
- **Penyusunan Rencana Pelaksanaan Pembelajaran (RPS):** Modul interaktif (wizard) untuk membantu guru menyusun rencana KBM mingguan (maksimal 16 pertemuan per semester). (Status: ✅ Selesai)
- **Pengunggahan Materi Belajar (LMS):** Mengunggah bahan ajar baik berupa berkas (PDF, PPT, Word) maupun tautan video/website eksternal yang dapat diakses siswa. (Status: ✅ Selesai)
- **Pencatatan Kehadiran Siswa (Absensi Kelas):** Menginput absensi siswa (Hadir, Izin, Sakit, Alpa) per pertemuan secara digital dilengkapi kolom catatan/keterangan. (Status: ✅ Selesai)
- **Input & Konfirmasi Nilai (UTS/UAS/Tugas/Kuis):** Guru dapat memasukkan nilai siswa dan melakukan kalkulasi otomatis berdasarkan bobot pelajaran. (Status: ✅ Selesai)
- **Sistem Penguncian Nilai (Data Locking):** Setelah guru mengonfirmasi nilai, sistem akan mengunci data tersebut agar tidak bisa diubah kembali secara tidak sengaja. (Status: ✅ Selesai)
- **Presensi Mandiri Guru (Self Check-in):** Fitur bagi guru untuk melakukan absensi kehadiran mereka sendiri ke sistem sekolah. (Status: ✅ Selesai)
- **Konsultasi Wali Murid:** Ruang komunikasi chat dua arah antara guru dan orang tua siswa untuk membahas perkembangan siswa. (Status: ✅ Selesai)
- **Ekspor Absensi ke Excel:** Mengunduh rekapitulasi data kehadiran siswa ke file Excel berformat Sekolah Maleo. (Status: ✅ Selesai)

### 👨‍🎓 Portal Siswa (Maleo Hub)
- **Dashboard Belajar Siswa:** Menampilkan tugas-tugas aktif, pengumuman terbaru dari sekolah, jadwal belajar hari ini, dan grafik rata-rata nilai. (Status: ✅ Selesai)
- **Pusat Materi Pembelajaran:** Siswa dapat melihat materi pelajaran, rincian pertemuan RPS, serta mengunduh berkas materi yang disediakan guru. (Status: ✅ Selesai)
- **Manajemen Tugas Mandiri:** Menampilkan daftar tugas yang diberikan guru beserta batas waktu pengumpulan (deadline). (Status: ✅ Selesai)
- **Pengumpulan Tugas Digital (Submission):** Siswa dapat mengunggah hasil jawaban tugas berupa dokumen atau menulis jawaban teks secara langsung di sistem. (Status: ✅ Selesai)
- **Pengecekan Riwayat Absensi:** Siswa dapat melihat persentase kehadiran mereka sendiri dalam indikator warna yang ramah mata. (Status: ✅ Selesai)
- **Pengecekan Nilai Transparan:** Menampilkan rekapitulasi nilai tugas, kuis, UTS, UAS, dan nilai akhir rapor yang diperoleh siswa secara real-time (hanya membaca). (Status: ✅ Selesai)

### 👪 Portal Orang Tua (Maleo Connect)
- **Dashboard Monitoring Anak:** Menampilkan ringkasan status kehadiran harian anak, pengumuman khusus orang tua, serta daftar tugas anak yang belum selesai. (Status: ✅ Selesai)
- **Laporan Kehadiran Detail:** Memantau keaktifan kehadiran anak di kelas (berapa kali hadir, sakit, izin, atau alpa). (Status: ✅ Selesai)
- **Buku Nilai Digital Anak:** Melihat rekap nilai akademik anak secara transparan beserta nilai akhir kalkulasi sistem. (Status: ✅ Selesai)
- **Analisis Minat & Bakat (Student Classification):** Menampilkan analitik rekomendasi minat belajar anak berdasarkan data kecenderungan nilai dan keaktifan. (Status: ✅ Selesai)
- **Konsultasi dengan Wali Kelas/Guru:** Menghubungi guru pengajar untuk berkonsultasi mengenai perkembangan belajar atau kendala anak di sekolah. (Status: ✅ Selesai)

### 🏫 Portal Kepala Sekolah
- **Dashboard Pantau Utama (Executive Monitoring):** Menampilkan rangkuman kesehatan operasional sekolah, seperti grafik kehadiran total, rata-rata nilai seluruh siswa, dan jumlah siswa/guru aktif. (Status: ✅ Selesai)
- **Monitoring Kehadiran Guru:** Memantau secara real-time kedatangan guru, keterlambatan jam mengajar, serta alasan ketidakhadiran guru. (Status: ✅ Selesai)
- **Laporan Statistik Absensi Siswa:** Memantau pergerakan grafik absensi siswa per tingkat kelas untuk bahan evaluasi kebijakan sekolah. (Status: ✅ Selesai)
- **Pengawasan Dokumen Pembelajaran:** Memantau seluruh pengumuman yang aktif di lingkungan sekolah. (Status: ✅ Selesai)

---

## 3. FITUR TERBARU (Update Juni 2026)

Pada periode pengembangan bulan Juni 2026, tim berfokus pada penyelesaian fitur-fitur pemulihan akses (Forgot Password) dan pelaporan data absensi sekolah:

### 🔑 Fitur Lupa Password
Fitur ini dirancang untuk mengatasi kendala hilangnya akses akun bagi pengguna non-admin (Guru, Siswa, Wali Murid, dan Kepala Sekolah) tanpa perlu membebani Administrator untuk melakukan reset manual.
- **Cara Kerja Sederhana:**
  1. Pengguna membuka halaman login dan mengeklik tombol **"Lupa Password"**.
  2. Pengguna memasukkan identitas unik mereka (Email / NIP / NIS) beserta **Password Default Sistem**.
  3. *Password Default Sistem* dibuat menggunakan kombinasi kode khusus yang hanya diketahui oleh pengguna yang bersangkutan (Rumus: Inisial Peran + Kode Login Unik Pengguna). Contoh: Untuk siswa dengan kode login `123456`, password default-nya adalah `S123456`.
  4. Jika kombinasi cocok, sistem akan langsung meminta pengguna untuk mengetikkan password baru mereka.
  5. Setelah tersimpan, pengguna dapat langsung login menggunakan password baru tersebut.
- **Pentingnya untuk Keamanan:** Fitur ini mencegah kebocoran akun karena verifikasi didasarkan pada kecocokan kode login unik yang hanya dipegang oleh pengguna tersebut, sekaligus menghemat waktu administrasi sekolah.

### 📊 Ekspor Absensi ke Excel
Fitur ini ditambahkan untuk mempermudah pelaporan absensi kelas ke pihak manajemen sekolah dan Dinas Pendidikan.
- **Fungsi Sederhana:** Guru dan Staf Admin dapat mengunduh seluruh rekap kehadiran siswa dan guru dalam bentuk file Excel (`.xlsx`) hanya dengan satu klik.
- **Format Khusus Sekolah Maleo:** Format tabel Excel yang dihasilkan telah disesuaikan dengan standar administrasi Sekolah Maleo (mencakup kolom Nomor, Nama Siswa, NIS, Kelas, Tanggal, Status Kehadiran, dan Catatan Khusus).
- **Fleksibilitas Laporan:** File ekspor dapat disaring berdasarkan kriteria tertentu, seperti per kelas, per tanggal tertentu, atau rekapitulasi bulanan. Fitur ini berlaku bagi pencatatan absensi siswa maupun kehadiran harian guru.

---

## 4. ARSITEKTUR SISTEM

Untuk memudahkan pemahaman, sistem SIAKAD Maleo dibangun menggunakan arsitektur tiga lapis yang dapat dianalogikan seperti operasional sebuah restoran modern:

```
┌─────────────────────────────────────────────────────────────┐
│                    TAMPILAN WEBSITE (Frontend)              │
│       "Pelayan Restoran" — Next.js & React                  │
│  - Menerima pesanan/input dari pengguna                     │
│  - Menampilkan menu, materi, nilai, dan dashboard           │
└──────────────────────────────┬──────────────────────────────┘
                               │
                      Mengirim Permintaan
                               │
┌──────────────────────────────▼──────────────────────────────┐
│                    OTAK SISTEM (Backend)                    │
│       "Dapur/Koki Utama" — Express.js & Node.js             │
│  - Memproses logika (menghitung nilai akhir, verifikasi)    │
│  - Memeriksa hak akses dan keamanan pengguna                │
└──────────────────────────────┬──────────────────────────────┘
                               │
                     Mengambil/Menyimpan Data
                               │
┌──────────────────────────────▼──────────────────────────────┐
│                  DATABASE / TEMPAT PENYIMPANAN              │
│       "Gudang Bahan Makanan" — PostgreSQL & Prisma          │
│  - Menyimpan data siswa, guru, absensi, dan nilai secara rapi│
└─────────────────────────────────────────────────────────────┘
```

1. **Tampilan Website (Frontend - Next.js):** Bertindak sebagai "Pelayan Restoran". Bagian ini adalah apa yang dilihat dan digunakan oleh pengguna di layar komputer atau HP. Tugasnya adalah menerima input dari pengguna (seperti mengetik nilai atau absensi) lalu menampilkannya kembali dalam bentuk tabel atau grafik yang cantik.
2. **Otak Sistem (Backend - Express.js):** Bertindak sebagai "Koki di Dapur". Bagian ini bekerja di balik layar, menerima permintaan dari tampilan website, lalu memprosesnya secara cerdas (misalnya: menghitung nilai akhir dengan rumus bobot atau memverifikasi apakah password yang dimasukkan benar).
3. **Tempat Penyimpanan Data (Database - PostgreSQL & Prisma):** Bertindak sebagai "Gudang Penyimpanan". Di sinilah seluruh data sekolah disimpan dengan sangat rapi dan aman, mulai dari data diri siswa, daftar nilai, hingga riwayat absensi dari tahun ke tahun.

---

## 5. PENGGUNA & HAK AKSES

Pembagian peran dan batasan akses diatur secara ketat melalui sistem keamanan aplikasi (*middleware*) untuk memastikan privasi data tetap terjaga:

| Peran (Role) | Siapa Penggunanya | Apa yang Bisa Dilakukan (Hak Akses) | Apa yang Tidak Bisa Dilakukan (Batasan) |
| :--- | :--- | :--- | :--- |
| **Admin** | Staf Tata Usaha / Operator Sekolah | - Mengelola data master guru, siswa, wali murid, dan kepala sekolah.<br>- Menyusun jadwal pelajaran dan tahun ajaran aktif.<br>- Mengatur komposisi bobot nilai sekolah.<br>- Membuat pengumuman umum sekolah. | - Mengunggah materi pelajaran ke kelas.<br>- Mengisi/mengubah nilai siswa secara langsung (ini hak guru pengampu).<br>- Melakukan presensi mandiri guru. |
| **Kepala Sekolah** | Kepala Sekolah / Pimpinan | - Memantau dashboard statistik operasional sekolah secara menyeluruh.<br>- Memantau rekap absensi guru dan absensi siswa.<br>- Melihat daftar pengumuman aktif. | - Menambah/mengubah/menghapus data guru, siswa, kelas, maupun mata pelajaran.<br>- Melakukan pengisian nilai kelas.<br>- Melakukan konfigurasi teknis sistem. |
| **Guru** | Tenaga Pendidik / Wali Kelas | - Menyusun rencana pembelajaran (RPS) mingguan.<br>- Mengunggah materi ajar (PDF, PPT, Tautan).<br>- Membuat tugas kelas.<br>- Melakukan absensi siswa di kelas.<br>- Memasukkan dan mengunci nilai siswa.<br>- Melakukan absensi kehadiran mandiri.<br>- Berkonsultasi dengan wali murid. | - Mengubah data administrasi sekolah di luar kelasnya.<br>- Mengubah setelan dasar bobot nilai sekolah.<br>- Mengubah nilai siswa yang statusnya sudah dikonfirmasi dan dikunci. |
| **Siswa** | Peserta Didik / Murid | - Melihat jadwal pelajaran sendiri.<br>- Mengunduh materi belajar yang dibagikan guru.<br>- Melihat daftar tugas dan mengumpulkan tugas.<br>- Memantau riwayat absensi kehadiran sendiri.<br>- Melihat nilai akhir rapor mereka. | - Mengubah atau mengedit data apa pun di dalam sistem.<br>- Menginput absensi sendiri.<br>- Menginput nilai pelajaran.<br>- Mengakses portal administrator atau kepala sekolah. |
| **Wali Murid** | Orang Tua Siswa | - Memantau grafik nilai dan perkembangan belajar anak.<br>- Melihat riwayat absensi anak (kehadiran harian).<br>- Melihat tugas sekolah anak yang belum dikerjakan.<br>- Berkonsultasi dengan wali kelas/guru pengampu. | - Mengubah data nilai atau absensi anak.<br>- Mengakses materi pelajaran atau mengumpulkan tugas.<br>- Melihat data siswa lain (menjaga privasi). |

---

## 6. DATA YANG SUDAH MASUK SISTEM

Sistem SIAKAD Maleo telah diisi dengan data awal (*seed data*) melalui basis data untuk keperluan simulasi operasional sekolah dengan struktur sebagai berikut:
- **Tahun Ajaran:** Telah dikonfigurasi Tahun Ajaran Aktif **2025/2026** untuk **Semester Ganjil** yang berjalan dari tanggal **14 Juli 2025** hingga **19 Desember 2025**.
- **Mata Pelajaran:** 10 Mata Pelajaran utama tingkat dasar telah terdaftar di dalam sistem, meliputi:
  1. Pendidikan Agama dan Budi Pekerti (PAI)
  2. Pendidikan Pancasila (PPKN)
  3. Bahasa Indonesia (BIN)
  4. Matematika (MAT)
  5. Ilmu Pengetahuan Alam (IPA)
  6. Ilmu Pengetahuan Sosial (IPS)
  7. Bahasa Inggris (BING)
  8. Pendidikan Jasmani, Olahraga, dan Kesehatan (PJOK)
  9. Informatika (TIK)
  10. Seni Budaya dan Prakarya (SBD)
- **Kelas & Penjadwalan KBM:** Struktur data telah siap untuk memproses pendaftaran kelas dinamis di tingkat kelas 10, 11, dan 12. Staf Admin dapat mengalokasikan guru pengampu ke masing-masing kelas secara langsung melalui dashboard admin.
- **Akun Utama:** Akun administrator utama telah siap dengan alamat email `admin@maleo.sch.id` untuk pengelolaan awal sistem.

---

## 7. YANG SEDANG DIKERJAKAN / BELUM SELESAI

Dalam fase sprint terakhir ini, tim sedang berfokus untuk menyempurnakan beberapa aspek laporan dan visualisasi data:
1. **Ekspor Rapor Digital (Format PDF):** Pengembangan fitur untuk menghasilkan rapor akhir semester siswa dalam format PDF siap cetak, yang mencakup rangkuman nilai akademis, kehadiran, serta catatan perkembangan sikap. (Status: 🔄 Dalam Penyempurnaan)
2. **Visualisasi Analitik Performa Kelas:** Pembuatan grafik tren perkembangan nilai rata-rata kelas bagi Guru dan Kepala Sekolah untuk mempermudah evaluasi hasil belajar. (Status: 🔄 Dalam Penyempurnaan)
3. **Penyempurnaan Antarmuka Responsif:** Penyesuaian beberapa halaman dashboard di perangkat HP agar lebih nyaman digunakan oleh Wali Murid saat mengakses sistem melalui smartphone. (Status: 🔄 Dalam Penyempurnaan)

---

## 8. TEKNOLOGI YANG DIGUNAKAN

Sistem SIAKAD Maleo dibangun menggunakan kombinasi teknologi modern untuk menjamin performa cepat, aman, dan mudah dikembangkan di masa depan:
- **Next.js:** Kerangka kerja (*framework*) modern untuk membangun tampilan website. Next.js membuat website terasa sangat cepat saat dibuka dan mempermudah pengaturan rute halaman.
- **Express.js:** Sistem pengolah data yang bekerja di server (backend). Bertugas menerima setiap permintaan dari website, memproses logika bisnis sekolah, dan mengirimkannya kembali dengan aman.
- **PostgreSQL:** Sistem basis data relasional tingkat industri yang digunakan untuk menyimpan seluruh data sekolah secara permanen dengan tingkat keamanan dan keandalan data yang tinggi.
- **Prisma:** Alat bantu (*ORM*) yang menjembatani bahasa pemrograman dengan basis data. Prisma membantu developer menulis perintah database secara aman tanpa takut terjadi salah ketik struktur data.
- **TypeScript:** Bahasa pemrograman utama yang digunakan. TypeScript membantu mendeteksi kesalahan penulisan kode lebih awal sebelum program dijalankan, sehingga meminimalisir adanya *bug* atau eror pada sistem.

---

## 9. KESIMPULAN

Secara keseluruhan, proyek pengembangan SIAKAD Maleo telah mencapai tingkat penyelesaian sebesar **90%**. Seluruh fitur utama yang mencakup manajemen data sekolah, rencana pembelajaran (RPS), absensi digital, pengumpulan tugas, dan input nilai dengan penguncian keamanan telah selesai dibangun dan berfungsi dengan baik.

Sistem saat ini sudah siap untuk diuji coba secara terbatas untuk operasional harian sekolah. Komitmen tim pengembang pada sisa sprint ini adalah menyelesaikan fitur cetak rapor PDF dan pemolesan tampilan mobile untuk memastikan aplikasi ini siap digunakan secara penuh pada tahun ajaran baru. Tim optimis target penyelesaian 100% dapat dicapai tepat waktu sesuai dengan jadwal yang telah direncanakan.
