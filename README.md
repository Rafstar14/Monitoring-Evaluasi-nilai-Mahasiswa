# Sistem Monitoring dan Evaluasi Nilai Akademik Mahasiswa (C++)

Repositori ini berisi *source code* program C++ berbasis *console* (CLI) untuk studi kasus mata kuliah **Algoritma dan Pemrograman Dasar**. 

Program ini dibuat untuk mengelola data nilai mahasiswa dari 3 kelas yang berbeda (`TI-1A`, `TI-1B`, dan `TI-1C`). Fokus utama dari kode ini adalah pemanfaatan **Array 1 Dimensi, 2 Dimensi, dan 3 Dimensi** secara bersamaan, pembuatan fungsi modular, serta implementasi algoritma pengurutan manual (*sorting*) tanpa bantuan fungsi `std::sort` bawaan C++.

---

## Bedah Isi Kode (`main.cpp`)

Seluruh logika program berada di dalam satu file `main.cpp` yang dibagi menjadi tiga bagian utama: **Deklarasi Struktur Array**, **Fungsi Pembantu (Helper)**, dan **Fungsi Fitur (Menu 1–9)**.

### 1. Struktur Array yang Digunakan
Di dalam fungsi `main()`, data mahasiswa dibagi ke dalam 3 jenis array sesuai aturan studi kasus:

* **Array 1 Dimensi (1D)**
  * `string namaKelas[3]` : Menyimpan nama kelas (`TI-1A`, `TI-1B`, `TI-1C`).
  * `double bobot[4]` : Menyimpan bobot nilai (`0.20` untuk Tugas, `0.20` Kuis, `0.25` UTS, dan `0.35` UAS).
  * `string namaKomponen[4]` : Menyimpan teks label komponen (`Tugas`, `Kuis`, `UTS`, `UAS`) supaya mudah dipanggil saat mencetak laporan.
* **Array 2 Dimensi (2D)**
  * `string nama[3][5]` : Menyimpan nama 5 mahasiswa untuk masing-masing dari 3 kelas.
  * `double nilaiAkhir[3][5]` : Menyimpan hasil hitungan nilai akhir tiap mahasiswa.
* **Array 3 Dimensi (3D)**
  * `int nilai[3][5][4]` : Menyimpan nilai mentah setiap komponen.
  * Cara baca indeksnya: `nilai[indeks_kelas][indeks_mahasiswa][indeks_komponen]`. Misalnya `nilai[0][2][3]` berarti mengambil data kelas `TI-1A` (indeks 0), mahasiswa ke-3 yaitu `Citra` (indeks 2), untuk nilai `UAS` (indeks 3).

### 2. Fungsi Pembantu (Helper Functions)
Sebelum masuk ke fungsi menu utama, ada beberapa fungsi kecil di bagian atas kode untuk menangani perhitungan dasar dan mencegah *bug*:

* `hitungSemuaNilaiAkhir()` : Mengalikan nilai Tugas, Kuis, UTS, dan UAS di array 3D dengan masing-masing bobot di array 1D, lalu menyimpan hasilnya ke array 2D `nilaiAkhir`. Fungsi ini dipanggil saat program pertama kali jalan dan setiap kali ada perubahan nilai.
* `tentukanGrade()` & `tentukanStatus()` : Mengubah angka nilai akhir menjadi huruf mutu (`A`, `B`, `C`, `D`, `E`) dan menentukan apakah mahasiswa `LULUS` (nilai $\ge 65$) atau `TIDAK LULUS` (nilai $< 65$).
* `keHurufKecil()` : Mengubah semua huruf kapital pada `string` menjadi huruf kecil secara manual lewat kode ASCII. Fungsi ini dipakai agar fitur pencarian nama tidak peduli huruf besar/kecil (*case-insensitive*) dan untuk pengurutan abjad yang konsisten.
* `bersihkanInput()` : Menggunakan `cin.clear()` dan `cin.ignore()` untuk mencegah program mengalami *infinite loop* kalau pengguna tidak sengaja mengetik huruf saat diminta memasukkan angka menu.
* `pilihKelas()` : Menampilkan pilihan kelas 1 sampai 3 sekaligus memvalidasi inputnya sebelum dipakai oleh menu lain.

---

## Penjelasan Fitur di Dalam Program

Program memiliki 9 menu utama yang masing-masing dijalankan oleh fungsinya sendiri:

### 1. Tampilkan Seluruh Data Mahasiswa (`tampilkanSeluruhData`)
Menampilkan tabel nilai mentah (Tugas, Kuis, UTS, UAS) dari seluruh kelas. Di dalam kode, fungsi ini menggunakan *nested loop* 3 tingkat (`for` untuk kelas, mahasiswa, dan komponen nilai) serta `setw()` dari library `<iomanip>` supaya kolom tabelnya rapi.

