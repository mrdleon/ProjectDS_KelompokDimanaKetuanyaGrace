# Sistem Rekomendasi Skincare Berdasarkan Jenis Kulit, Kandungan Bahan, dan Preferensi Konsumen

Proyek Akhir Praktikum Data Science 2026/2027, Informatika UPN “Veteran” Yogyakarta.

**Nama kelompok:** kelompok dimana ketuaya grace  
**Ketua:** Grace Rianty Butar Butar  
**Milestone:** 30 Oktober 2026.

## Ringkasan proyek

Proyek menyiapkan data dan rancangan klasifikasi keputusan rekomendasi skincare dari jenis kulit, komposisi bahan, merek, kategori, dan harga. Satu baris supervised mewakili satu ulasan. Target `is_recommended` bernilai 1 untuk merekomendasikan dan 0 untuk tidak merekomendasikan. Pada tahap berikutnya, skor klasifikasi direncanakan untuk mengurutkan kandidat produk dan disaring menurut preferensi anggaran/merek/kategori/bahan.

Notebook memuat Business Understanding, Data Gathering, EDA, Cleaning, Preprocessing, Feature Selection, dan alasan pemilihan model. Kelima belas sel kode telah dijalankan dan enam grafik beserta interpretasinya tertanam dalam notebook.

**Metode dipilih: Klasifikasi. Model utama pilihan awal: Random Forest Classifier.** Model dipilih untuk menangkap interaksi nonlinier jenis kulit dengan karakteristik produk. Tidak ada classifier yang dibuat, dilatih, atau dievaluasi pada milestone ini. Sebutan model terbaik secara empiris baru dapat ditetapkan setelah evaluasi. Cocokkan pilihan algoritma dengan materi praktikum yang benar-benar diajarkan karena daftar materi belum dilampirkan.

## Anggota dan kontribusi

Pembagian berikut merupakan rancangan kontribusi. Anggota perlu menyesuaikannya dengan pekerjaan nyata dan commit masing-masing sebelum pengumpulan akhir.

| NIM | Nama | Kontribusi yang dialokasikan |
| --- | --- | --- |
| 123240084 | Kevin Ridoi Parhusip | Data Gathering, audit sumber, statistik dan visual EDA |
| 123240036 | Grace Rianty Butar Butar | Ketua, Business Understanding, koordinasi dan pemeriksaan panduan |
| 1232400153 | Muhammad Kholiq | Cleaning, audit kualitas, dan preprocessing |
| 123240110 | Dimas Ramadhan | Feature Selection, alasan pemilihan model, dokumentasi dan reproduksi |

Nama dan NIM disalin sesuai informasi yang diberikan. Verifikasi NIM `1232400153` dengan identitas akademik sebelum pengumpulan.

## Isi paket

| Isi | Keterangan |
| --- | --- |
| `README.md` | Ringkasan, anggota/kontribusi, sumber data, dan panduan menjalankan |
| `Proyek_Akhir_DS_Grace_30_Oktober_2026.ipynb` | Notebook utama dengan seluruh hasil eksekusi |
| `dataset/` | Lima CSV yang benar-benar digunakan: satu katalog dan empat bagian ulasan |

Metadata sumber, kamus seluruh kolom, pola bahan, dan checksum acuan ditanam langsung dalam notebook. Notebook tidak bergantung pada berkas pendukung di luar isi paket di atas. Hasil EDA dan preprocessing ditampilkan di notebook dan tersedia sebagai variabel di memori setelah dijalankan.

## Sumber data dan lisensi

Nady Inky (`nadyinky`). **Sephora Products and Skincare Reviews**. Kaggle.

- Dataset: https://www.kaggle.com/datasets/nadyinky/sephora-products-and-skincare-reviews
- Metadata primer: https://www.kaggle.com/api/v1/datasets/view/nadyinky/sephora-products-and-skincare-reviews
- Lisensi yang dinyatakan metadata: **Attribution 4.0 International (CC BY 4.0)**, https://creativecommons.org/licenses/by/4.0/
- Metadata diverifikasi 30 Oktober 2026. Deskripsi penerbit menyebut scraping dari toko daring Sephora pada Maret 2023.
- Scraper yang ditautkan penerbit: https://github.com/nadyinky/sephora-analysis/tree/main/sephora_scraper

Katalog produk disertakan tanpa perubahan byte. CSV ulasan merupakan proyeksi dari data asli: hanya `product_id`, `skin_type`, `rating`, `is_recommended`, dan nomor posisi `source_row` untuk audit. Identitas penulis, teks/judul ulasan, tanggal, serta atribut penampilan lain tidak dimuat atau dibagikan. Tidak ada ulasan yang dibuat, digabung, atau dihapus pada tahap proyeksi kolom. Angka yang sama pada empat atribut tidak otomatis dianggap duplikat ulasan. Rating hanya digunakan untuk EDA, bukan prediktor atau pengganti target kosong.

