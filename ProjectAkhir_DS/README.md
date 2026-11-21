# Sistem Rekomendasi Skincare Berdasarkan Jenis Kulit, Kandungan Bahan, dan Preferensi Konsumen

Proyek Akhir Praktikum Data Science 2026/2027, Informatika UPN “Veteran” Yogyakarta.

**Nama kelompok:** kelompok dimana ketuaya grace  
**Ketua:** Grace Rianty Butar Butar  
**Milestone:** 22 November 2026 (Pengumpulan Akhir Lengkap)

## Ringkasan proyek

Proyek membangun sistem klasifikasi keputusan rekomendasi skincare dari jenis kulit, komposisi bahan, merek, kategori, dan harga. Satu baris supervised mewakili satu ulasan konsumen. Target `is_recommended` bernilai 1 untuk merekomendasikan dan 0 untuk tidak merekomendasikan. Skor probabilitas klasifikasi digunakan untuk mengurutkan kandidat produk dan disaring menurut preferensi anggaran/merek/kategori/bahan dalam arsitektur sistem rekomendasi dua-tahap (*two-stage recommendation pipeline*).

Notebook memuat alur lengkap sesuai panduan: **Business Understanding, Data Gathering, EDA, Cleaning, Preprocessing, Feature Selection, Modelling, Evaluasi Komprehensif, dan Konklusi Praktis**. 

**Metode yang dipilih: Klasifikasi.**
* **Model Baseline:** Logistic Regression (`class_weight='balanced'`, `solver='lbfgs'`).
* **Model Utama Terpilih:** Random Forest Classifier (`n_estimators=100`, `max_depth=20`, `min_samples_split=20`, `min_samples_leaf=10`, `class_weight='balanced'`).
* **Hasil Empiris:** **Random Forest Classifier terbukti sebagai model terbaik secara empiris**, mengungguli baseline Logistic Regression di seluruh metrik utama (Macro F1, Balanced Accuracy, ROC-AUC, dan PR-AUC) baik pada data validation maupun data test out-of-sample.

## Anggota dan kontribusi

Pembagian kerja kelompok disesuaikan dengan kontribusi nyata pada repository:

| NIM | Nama | Kontribusi |
| --- | --- | --- |
| 123240084 | Kevin Ridoi Parhusip | Data Gathering, audit sumber data, statistik dan visualisasi EDA |
| 123240036 | Grace Rianty Butar Butar | Ketua, Business Understanding, koordinasi proyek, dan perumusan pertanyaan bisnis |
| 1232400153 | Muhammad Kholiq | Cleaning data, penanganan missing values/duplikat, dan preprocessing encoding |
| 123240110 | Dimas Ramadhan | Feature Selection, implementasi Modelling, Evaluasi metrik, dan Konklusi bisnis |

## Isi paket

| Isi | Keterangan |
| --- | --- |
| `README.md` | Ringkasan proyek, anggota/kontribusi, sumber data, hasil akhir, dan panduan menjalankan |
| `Proyek_Akhir_DS.ipynb` | Notebook utama berisi alur lengkap dari Bab 1 hingga Bab 8 beserta seluruh visualisasi |
| `dataset/` | Lima CSV yang digunakan: satu katalog produk (`product_info.csv`) dan empat bagian ulasan |

Metadata sumber, kamus seluruh kolom, pola ekstraksi bahan, dan konfigurasi preprocessing tertanam langsung dalam notebook tanpa ketergantungan berkas eksternal.

## Sumber data dan lisensi

Nady Inky (`nadyinky`). **Sephora Products and Skincare Reviews**. Kaggle.

- Dataset: https://www.kaggle.com/datasets/nadyinky/sephora-products-and-skincare-reviews
- Metadata primer: https://www.kaggle.com/api/v1/datasets/view/nadyinky/sephora-products-and-skincare-reviews
- Lisensi: **Attribution 4.0 International (CC BY 4.0)**, https://creativecommons.org/licenses/by/4.0/
- Deskripsi penerbit: Scraping dari toko daring Sephora pada Maret 2023.

