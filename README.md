# Data Cleansing & Data Enrichment: Dataset Kelulusan Mahasiswa

Tugas Studi Kasus Mata Kuliah **Data Mining** (Pertemuan 4)
**Nama:** Dio Raditya Putra Pratama | **NIM:** 2318044 | **Kampus:** Institut Teknologi Nasional Malang

## Deskripsi Project
Project ini membersihkan (*data cleansing*) dan memperkaya (*data enrichment*) dataset kelulusan mahasiswa menggunakan Python di Google Colab. Dataset berisi 500 data mahasiswa dengan 13 kolom, mencakup data akademik (IPK, nilai UTS/UAS, kehadiran, tugas), kebiasaan belajar, aktivitas di luar kuliah, dan status kelulusan (Tepat Waktu / Terlambat).

## Struktur Repository
```
├── 2318044DataCleansing.ipynb            # notebook utama
├── dataset_kelulusan_mahasiswa.csv       # dataset asli
├── dataset_kelulusan_mahasiswa_clean.csv # dataset hasil cleansing & enrichment
└── README.md
```

## Masalah Kualitas Data yang Ditemukan
| Masalah | Temuan |
|---|---|
| Missing values | 37 sel: `ipk` (10), `kehadiran_persen` (12), `jam_belajar_per_minggu` (15) |
| Outlier (IQR) | 1-5 nilai per kolom numerik |
| Duplikat | Tidak ditemukan |
| Inkonsistensi format kategori | Tidak ditemukan |
| Nilai di luar rentang valid | Tidak ditemukan |

## Teknik Data Cleansing
1. **Standardisasi format**: merapikan nama kolom dan menghapus spasi berlebih pada teks.
2. **Penanganan missing values**: imputasi dengan median per jurusan. Median dipilih karena tahan terhadap outlier, dan `status_kelulusan` tidak dipakai agar tidak terjadi data leakage.
3. **Penanganan outlier**: *capping* dengan metode IQR (batas disesuaikan dengan batas logis, misalnya maksimal 100 untuk kehadiran dan nilai). Nilai tidak dihapus agar jumlah data tetap utuh.
4. **Perbaikan tipe data**: kolom nilai dan tugas menjadi integer, kolom kategorikal menjadi `category`.

## Teknik Data Enrichment
Delapan fitur turunan ditambahkan dari kolom yang sudah ada:

| Fitur baru | Keterangan |
|---|---|
| `rata_nilai_ujian` | Rata-rata nilai UTS dan UAS |
| `selisih_uas_uts` | Perubahan nilai dari UTS ke UAS |
| `kategori_ipk` | Cukup / Baik / Sangat Baik |
| `kategori_kehadiran` | Rendah / Sedang / Tinggi |
| `rasio_tugas` | Tugas terkumpul dibanding jumlah maksimum |
| `jumlah_aktivitas_luar` | Organisasi + kerja paruh waktu (0-2) |
| `tingkat_studi` | Awal (Sem 1-4) / Akhir (Sem 5-8) |
| `target_tepat_waktu` | Encoding target: 1 = Tepat Waktu |

## Hasil Sebelum vs Sesudah
| | Sebelum | Sesudah |
|---|---|---|
| Jumlah baris | 500 | 500 |
| Jumlah kolom | 13 | 21 |
| Total missing value | 37 | 0 |
| Baris duplikat | 0 | 0 |

## Insight Utama
- Sekitar 71,8% mahasiswa (359 dari 500) lulus tepat waktu.
- Persentase lulus tepat waktu naik tajam seiring kategori IPK: 10,1% (Cukup), 84,0% (Baik), 97,3% (Sangat Baik).
- Kehadiran juga berpengaruh besar: 34,5% (Rendah), 82,0% (Sedang), 99,1% (Tinggi).
- Mahasiswa dengan lebih banyak aktivitas di luar kuliah cenderung sedikit lebih rendah persentase lulus tepat waktunya (75,4% untuk 0 aktivitas, 67,9% untuk 2 aktivitas), tetapi selisihnya jauh lebih kecil dibanding pengaruh IPK dan kehadiran.

## Cara Menjalankan
1. Buka `2318044DataCleansing.ipynb` di Google Colab.
2. Upload `dataset_kelulusan_mahasiswa.csv` ke Colab (ikon folder di sidebar kiri).
3. Jalankan semua sel (**Runtime → Run all**).

**Link Google Colab:** [isi link Colab di sini]

## Tools
Python, Pandas, NumPy, Matplotlib, Google Colab