### 2. Tampilkan Nilai Berdasarkan Kelas (`tampilkanDataPerKelas`)
Mirip dengan fitur pertama, tetapi pengguna memilih satu kelas terlebih dahulu. Kalau pengguna memasukkan nomor kelas di luar 1–3, program akan menolak dan kembali ke menu.

### 3. Hitung dan Tampilkan Nilai Akhir (`tampilkanNilaiAkhir`)
Menampilkan laporan nilai akhir seluruh mahasiswa per kelas lengkap dengan Grade dan Status Kelulusannya. Angka desimal diformat menjadi 2 angka di belakang koma menggunakan `fixed` dan `setprecision(2)`.

### 4. Cari Mahasiswa (`cariMahasiswa`)
Mencari data mahasiswa berdasarkan nama di seluruh kelas. Karena dibantu fungsi `keHurufKecil()`, pengguna bebas mengetik `"andi"`, `"ANDI"`, atau `"AnDi"`. Jika ketemu, semua rincian nilai komponen, nilai akhir, grade, dan status kelulusannya akan ditampilkan.

### 5. Ubah Nilai Mahasiswa (`ubahNilaiMahasiswa`)
Mengizinkan pengguna mengedit salah satu nilai komponen (Tugas/Kuis/UTS/UAS) milik mahasiswa tertentu. 
* Nilai baru divalidasi harus berada di rentang `0` sampai `100`.
* Begitu nilai baru disimpan ke array `nilai[k][m][c]`, program langsung memanggil `hitungSemuaNilaiAkhir()` lagi. Hasilnya, peringkat dan statistik di menu lain otomatis ikut berubah mengikuti data terbaru.

### 6. Tampilkan Peringkat Mahasiswa (`tampilkanPeringkatKelas`)
Menampilkan ranking 1 sampai 5 di kelas yang dipilih dari nilai akhir tertinggi ke terendah.
* **Kenapa disalin ke array lokal?** Supaya urutan asli mahasiswa di array utama tidak berantakan setelah di-sort, nama dan nilai akhir disalin dulu ke variabel sementara `urutNama` dan `urutNilai`.
* **Algoritma Sorting Manual:** Menggunakan **Bubble Sort** buatan sendiri (tanpa `std::sort`). Jika ada dua mahasiswa dengan nilai akhir yang persis sama, kode akan membandingkan nama mereka dan mengurutkannya sesuai abjad (A–Z).

### 7. Analisis Statistik Per Kelas (`analisisStatistikPerKelas`)
Menghitung statistik lengkap untuk satu kelas yang dipilih:
* Mencari nilai akhir tertinggi dan terendah beserta nama mahasiswanya (jika ada lebih dari satu orang dengan nilai tertinggi/terendah yang sama, semuanya ditampilkan).
* Menghitung rata-rata nilai akhir kelas serta jumlah mahasiswa yang lulus dan tidak lulus.
* Menghitung rata-rata untuk masing-masing komponen (Tugas, Kuis, UTS, UAS) dan mencari komponen mana yang rata-ratanya paling anjlok di kelas tersebut.

### 8. Analisis Seluruh Kelas (`analisisSeluruhKelas`)
Melakukan rekapitulasi gabungan untuk ketiga kelas sekaligus:
* Menampilkan rata-rata nilai akhir tiap kelas dan menentukan kelas mana yang rata-ratanya paling tinggi.
* Menghitung total mahasiswa yang lulus, tidak lulus, serta persentase kelulusan dari total 15 mahasiswa.
* Mencari mahasiswa dengan nilai tertinggi dan terendah dari seluruh angkatan.

### 9. Tampilkan Mahasiswa Remedial (`tampilkanMahasiswaRemedial`)
Menyaring siapa saja mahasiswa dari seluruh kelas yang nilai akhirnya di bawah `65.00`.
* Daftar mahasiswa yang masuk remedial diurutkan menggunakan *Bubble Sort* mulai dari nilai akhir yang **paling rendah ke tertinggi**.
* Program juga mengecek ke array 3D untuk mencari komponen nilai apa yang paling rendah dari mahasiswa tersebut (jika ada 2 komponen yang nilainya sama-sama paling rendah, keduanya akan ditampilkan).

---

## Cara Kompilasi dan Menjalankan

Pastikan sudah menginstal compiler C++ seperti `g++` (GCC / MinGW). Buka terminal di folder proyek ini, lalu jalankan perintah berikut:

**Linux / macOS:**
```bash
g++ main.cpp -o monitoring_nilai
./monitoring_nilai
```

**Windows (CMD / PowerShell):**
```powershell
g++ main.cpp -o monitoring_nilai.exe
.\monitoring_nilai.exe
```