File `reviews_0-250.csv` tidak tersedia saat pengerjaan. Paket memakai empat bagian ulasan yang tersedia: `reviews_250-500.csv`, `reviews_500-750.csv`, `reviews_750-1250.csv`, dan `reviews_1250-end.csv`. Tidak ada penggantian bagian yang hilang dengan data sintetis. Atribusi tidak menyatakan dukungan Sephora atau penerbit terhadap proyek ini.

## Hasil aktual

Katalog memuat **8.494 produk**, sedangkan empat bagian ulasan memuat **492.281 ulasan asli**. Setelah seleksi ruang lingkup, kelayakan bahan, dan target, tersedia **404.281 ulasan supervised**, **1.962 produk kandidat**, **27 fitur dasar**, serta **180 fitur setelah encoding**.

| Partisi setelah cleaning | Ulasan | Produk unik | Kelompok formula |
| --- | ---: | ---: | ---: |
| Train | 285.887 | 1.197 | 1.118 |
| Validation | 60.472 | 240 | 226 |
| Test | 57.922 | 252 | 236 |

Partisi menggunakan kelompok formula yang dinormalisasi dengan seed 42. EDA hubungan dengan target hanya menggunakan train. Encoder dan median juga hanya dipelajari dari train. Tidak ada overlap produk/grup formula antarpartisi. Pembagian ini tidak menguji orang baru karena identitas penulis tidak dipakai. Skor rekomendasi bukan bukti medis atau jaminan kecocokan individu. Harga dan komposisi mengikuti snapshot sumber, bukan kondisi produk saat ini.

## Menjalankan di komputer lokal

1. Ekstrak ZIP. Pastikan README, notebook, dan folder `dataset/` berada pada folder yang sama.
2. Buka terminal pada folder tersebut, lalu pasang dependensi yang digunakan saat keluaran dibuat. Python 3.12 digunakan pada eksekusi yang tersimpan.

```bash
python -m pip install pandas==2.2.3 numpy==2.3.5 scipy==1.17.0 scikit-learn==1.8.0 matplotlib==3.10.8 seaborn==0.13.2
```

3. Buka `.ipynb` melalui Jupyter, VS Code dengan dukungan notebook, atau Google Colab. Gunakan folder yang memuat README sebagai working directory.
4. Jalankan seluruh sel dari atas ke bawah. Keluaran yang disimpan juga sudah dapat dibaca tanpa menjalankan ulang. Tidak diperlukan file requirements atau skrip tambahan.

## Menjalankan di Google Colab

1. Buka file `.ipynb` melalui Colab.
2. Unggah ZIP paket pada panel Files, kemudian jalankan sel persiapan berikut sebelum sel notebook utama.

```python
from pathlib import Path
from zipfile import ZipFile
import os

with ZipFile('/content/Project_Akhir_DS_Grace_30_Oktober_2026.zip') as archive:
    archive.extractall('/content/proyek_ds')
os.chdir('/content/proyek_ds')
```

3. Jalankan semua sel notebook berurutan. Bila dependensi lingkungan berbeda dan menimbulkan error, gunakan versi paket yang dicantumkan di atas lalu mulai ulang runtime.
4. Simpan notebook setelah eksekusi agar output tetap terlihat saat dikumpulkan.

## Saat sumber diubah atau dilengkapi

Folder `dataset/` adalah satu-satunya sumber data yang dibaca notebook. Jika menambahkan bagian yang belum tersedia, loader hanya mengambil kolom ulasan yang relevan dan membuat `source_row` jika belum ada. Untuk mengganti CSV yang memiliki checksum acuan, periksa perubahan datanya dan perbarui checksum pada sel Data Gathering. Setelah sumber berubah, jalankan semua sel dan perbarui paragraf Markdown yang memuat angka serta ringkasan README agar sesuai keluaran baru. Jangan mempertahankan angka lama sebagai hasil data baru.

## Pengumpulan

Unggah README, notebook dengan output, dan folder dataset ke repository GitHub kelompok, lalu kumpulkan tautannya melalui Spada. Cocokkan pencatatan kelompok/dataset dan algoritma dengan ketentuan kelas. Tahap modelling, evaluasi, serta konklusi akhir dilanjutkan untuk tenggat 22 November 2026. Deployment tidak diperlukan menurut panduan.
