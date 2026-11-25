# Penilaian Kesehatan Finansial Menggunakan Metode Simple Additive Weighting (SAW)

Program berbasis antarmuka teks (*command line*) yang dikembangkan dengan bahasa Python untuk menilai kondisi kesehatan finansial pengguna berdasarkan rasio tabungan dan rasio pembayaran utang terhadap pendapatan. Hasil penilaian dikelompokkan ke dalam empat level dan diperingkatkan menggunakan metode **Simple Additive Weighting (SAW)**.

---

## Daftar Isi

1. [Latar Belakang](#latar-belakang)
2. [Tujuan](#tujuan)
3. [Fitur Utama](#fitur-utama)
4. [Metodologi](#metodologi)
5. [Struktur Repositori](#struktur-repositori)
6. [Persyaratan Sistem](#persyaratan-sistem)
7. [Cara Menjalankan Program](#cara-menjalankan-program)
8. [Panduan Penggunaan](#panduan-penggunaan)
9. [Format Data](#format-data)
10. [Keterbatasan](#keterbatasan)
11. [Tim Pengembang](#tim-pengembang)

---

## Latar Belakang

Pengelolaan keuangan pribadi yang baik ditandai oleh kemampuan menabung yang memadai serta beban utang yang terkendali. Namun, banyak individu belum memiliki tolok ukur yang jelas untuk menilai posisi keuangannya. Program ini menyediakan penilaian kuantitatif yang sederhana dan terukur dengan memanfaatkan dua indikator utama, yaitu rasio tabungan dan rasio pembayaran utang, serta menerapkan metode SAW untuk membandingkan kondisi finansial antarpengguna.

## Tujuan

- Menghitung rasio tabungan dan rasio pembayaran utang terhadap pendapatan tahunan pengguna.
- Mengklasifikasikan kondisi finansial pengguna ke dalam level A hingga D.
- Memeringkatkan pengguna secara global maupun per kelompok menggunakan metode SAW.
- Memberikan rekomendasi tindakan yang terukur agar pengguna dapat meningkatkan level finansialnya.

## Fitur Utama

| No | Fitur | Keterangan |
|----|-------|------------|
| 1 | Pendaftaran dan *login* akun | Pengguna mendaftar dan masuk dengan *username* dan *password*. |
| 2 | Perhitungan rasio dan penentuan level | Menghitung rasio tabungan serta rasio utang, kemudian menentukan level A–D. |
| 3 | Perangkingan metode SAW | Peringkat global (seluruh pengguna) dan peringkat per kelompok. |
| 4 | Rekomendasi tindakan | Nominal tabungan yang perlu ditambah atau utang yang perlu dikurangi, disertai saran sesuai kelompok. |
| 5 | Data contoh | Data contoh dimuat otomatis dari berkas `data_contoh_reddit.csv` agar perangkingan dapat langsung diuji. |

Kelompok pengguna yang tersedia: **Mahasiswa**, **Pekerja**, **Wirausaha**, dan **Lainnya**.

## Metodologi

### 1. Rasio Keuangan

Seluruh nilai dimasukkan per tahun dalam satuan rupiah (bilangan bulat).

```
Rasio Tabungan = Tabungan / Pendapatan
Rasio Utang    = Pembayaran Utang / Pendapatan
```

Untuk kelompok Mahasiswa, pendapatan diisi dengan uang saku per tahun.

### 2. Penentuan Level

Level ditentukan berdasarkan ambang batas berikut.

| Indikator | Ambang Batas | Kategori |
|-----------|--------------|----------|
| Rasio Tabungan | ≥ 10% | Tinggi |
| Rasio Utang | ≥ 20% | Tinggi |

| Level | Kondisi | Keterangan |
|-------|---------|------------|
| **A** | Tabungan tinggi, utang rendah | Terbaik |
| **B** | Tabungan rendah, utang rendah | Baik |
| **C** | Tabungan tinggi, utang tinggi | Berisiko |
| **D** | Tabungan rendah, utang tinggi | Terburuk |

### 3. Perangkingan dengan Metode SAW

Dua kriteria digunakan dengan bobot yang sama.

| Kriteria | Atribut | Bobot |
|----------|---------|-------|
| Rasio Tabungan | *Benefit* (semakin tinggi semakin baik) | 0,5 |
| Rasio Utang | *Cost* (semakin rendah semakin baik) | 0,5 |

**Normalisasi:**

```
R-Tabungan = Rasio Tabungan / Rasio Tabungan tertinggi

R-Utang    = Rasio Utang terendah (selain 0) / Rasio Utang
             (bernilai 1 apabila Rasio Utang = 0)
```

**Skor akhir:**

```
Skor = (0,5 × R-Tabungan) + (0,5 × R-Utang)
```

Pengguna diurutkan dari skor tertinggi ke terendah. Apabila skor sama, urutan ditentukan oleh level (A lebih unggul daripada D). Apabila satu pengguna menginput data lebih dari sekali, data terbaru yang digunakan.

## Struktur Repositori

```
.
├── Program Kesehatan Finansial Menggunakan Metode SAW.py   # Program utama
├── data_contoh_reddit.csv                                   # Data contoh yang dimuat otomatis saat program berjalan
├── reddit_finance.csv                                       # Dataset keuangan berukuran lebih besar (sekitar 2.000 baris)
└── README.md                                                # Dokumentasi
```

Berkas berikut dibuat otomatis oleh program saat pertama kali dijalankan:

| Berkas | Fungsi |
|--------|--------|
| `akun.csv` | Menyimpan data akun pengguna (*username* dan *password*). |
| `data_penilaian.csv` | Menyimpan riwayat data keuangan dan hasil penilaian. |

## Persyaratan Sistem

- Python versi 3.6 atau yang lebih baru.
- Tidak memerlukan pustaka tambahan (hanya menggunakan pustaka standar: `csv` dan `os`).

## Cara Menjalankan Program

1. Kloning repositori:

   ```bash
   git clone https://github.com/ppresciouzz/Kesehatan-Finansial-Menggunakan-Metode-SAW-Final.git
   cd Kesehatan-Finansial-Menggunakan-Metode-SAW-Final
   ```

2. Jalankan program:

   ```bash
   python "Program Kesehatan Finansial Menggunakan Metode SAW.py"
   ```

   Pada sebagian sistem, gunakan perintah `python3`.

> **Catatan:** Jalankan program dari dalam direktori repositori agar berkas `data_contoh_reddit.csv` dapat terbaca dan berkas `akun.csv` serta `data_penilaian.csv` dibuat pada lokasi yang sesuai.

## Panduan Penggunaan

1. **Daftar akun.** Pilih menu *Daftar akun*. *Username* minimal 3 karakter dan *password* minimal 4 karakter.
2. **Login.** Masuk menggunakan akun yang telah dibuat.
3. **Menu Utama:**
   - **Menu 1 – Hitung rasio.** Pilih kelompok, lalu masukkan pendapatan, tabungan, dan pembayaran utang per tahun. Program menampilkan rasio dan level, kemudian menyimpan hasilnya.
   - **Menu 2 – Perangkingan.** Lihat peringkat global atau peringkat per kelompok beserta posisi Anda.
   - **Menu 3 – Rekomendasi.** Lihat langkah yang disarankan untuk mempertahankan atau meningkatkan level, berikut nominal target tabungan atau batas utang.
   - **Menu 4 – Logout.**

Menu 3 memerlukan data yang telah diisi pada Menu 1. Menu 2 tetap dapat dibuka tanpa data pribadi, tetapi posisi Anda baru ditampilkan setelah Menu 1 diisi.

## Format Data

Berkas `data_contoh_reddit.csv` menggunakan format berikut.

| Kolom | Keterangan |
|-------|------------|
| `username` | Nama pengguna (unik). |
| `kelompok` | Salah satu dari: Mahasiswa, Pekerja, Wirausaha, Lainnya. |
| `pendapatan` | Pendapatan per tahun (bilangan bulat, lebih dari 0). |
| `tabungan` | Jumlah pendapatan yang ditabung per tahun. |
| `utang` | Total pembayaran utang per tahun. |

Contoh:

```csv
username,kelompok,pendapatan,tabungan,utang
reddit_490,Lainnya,490000,150000,0
reddit_347,Pekerja,170000,100000,0
```

## Keterbatasan

- Penyimpanan data menggunakan berkas CSV sehingga belum sesuai untuk penggunaan skala besar atau serentak oleh banyak pengguna.
- *Password* disimpan sebagai teks biasa (*plain text*) dan belum dienkripsi. Program ini ditujukan untuk keperluan pembelajaran dan tidak disarankan untuk data sensitif yang sebenarnya.
- Ambang batas (10% tabungan dan 20% utang) serta bobot kriteria (0,5 dan 0,5) bersifat tetap dan dapat diubah melalui konstanta pada bagian awal program.
- Penilaian hanya menggunakan dua kriteria dan belum mempertimbangkan faktor lain seperti pengeluaran, aset, dan dana darurat.

## Tim Pengembang

**Kelompok 8A**

| No | Nama | NIM |
|----|------|-----|
| 1 | Sagiv Albova Masri | 2610931017 |
| 2 | Kaila Nazwa Putri Rizani | 2610933042 |
| 3 | Felania Mutri Sari | 2610932008 |
