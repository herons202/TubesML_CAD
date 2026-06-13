# Analisis Dataset CAD Alizadeh dengan Machine Learning

Projek ini berfokus pada deteksi dini Penyakit Jantung Koroner (Coronary Artery Disease atau CAD) menggunakan teknik Supervised dan Unsupervised Learning berdasarkan dataset klinis CAD Alizadeh. Projek ini mencakup analisis eksplorasi data, penanganan ketidakseimbangan kelas, seleksi fitur ensemble, pemodelan klasifikasi dengan 5 algoritma berbeda, implementasi algoritma K-Means clustering dari awal (pure NumPy), serta pembuatan presentasi otomatis menggunakan Python.

## Deskripsi Dataset

Dataset CAD Alizadeh berisi data klinis dari **303 pasien** dengan **55 fitur**. Fitur-fitur tersebut terbagi menjadi beberapa kelompok:
- **Demografis:** Usia, berat badan, tinggi badan, BMI, jenis kelamin.
- **Klinis dan Riwayat:** Tekanan darah, riwayat diabetes mellitus (DM), hipertensi (HTN), riwayat merokok.
- **Pemeriksaan Laboratorium:** Kolesterol LDL, HDL, trigliserida, hemoglobin, gula darah puasa (FBS), ESR, kreatinin.
- **Kardiologi (Non-Invasif):** Ejection Fraction (EF-TTE), Regional Wall Motion Abnormality (Region RWMA), Valvular Heart Disease (VHD).

Target klasifikasi berada pada kolom `Cath` yang terbagi menjadi dua kelas:
- **CAD (Positif):** 216 pasien (71.3%)
- **Normal (Negatif):** 87 pasien (28.7%)

## Struktur Projek

- `CAD_Analysis.ipynb` / `CADAnalysis.ipynb`: Notebook Jupyter utama yang berisi seluruh alur analisis, preprocessing, feature selection, pelatihan model klasifikasi (supervised), serta clustering (unsupervised).
- `Part2/`:
  - `CAD_ML_Analysis.ipynb`: Notebook Jupyter khusus untuk analisis tambahan atau bagian terpisah.
  - `CAD_alizadeh.xls`: File dataset mentah dalam format Excel.
- `generate_pptx.py`: Script Python untuk mengonversi slide presentasi berformat Markdown menjadi file presentasi PowerPoint (.pptx).
- `slide_presentasi.md`: Dokumen slide presentasi dalam format Markdown.
- `presentasi_cad_alizadeh.pptx`: Hasil generate file presentasi PowerPoint.
- `fig*.png`: Visualisasi dan plot hasil analisis (seperti matriks korelasi, pentingnya fitur, kurva ROC, konvergensi K-Means, dan evaluasi kluster).

## Alur Implementasi

### Bagian 1: Supervised Learning (Klasifikasi)

1. **Preprocessing Data:**
   - Label encoding untuk fitur kategorikal.
   - Penskalaan fitur numerik menggunakan `StandardScaler` (hanya di-fit pada data training untuk menghindari data leakage).
   - Pembagian data dengan rasio 80:20 menggunakan **Stratified Split** untuk mempertahankan proporsi kelas target (CAD vs Normal) pada data training dan testing.

2. **Seleksi Fitur (Feature Selection):**
   Menggunakan pendekatan ensemble dari 3 metode untuk memilih 20 fitur paling diskriminatif:
   - Pearson Correlation
   - Mutual Information (MI)
   - Random Forest Feature Importance

3. **Pemodelan dan Evaluasi:**
   Membandingkan 5 algoritma menggunakan 5-Fold Stratified Cross-Validation:
   - Naive Bayes (Gaussian NB)
   - Decision Tree
   - K-Nearest Neighbors (KNN)
   - Random Forest
   - Support Vector Machine (SVM)

   Hasil akhir pengujian pada data uji menunjukkan bahwa **SVM Linear** adalah model terbaik dengan nilai **AUC-ROC mencapai 0.910**, sedangkan **Random Forest** memberikan akurasi pengujian tertinggi (**85.2%**).

### Bagian 2: Unsupervised Learning (Clustering)

1. **Implementasi K-Means dari Awal:**
   - Ditulis menggunakan pure NumPy (tanpa modul `sklearn.cluster.KMeans`).
   - Menggunakan inisialisasi **K-Means++** untuk mempercepat konvergensi dan menghindari optimum lokal.
   - Dilengkapi penanganan kluster kosong (empty cluster handling).

2. **Pemilihan Jumlah Kluster (k) Optimal:**
   Menggunakan tiga metrik evaluasi:
   - Elbow Method (Inertia/WCSS)
   - Silhouette Coefficient (Skor tertinggi pada k=2 sebesar 0.142)
   - Davies-Bouldin Index (Skor terbaik/terendah pada k=3 sebesar 2.545)

   Dipilih **k=2** sebagai kluster utama karena selaras secara klinis dengan diagnosis CAD vs Normal, serta **k=3** untuk membagi tingkat risiko keparahan pasien (Risiko Sangat Tinggi, Risiko Menengah, Risiko Rendah).

3. **Visualisasi:**
   Menggunakan Reduksi Dimensi PCA (Principal Component Analysis) untuk memproyeksikan kluster ke dalam grafik 2D.

## Persyaratan Sistem dan Pemasangan

Projek ini membutuhkan Python 3.x dan beberapa library pendukung. Anda dapat menginstal semua dependensi dengan perintah berikut:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn xlrd python-pptx
```

## Cara Menjalankan Projek

1. **Menjalankan Analisis:**
   Buka Jupyter Notebook atau JupyterLab, lalu jalankan file `CAD_Analysis.ipynb` untuk melihat seluruh visualisasi dan alur kode dari preprocessing hingga evaluasi model.

2. **Membuat Slide Presentasi:**
   Jalankan script Python berikut untuk memperbarui atau menghasilkan file presentasi PowerPoint secara otomatis berdasarkan isi `slide_presentasi.md`:

   ```bash
   python generate_pptx.py
   ```
