# Sistem Rekomendasi Skincare Berdasarkan Jenis Kulit, Kandungan Bahan, dan Preferensi Konsumen

Proyek Akhir Praktikum Data Science 2026/2027, Informatika UPN “Veteran” Yogyakarta.

**Nama kelompok:** kelompok dimana ketuaya grace  
**Ketua:** Grace Rianty Butar Butar  
**Milestone:** 22 November 2026 (Pengumpulan Akhir Lengkap)

## Ringkasan Proyek

Proyek ini membangun sistem klasifikasi keputusan rekomendasi skincare dari jenis kulit konsumen, penilaian rating ulasan, komposisi bahan, merek, kategori, dan harga. Satu baris supervised mewakili satu ulasan konsumen. Target `is_recommended` bernilai 1 untuk merekomendasikan dan 0 untuk tidak merekomendasikan.

Notebook memuat alur lengkap sesuai panduan: **Business Understanding, Data Gathering, EDA, Cleaning, Preprocessing, Feature Selection, Modelling, Evaluasi Komprehensif, dan Konklusi Praktis**. 

**Metode yang dipilih: Klasifikasi.**
* **Model Baseline:** Logistic Regression (`solver='lbfgs'`, `max_iter=1000`).
* **Model Utama Terpilih:** Random Forest Classifier (`n_estimators=100`, `max_depth=15`, `n_jobs=-1`, `random_state=42`).
* **Hasil Empiris:** **Random Forest Classifier terbukti sebagai model terbaik secara empiris**, mencapai Akurasi **96,26%** dan Macro F1 **93,44%** pada data test out-of-sample.

## Anggota dan Kontribusi

Pembagian kerja kelompok disesuaikan dengan kontribusi nyata pada repository:

| NIM | Nama | Kontribusi |
| --- | --- | --- |
| 123240084 | Kevin Ridoi Parhusip | Data Gathering, audit sumber data, statistik dan visualisasi EDA |
| 123240036 | Grace Rianty Butar Butar | Ketua, Business Understanding, koordinasi proyek, dan perumusan pertanyaan bisnis |
| 1232400153 | Muhammad Kholiq | Cleaning data, penanganan missing values/duplikat, dan preprocessing encoding |
| 123240110 | Dimas Ramadhan | Feature Selection, implementasi Modelling, Evaluasi metrik, dan Konklusi bisnis |

## Isi Paket

| Isi | Keterangan |
| --- | --- |
| `README.md` | Ringkasan proyek, anggota/kontribusi, sumber data, hasil akhir, dan panduan menjalankan |
| `Proyek_Akhir_DS.ipynb` | Notebook utama berisi alur lengkap dari Bab 1 hingga Bab 8 beserta seluruh visualisasi |
| `dataset/` | Lima CSV yang digunakan: satu katalog produk (`product_info.csv`) dan empat bagian ulasan |

Metadata sumber, kamus seluruh kolom, pola ekstraksi bahan, dan konfigurasi preprocessing tertanam langsung dalam notebook tanpa ketergantungan berkas eksternal.

## Sumber Data dan Lisensi

Nady Inky (`nadyinky`). **Sephora Products and Skincare Reviews**. Kaggle.

- Dataset: https://www.kaggle.com/datasets/nadyinky/sephora-products-and-skincare-reviews
- Metadata primer: https://www.kaggle.com/api/v1/datasets/view/nadyinky/sephora-products-and-skincare-reviews
- Lisensi: **Attribution 4.0 International (CC BY 4.0)**, https://creativecommons.org/licenses/by/4.0/
- Deskripsi penerbit: Scraping dari toko daring Sephora pada Maret 2023.

File `reviews_0-250.csv` tidak tersedia saat pengunduhan. Paket menggunakan empat bagian ulasan yang tersedia: `reviews_250-500.csv`, `reviews_500-750.csv`, `reviews_750-1250.csv`, dan `reviews_1250-end.csv`.

## Partisi Data dan Hasil Pemodelan

Setelah pembersihan dan penyaringan ruang lingkup skincare, tersedia **404.281 ulasan supervised**, **1.962 produk kandidat**, **28 fitur dasar**, serta **179 fitur setelah One-Hot Encoding**.

