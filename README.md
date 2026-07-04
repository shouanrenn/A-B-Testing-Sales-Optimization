# A/B Testing & Sales Optimization

[![Buka di Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/shouanrenn/A-B-Testing-Sales-Optimization/blob/main/AB_Testing_Sales_Optimization.ipynb)

## Deskripsi Proyek
Proyek ini bertujuan untuk menganalisis efektivitas sebuah strategi/intervensi bisnis terhadap peningkatan penjualan melalui metode **A/B Testing**. Selain melakukan analisis kausal dengan uji statistik, proyek ini juga mengimplementasikan pemodelan **Machine Learning** menggunakan *Random Forest Classifier* untuk memprediksi probabilitas pembelian pelanggan (`Purchase_Made`).

## Struktur Direktori
- `AB_Testing_Sales_Optimization.ipynb`: Jupyter Notebook utama yang berisi seluruh kode mulai dari persiapan data, EDA, pengujian hipotesis, hingga pembuatan model ML.
- `sales.csv`: Dataset yang digunakan untuk analisis (berisi metrik penjualan dan kepuasan sebelum dan sesudah intervensi pada grup *Control* dan *Treatment*).
- `.gitignore`: Mengabaikan file sistem dan folder environment (seperti `.venv`).

## Dataset (`sales.csv`)
Dataset berisi informasi pelanggan dengan kolom utama:
* `Group`: Menunjukkan apakah pelanggan berada di grup **Control** atau **Treatment**.
* `Customer_Segment`: Segmentasi pelanggan (misal: *High Value*).
* `Sales_Before` & `Sales_After`: Angka penjualan sebelum dan sesudah eksperimen.
* `Customer_Satisfaction_Before` & `Customer_Satisfaction_After`: Skor kepuasan pelanggan.
* `Purchase_Made`: Target klasifikasi yang menunjukkan apakah konversi pembelian terjadi (Yes/No).

## Tahapan Analisis
1. **Persiapan Dataset & Cleansing**: Memuat data dan menangani *missing values*.
2. **Exploratory Data Analysis (EDA)**: Mengeksplorasi distribusi dan karakteristik data.
3. **Uji Statistik A/B Testing**: Menggunakan *T-Test* (`scipy.stats.ttest_ind`) untuk membandingkan rata-rata `Sales_After` antara grup Control dan Treatment guna mendapatkan *P-Value* dan kesimpulan bisnis.
4. **Machine Learning (Klasifikasi)**: 
   - *Feature Engineering* dan pengkodean kategori.
   - Melatih model `RandomForestClassifier` untuk memprediksi konversi.
   - Evaluasi menggunakan skor **ROC-AUC**.
5. **Visualisasi**: Membuat *Confusion Matrix* dan *Heatmap Korelasi* untuk mengidentifikasi hubungan antar fitur.

## Teknologi yang Digunakan
* **Python**
* **Pandas & NumPy** (Manipulasi data)
* **SciPy** (Uji statistik T-Test)
* **Scikit-Learn** (Machine learning & metrik evaluasi)
* **Matplotlib & Seaborn** (Visualisasi)

## Cara Menjalankan Melalui Google Colab
Proyek ini sangat mudah dijalankan langsung melalui browser menggunakan **Google Colab**.
1. Klik tombol **Buka di Colab** yang ada di bagian paling atas halaman ini.
2. Anda akan diarahkan langsung ke Google Colab.
3. Klik **Runtime** > **Run all** untuk mengeksekusi seluruh sel kode dari awal hingga akhir.

## Hasil Analisis & Rekomendasi Bisnis
Berdasarkan pengolahan 10.000 baris data pelanggan, berikut adalah temuan utama yang dihasilkan dari eksperimen dan pemodelan:

### 1. Hasil A/B Testing
* **Perbandingan Pendapatan:** Terdapat perbedaan yang sangat nyata pada *Sales_After* (setelah intervensi). Grup **Control** tanpa kampanye hanya menghasilkan rata-rata penjualan sebesar **253.62**, sedangkan grup **Treatment** diberikan kampanye melonjak hingga **314.73**.
* **Signifikansi Statistik:** Uji T-Test menghasilkan *P-Value* sebesar **0.0000**. 
* **Rekomendasi Bisnis:** Intervensi/kampanye bisnis terbukti **sangat efektif** meningkatkan penjualan (peningkatan sekitar ~24%). Perusahaan direkomendasikan untuk melakukan *roll-out* (menerapkan strategi ini) secara permanen dan luas ke seluruh pelanggan karena ROI (*Return on Investment*) sudah tervalidasi oleh data.

### 2. Kinerja Prediksi Perilaku Konsumen
Model *Random Forest Classifier* dilatih untuk memprediksi probabilitas seorang pelanggan melakukan konversi pembelian (`Purchase_Made`) menggunakan data sebelum eksperimen (`Sales_Before`, `Customer_Satisfaction_Before`, Segment, dan Grup).
* **Evaluasi Model:** Model menghasilkan skor akurasi (F1-Score) sebesar **0.51** dan nilai **ROC-AUC** sebesar **0.5042**.
* **Insight Analitik:** Skor ROC-AUC di angka 0.50 menunjukkan bahwa performa model saat ini setara dengan menebak secara acak (*random guessing*). Fitur-fitur metrik historis yang digunakan belum cukup untuk menebak secara akurat apakah pelanggan akan berbelanja.
* **Rekomendasi Bisnis:** Tim pemasaran **belum bisa** bergantung pada algoritma *Machine Learning* ini untuk menargetkan pelanggan. Disarankan untuk mengumpulkan fitur lain yang lebih kaya, seperti demografi, durasi kunjungan, atau riwayat produk spesifik, sebelum mengimplementasikan model kecerdasan buatan ke sistem *live*.

### 3. Integritas Kualitas Data
* **Temuan:** Terdapat volume data kosong (*missing values*) yang cukup signifikan sebelum data dibersihkan. (Contoh: `Customer_Segment` hilang ~1.966 baris, `Customer_Satisfaction` hilang lebih dari 1.600 baris dari total 10.000 baris dataset).
* **Rekomendasi Bisnis:** Meskipun analisis ini telah diatasi dengan imputasi statistik menggunakan *median* dan modus, tim operasional/data *engineering* perlu melakukan audit terhadap sistem *tracking* atau proses pencatatan data. Data yang hilang secara massal dapat menyebabkan bias dalam pengambilan keputusan jangka panjang.