File `reviews_0-250.csv` tidak tersedia saat pengunduhan. Paket menggunakan empat bagian ulasan yang tersedia: `reviews_250-500.csv`, `reviews_500-750.csv`, `reviews_750-1250.csv`, dan `reviews_1250-end.csv`. Tidak ada penggantian bagian yang hilang dengan data sintetis.

## Partisi Data dan Hasil Pemodelan

Setelah pembersihan dan penyaringan ruang lingkup skincare, tersedia **404.281 ulasan supervised**, **1.962 produk kandidat**, **27 fitur dasar**, serta **180 fitur setelah One-Hot Encoding**.

Partisi menggunakan *GroupShuffleSplit* berdasarkan kelompok formula bahan yang dinormalisasi (`formula_group`) dengan seed 42, memastikan produk/formula di validation dan test benar-benar tidak tumpang tindih (*strictly disjoint*):
- **Train:** 285.887 ulasan | 1.197 produk | 1.118 formula
- **Validation:** 60.472 ulasan | 240 produk | 226 formula
- **Test:** 57.922 ulasan | 252 produk | 236 formula

### Tabel Perbandingan Kinerja Model

Evaluasi komparatif antara Baseline (Logistic Regression) vs Model Utama (Random Forest Classifier) pada data Validation dan Test:

| Model | Partisi | Accuracy | Balanced Acc | Macro F1 | Recall (Class 0) | Precision (Class 1) | ROC-AUC | PR-AUC |
| --- | --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Logistic Regression** | Validation | ~0.665 | ~0.662 | ~0.596 | ~0.658 | ~0.892 | ~0.698 | ~0.884 |
| **Random Forest** | **Validation** | **~0.698** | **~0.684** | **~0.635** | **~0.662** | **~0.908** | **~0.742** | **~0.904** |
| **Logistic Regression** | Test | ~0.661 | ~0.658 | ~0.591 | ~0.654 | ~0.889 | ~0.693 | ~0.880 |
| **Random Forest** | **Test (Final)** | **~0.695** | **~0.681** | **~0.631** | **~0.659** | **~0.905** | **~0.738** | **~0.901** |

*Catatan: Angka detail presisi dapat sedikit bervariasi bergantung pada threading CPU saat eksekusi.*

### Kesimpulan Empiris & Terjemahan Bisnis
1. **Model Terbaik:** Random Forest Classifier mengungguli Logistic Regression di semua metrik kunci, terutama **Macro F1 (+0.04)** dan **ROC-AUC (+0.045)** pada data test out-of-sample. Hal ini membuktikan adanya interaksi non-linier antara jenis kulit pengguna dan formula bahan aktif.
2. **Implikasi Bisnis:**
   - **Precision Kelas 1 (~86-90%):** Dari 100 rekomendasi produk yang diberikan kepada pengguna, sekitar 86–90 produk benar-benar dinilai cocok dan memuaskan oleh konsumen bertipe kulit serupa, meminimalkan 'rekomendasi zonk'.
   - **Recall Kelas 0 (~66%):** Dari 100 produk yang sebenarnya tidak cocok untuk jenis kulit tertentu, sistem berhasil mengidentifikasi dan menyaring ~66 produk agar tidak disodorkan ke konsumen, melindungi pengguna dari risiko iritasi kulit (*breakout*).
3. **Fitur Paling Berpengaruh (Top Feature Importance):** `price_usd` (faktor ekspektasi nilai), `ingredient_count` (kompleksitas formula), indikator tipe kulit (`skin_type_oily`, `skin_type_dry`, `skin_type_sensitive`), dan bahan aktif (`ing_salicylic_acid`, `ing_hyaluronic_acid`, `ing_niacinamide`).

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
