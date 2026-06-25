# Digital Divide Indonesia: Analisis Statistik Hubungan Kemiskinan dan Akses Internet Antarprovinsi

Studi statistik terapan yang menelusuri kesenjangan digital di Indonesia tahun 2024, menggunakan korelasi Pearson, regresi linear dengan uji asumsi klasik penuh, dan uji chi-square dengan Cramer's V serta Fisher's Exact Test.

## Daftar Isi

1. [Ringkasan Proyek](#ringkasan-proyek)
2. [Masalah Utama](#masalah-utama)
3. [Tujuan](#tujuan)
4. [Sumber Data](#sumber-data)
5. [Tools dan Teknologi](#tools-dan-teknologi)
6. [Metodologi](#metodologi)
7. [Workflow](#workflow)
8. [Hasil dan Temuan Utama](#hasil-dan-temuan-utama)
9. [Cara Menjalankan](#cara-menjalankan)
10. [Keterbatasan](#keterbatasan)
---

## Ringkasan Proyek

Proyek ini menguji apakah tingkat kemiskinan di suatu provinsi berasosiasi dengan tingkat akses internet rumah tangga di provinsi tersebut, menggunakan data resmi BPS (Badan Pusat Statistik) tahun 2024 untuk 37 provinsi di Indonesia. Analisis dilakukan dalam enam tahap berurutan, yaitu: pengumpulan data, pembersihan dan penggabungan dataset, eksplorasi data, kategorisasi variabel, pemodelan regresi dengan uji asumsi klasik, dan uji chi-square dengan effect size serta uji koreksi saat asumsi statistik tidak terpenuhi.

Yang membedakan proyek ini dari analisis korelasi sederhana adalah konsistensi metodologis. Setiap kali asumsi statistik gagal terpenuhi (homoskedastisitas, frekuensi harapan minimum pada tabel kontingensi), proyek ini tidak mengabaikannya, melainkan beralih ke metode yang lebih tepat (Fisher's Exact Test) atau secara eksplisit mencatatnya sebagai keterbatasan model.

## Masalah Utama

Akses internet sering diasumsikan sebagai indikator kesetaraan digital, tetapi distribusinya di Indonesia diduga tidak merata dan berkorelasi dengan kondisi sosial-ekonomi wilayah. Pertanyaan yang ingin dijawab:

- Apakah ada hubungan statistik yang signifikan antara tingkat kemiskinan provinsi dan tingkat akses internet rumah tangga di provinsi tersebut?
- Jika ada, seberapa kuat hubungan tersebut, dan apakah hasilnya konsisten ketika diuji dengan pendekatan numerik (regresi/korelasi) maupun pendekatan kategorik (chi-square)?
- Provinsi mana yang paling menyimpang dari pola umum, dan apa implikasinya terhadap kesenjangan digital nasional?

## Tujuan

- Mengukur kekuatan dan arah hubungan antara persentase kemiskinan dan persentase akses internet menggunakan korelasi Pearson dan regresi linear sederhana.
- Memvalidasi hubungan tersebut dari sudut pandang data kategorik menggunakan uji chi-square, Cramer's V, dan Fisher's Exact Test.
- Mengidentifikasi provinsi-provinsi yang menjadi pendorong utama (driver) dari signifikansi hubungan tersebut.
- Menyajikan seluruh hasil dengan transparansi penuh terhadap pelanggaran asumsi statistik, bukan hanya melaporkan hasil yang signifikan.

## Sumber Data

Dua dataset resmi dari BPS (Badan Pusat Statistik Republik Indonesia), tahun 2024:

| Dataset | Deskripsi | Dimensi Awal | Link |
|---|---|---|---|
| Akses Internet | Persentase rumah tangga yang pernah mengakses internet dalam 3 bulan terakhir, menurut provinsi dan klasifikasi daerah (perkotaan/perdesaan/total) | 39 baris x 4 kolom | [BPS - Akses Internet](https://www.bps.go.id/id/statistics-table/2/Mzk4IzI=/persentase-rumah-tangga-yang-pernah-mengakses-internet-dalam-3-bulan-terakhir-menurut-provinsi-dan-klasifikasi-daerah.html) |
| Kemiskinan | Persentase penduduk miskin (P0) menurut provinsi dan daerah | 39 baris x 10 kolom | [BPS - Penduduk Miskin](https://www.bps.go.id/id/statistics-table/2/MTkyIzI=/persentase-penduduk-miskin--p0--menurut-provinsi-dan-daerah.html) |

Catatan pengambilan data:
- Dari dataset kemiskinan, kolom yang digunakan adalah periode Maret 2024 (Semester I), dipilih untuk konsistensi temporal dengan periode survei akses internet.
- File CSV mentah tidak disertakan langsung di repository ini. Unduh dari kedua link di atas, lalu letakkan di `data/raw/`. Disarankan mengganti nama file menjadi lebih ringkas, misalnya `internet_access_2024.csv` dan `poverty_rate_2024.csv`, dan menyesuaikan path pada notebook.

## Tools dan Teknologi

- Python 3
- pandas, numpy (manipulasi data)
- matplotlib, seaborn (visualisasi)
- scipy.stats (uji statistik: Pearson, Shapiro-Wilk, chi-square, Fisher's Exact Test, dan lainnya)
- scikit-learn (LinearRegression, r2_score, untuk keperluan inferensial)
- Google Colab (lingkungan pengembangan asli)

## Metodologi

Pendekatan statistik dipilih berdasarkan jenis data dan kebutuhan validasi silang:

1. **Korelasi Pearson** untuk mengukur kekuatan dan arah hubungan linear antara dua variabel numerik kontinu.
2. **Regresi linear sederhana (OLS)** untuk mengukur seberapa besar variasi akses internet dapat dijelaskan oleh tingkat kemiskinan, dilengkapi uji signifikansi model (F-test) dan koefisien (t-test).
3. **Uji asumsi klasik regresi**: normalitas residual (Shapiro-Wilk), homoskedastisitas (Breusch-Pagan), dan autokorelasi (Durbin-Watson), untuk memastikan validitas interpretasi model, bukan sekadar melaporkan R².
4. **Kategorisasi berbasis kuartil** untuk mengubah kedua variabel numerik menjadi kategori Rendah/Sedang/Tinggi, sebagai dasar analisis kategorik.
5. **Uji chi-square independensi** dan **Cramer's V** untuk memvalidasi hubungan dari sudut pandang kategorik, sebagai pembanding independen terhadap hasil regresi.
6. **Fisher's Exact Test** digunakan sebagai pengganti chi-square pada tabel 2x2 ketika asumsi frekuensi harapan minimum (≥5) tidak terpenuhi pada mayoritas sel tabel kontingensi.
7. **Post-hoc pairwise comparison dengan koreksi Bonferroni** untuk mengidentifikasi pasangan kategori mana yang signifikan secara individual setelah uji chi-square keseluruhan signifikan.
8. **Goodness-of-fit test** untuk memeriksa apakah distribusi kategori kemiskinan dan akses internet merata atau tidak secara independen dari hubungan antar keduanya.

## Workflow

Notebook mengikuti enam bagian berurutan, dapat dijalankan dari atas ke bawah tanpa loncatan:

1. **Pengumpulan Data**: import library, unggah dua file CSV, pemeriksaan dimensi awal.
2. **Pembersihan dan Transformasi Data**: rename kolom, hapus baris agregat nasional ("INDONESIA"), standardisasi nama provinsi, konversi tipe data, penggabungan dataset (inner join), penanganan missing value, pemeriksaan duplikasi dan outlier (IQR method).
3. **Eksplorasi Data**: statistik deskriptif, analisis univariate dan bivariate, identifikasi provinsi top/bottom, deteksi outlier (Z-score).
4. **Pendefinisian dan Kategorisasi Variabel**: penentuan variabel dependen/independen, kategorisasi kuartil, crosstabulation, visualisasi distribusi kategori.
5. **Pemodelan Prediktif dan Uji Korelasi**: korelasi Pearson, regresi linear, uji F dan t, uji asumsi klasik, visualisasi diagnostik.
6. **Uji Chi-Square**: uji independensi, Cramer's V, Fisher's Exact Test, post-hoc pairwise comparison, goodness-of-fit test, perbandingan hasil kategorik vs numerik.

## Hasil dan Temuan Utama

### Ringkasan Data

Setelah pembersihan dan penggabungan, dataset akhir berisi **37 provinsi** dengan variabel utama Internet_Total (persentase akses internet) dan Kemiskinan_Persen (persentase penduduk miskin). Satu provinsi (DKI Jakarta) dihapus karena missing value pada kolom Internet_Perdesaan (kurang dari 5% dari total data).

| Variabel | Mean | Median | Std Dev | Rentang |
|---|---|---|---|---|
| Akses Internet (%) | 85.95 | 90.17 | 16.09 | 12.15 - 97.57 |
| Kemiskinan (%) | 11.33 | 10.47 | 6.74 | 4.00 - 32.97 |

<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/ea500da9-f791-4c35-a150-e90a1c51d008" />

### Hubungan Kemiskinan dan Akses Internet

| Metode | Statistik | p-value | Kesimpulan |
|---|---|---|---|
| Korelasi Pearson | r = -0.8215 (R² = 0.6748) | p < 0.001 | Hubungan negatif sangat kuat dan signifikan |
| Regresi Linear | F = 72.62, t(slope) = -8.52 | p < 0.001 | Model signifikan; kemiskinan menjelaskan 67.48% variasi akses internet |
| Chi-Square Test of Independence | χ² = 20.18, df = 4 | p < 0.001 | Kategori kemiskinan dan akses internet saling berkaitan signifikan |
| Cramer's V | V = 0.5222 | - | Kekuatan asosiasi kategorik: sangat kuat |
| Fisher's Exact Test (2x2) | Odds Ratio = 0 | p = 0.036 | Tidak ada provinsi dengan kemiskinan tinggi sekaligus akses internet tinggi |

Persamaan regresi yang dihasilkan:

```
Internet_Total = 108.16 + (-1.96) x Kemiskinan_Persen
```

Setiap kenaikan 1% tingkat kemiskinan dikaitkan dengan penurunan rata-rata 1.96% akses internet (CI 95%: -2.43 sampai -1.49), valid hanya dalam rentang data yang diamati (4.00% - 32.97%). Intercept (108.16%) berada di luar rentang 0-100% karena hasil ekstrapolasi di luar data observasi, sehingga tidak memiliki makna substantif tersendiri.

<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/99897e03-e1c7-4c1e-8c96-66984171e6b5" />

<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/b68772db-fea0-4154-b5af-bb4277972460" />

### Uji Asumsi Regresi

| Asumsi | Uji | Hasil | Status |
|---|---|---|---|
| Normalitas residual | Shapiro-Wilk | W = 0.9494, p = 0.092 | Terpenuhi |
| Homoskedastisitas | Breusch-Pagan | LM = 20.43, p < 0.001 | Tidak terpenuhi |
| Tidak ada autokorelasi | Durbin-Watson | DW = 1.04 | Tidak terpenuhi (autokorelasi positif) |

<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/2538bc2a-e996-4844-8c0c-2db73259dd6e" />

Dua dari tiga asumsi klasik regresi tidak terpenuhi sepenuhnya. Interpretasi koefisien (arah dan signifikansi) tetap valid, tetapi standard error dan confidence interval yang dilaporkan berpotensi bias akibat heteroskedastisitas dan autokorelasi. Lihat bagian Keterbatasan.

### Kategorisasi dan Analisis Kategorik

Kedua variabel dikategorikan menjadi Rendah/Sedang/Tinggi berdasarkan kuartil (Q1 dan Q3).

<img width="700" height="450" alt="image" src="https://github.com/user-attachments/assets/262cf076-6046-4277-9645-c69db167ed0b" />

<img width="440" height="350" alt="image" src="https://github.com/user-attachments/assets/4bcc40f4-375e-4d83-b007-3f1548e730af" />

Tabel kontingensi menunjukkan 77.8% sel memiliki frekuensi harapan di bawah 5, sehingga chi-square 3x3 dilengkapi dengan Fisher's Exact Test pada tabel 2x2 (kategori digabung menjadi Tinggi vs Rendah-Sedang). Sel dengan kontribusi terbesar terhadap nilai chi-square adalah kombinasi **Kemiskinan Tinggi x Akses Internet Rendah** (observasi 7 provinsi, ekspektasi 2.43 provinsi, kontribusi 42.50% dari total chi-square).

<img width="720" height="570" alt="image" src="https://github.com/user-attachments/assets/894cf4e6-83a9-4f1f-ab7f-0d7ee7ab9ad8" />

<img width="700" height="250" alt="image" src="https://github.com/user-attachments/assets/aa7cc52e-8867-4aab-adfe-d7d8c3f29fbd" />

### Post-Hoc dan Goodness-of-Fit

Post-hoc pairwise comparison (koreksi Bonferroni, alpha terkoreksi = 0.0056) menunjukkan kategori Kemiskinan Tinggi berbeda signifikan dari Rendah dan Sedang, sementara kategori Akses Internet Tinggi berbeda signifikan hanya dari kategori Rendah. Goodness-of-fit test menunjukkan distribusi kategori untuk kedua variabel secara individual tidak berbeda signifikan dari distribusi merata (p = 0.139 untuk Internet, p = 0.062 untuk Kemiskinan), artinya pola yang signifikan murni berasal dari hubungan antar dua variabel, bukan dari ketidakseimbangan distribusi salah satu variabel.

### Provinsi dengan Pola Ekstrem

| Kategori | Provinsi | Nilai |
|---|---|---|
| Akses internet tertinggi | Kep. Riau | 97.57% |
| Akses internet terendah | Papua Pegunungan | 12.15% |
| Kemiskinan tertinggi | Papua Pegunungan | 32.97% |
| Kemiskinan terendah | Bali | 4.00% |

Papua Pegunungan dan Papua Tengah terdeteksi sebagai outlier signifikan (Z-score > 3) pada kedua variabel, dan menjadi pendorong utama kekuatan hubungan yang teramati.

### Kesimpulan Akhir

Tingkat kemiskinan dan akses internet antarprovinsi di Indonesia tahun 2024 berhubungan negatif, kuat, dan signifikan secara statistik, konsisten ditunjukkan baik dari pendekatan numerik (Pearson r, regresi) maupun pendekatan kategorik (chi-square, Cramer's V, Fisher's Exact Test). Tidak ditemukan satu pun provinsi dengan kombinasi kemiskinan tinggi dan akses internet tinggi secara bersamaan, mengindikasikan kesenjangan digital yang nyata antarwilayah, bukan sekadar variasi acak.

## Cara Menjalankan

1. Clone repository:
```bash
git clone https://github.com/<username>/digital-divide-indonesia-2024.git
cd digital-divide-indonesia-2024
```

2. (Opsional) Buat virtual environment:
```bash
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Unduh dua dataset dari link pada bagian [Sumber Data](#sumber-data), lalu letakkan di `data/raw/`.

5. **Penting, catatan reproducibility**: notebook asli dikembangkan di Google Colab dan menggunakan widget unggah file (`google.colab.files.upload()`) pada bagian 1.2. Untuk menjalankan secara lokal (Jupyter/VS Code), ganti sel tersebut dengan pembacaan file langsung dari folder lokal:

```python
df_internet_raw = pd.read_csv('data/raw/internet_access_2024.csv', skiprows=3, encoding='utf-8')
df_kemiskinan_raw = pd.read_csv('data/raw/poverty_rate_2024.csv', skiprows=4, encoding='utf-8')
```

6. Jalankan notebook:
```bash
jupyter notebook notebooks/digital_divide_statistical_analysis.ipynb
```
Eksekusi sel secara berurutan dari atas ke bawah.

## Keterbatasan

- **Dua asumsi regresi klasik tidak terpenuhi** (homoskedastisitas dan non-autokorelasi). Koefisien dan arah hubungan tetap valid secara substantif, tetapi standard error dan p-value yang dilaporkan berpotensi terlalu optimis. Penggunaan robust standard error akan memberikan estimasi ketidakpastian yang lebih akurat.
- **Ukuran sampel kecil** (37 provinsi), sehingga hasil rentan dipengaruhi oleh sedikit provinsi ekstrem (Papua Pegunungan, Papua Tengah terdeteksi sebagai outlier pada kedua variabel).
- **Data cross-sectional satu tahun (2024)**, sehingga tidak dapat digunakan untuk klaim tren atau hubungan kausal, hanya asosiasi pada satu titik waktu.
- **Ambang kategorisasi berbasis kuartil** bersifat statistik, bukan ambang kebijakan resmi, sehingga label Rendah/Sedang/Tinggi relevan untuk analisis ini saja.
- **Notebook bergantung pada Google Colab** untuk input data, sehingga belum dapat dijalankan langsung di luar Colab tanpa modifikasi kecil (lihat bagian Cara Menjalankan).
- **Korelasi tidak menyimpulkan kausalitas**. Hubungan negatif antara kemiskinan dan akses internet kemungkinan dipengaruhi oleh faktor lain yang tidak dimodelkan, seperti ketersediaan infrastruktur, kebijakan, dan kondisi geografis.