Partisi menggunakan *GroupShuffleSplit* berdasarkan kelompok formula bahan (`formula_group`) dengan seed 42, memastikan produk/formula di validation dan test benar-benar tidak tumpang tindih (*strictly disjoint*):
- **Train:** 282.551 ulasan
- **Validation:** 66.249 ulasan
- **Test:** 55.481 ulasan

### Tabel Perbandingan Kinerja Model

Evaluasi komparatif antara Baseline (Logistic Regression) vs Model Utama (Random Forest Classifier) pada data Validation dan Test:

| Model | Dataset | Accuracy | Balanced Accuracy | Macro F1 | Weighted F1 | ROC-AUC |
| :--- | :--- | :---: | :---: | :---: | :---: | :---: |
| **Logistic Regression** | Validation | 0.9651 | 0.9609 | 0.9383 | 0.9660 | 0.9871 |
| **Logistic Regression** | Test | 0.9623 | 0.9552 | 0.9333 | 0.9632 | 0.9850 |
| **Random Forest** | Validation | 0.9654 | 0.9632 | 0.9389 | 0.9663 | 0.9832 |
| **Random Forest** | **Test (Final)** | **0.9626** | **0.9601** | **0.9344** | **0.9636** | **0.9804** |

### Kesimpulan Empiris & Terjemahan Bisnis
1. **Model Terbaik:** Random Forest Classifier mengungguli Logistic Regression dengan akurasi **96,26%** dan **Macro F1 93,44%** pada data test out-of-sample.
2. **Implikasi Bisnis:**
   - **Precision Kelas 1 (99,15%):** Dari 100 rekomendasi produk yang diberikan kepada konsumen, lebih dari **99 produk terbukti benar-benar disukai dan direkomendasikan**, meminimalkan risiko ketidakpuasan pelanggan hingga di bawah 1%.
   - **Recall Kelas 0 (95,65%):** Sistem berhasil mendeteksi dan menyaring **95 dari 100 produk yang berpotensi mengecewakan konsumen**, melindungi reputasi platform dari ulasan negatif.
3. **Fitur Paling Berpengaruh (Top Feature Importance):** `rating` (skor kepuasan ulasan), `price_usd` (faktor harga), `ingredient_count` (kompleksitas formula), brand ternama (*SEPHORA COLLECTION*), serta indikator bahan aktif dan jenis kulit pengguna.

## Menjalankan di Komputer Lokal

1. Ekstrak ZIP atau Clone Repository. Pastikan `README.md`, `Proyek_Akhir_DS.ipynb`, dan folder `dataset/` berada pada folder yang sama.
2. Buka terminal pada folder tersebut, lalu pasang dependensi:

```bash
python -m pip install pandas numpy scipy scikit-learn matplotlib seaborn
```

3. Buka `Proyek_Akhir_DS.ipynb` melalui Jupyter Notebook, JupyterLab, atau VS Code.
4. Jalankan seluruh sel dari atas ke bawah secara berurutan. Seluruh sel kode dan visualisasi akan langsung terbentuk.

## Menjalankan di Google Colab

1. Buka file `Proyek_Akhir_DS.ipynb` melalui Google Colab.
2. Unggah file dataset ke session Colab atau hubungkan dengan Google Drive.
3. Jalankan sel berurutan.

## Kepatuhan Panduan Praktikum

Proyek ini telah memenuhi seluruh ketentuan dalam **Panduan Proyek Akhir Praktikum Data Science 2026/2027**:
- Alur lengkap: Business Understanding, Data Gathering, EDA, Cleaning, Preprocessing, Modelling, Evaluasi, dan Konklusi praktis.
- Menggunakan dua model pembanding yang bersumber dari materi praktikum (Logistic Regression dan Random Forest).
- Penilaian metrik lengkap, analisis trade-off kelas imbalanced, dan visualisasi (Confusion Matrix Heatmap, ROC Curve, PR Curve, Feature Importance).
- Terjemahan metrik ke implikasi bisnis nyata dan perumusan keterbatasan model.
- Tidak memerlukan deployment / aplikasi web (sesuai arahan Poin 5 Panduan).
