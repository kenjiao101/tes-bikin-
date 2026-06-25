<div align="center">

# Workforce Allocation Optimization & Sensitivity Analysis

### Mengoptimalkan biaya dan alokasi tenaga kerja operasional bike-sharing Seoul melalui pendekatan Mathematical Programming dengan analisis sensitivitas empiris, parametrik, dan trade-off service level

<br>

[![License](https://img.shields.io/badge/license-MIT-3DA639?style=flat-square)](LICENSE)
[![Status](https://img.shields.io/badge/status-completed-blue?style=flat-square)]()
[![Python](https://img.shields.io/badge/python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/jupyter-notebook-F37626?style=flat-square&logo=jupyter&logoColor=white)](notebooks/)
[![PuLP](https://img.shields.io/badge/solver-PuLP%20%2B%20CBC-5C6BC0?style=flat-square)]()
[![Last Commit](https://img.shields.io/github/last-commit/[username]/workforce-scheduling-mip-sensitivity-analysis?style=flat-square)](https://github.com/[username]/workforce-scheduling-mip-sensitivity-analysis/commits)

</div>

---

## 📌 Overview

Proyek ini mengimplementasikan model **Mixed-Integer Programming (MIP)** untuk mengoptimalkan alokasi dan biaya tenaga kerja pada sistem bike-sharing Seoul. Dataset yang digunakan adalah Seoul Bike Sharing Demand dari UCI Repository (8.760 record selama satu tahun penuh, Desember 2017 sampai November 2018), yang diolah menjadi profil demand per jam sebagai input kebutuhan staf. Model diselesaikan menggunakan solver CBC via PuLP, lalu divalidasi dan diuji pada sembilan skenario empiris berbasis musim dan status hari libur, analisis sensitivitas parametrik melalui tornado chart, kurva trade-off biaya terhadap service level, serta diagnostik LP relaxation dengan shadow price. Output akhirnya adalah kerangka keputusan operasional yang secara kuantitatif menunjukkan biaya optimal per skenario, urutan prioritas validasi asumsi, dan batas service level yang sepadan dari sisi penghematan biaya.

---

## ❓ Problem Statement

**Konteks:** Sistem bike-sharing di perkotaan menghadapi tantangan alokasi staf yang tidak mudah karena demand bergerak sangat dinamis, bimodal dalam satu hari, berbeda jauh antarmusim (rasio hingga 4,6 kali antara Summer dan Winter berdasarkan data Seoul 2017-2018), dan dipengaruhi status hari libur. Tanpa kerangka analitik yang terstruktur, operator cenderung memakai pendekatan statis yang menghasilkan overstaffing di periode sepi sekaligus understaffing di jam puncak.

**Gap:** Pendekatan workforce planning yang umum digunakan bergantung pada aturan praktis atau perencanaan manual yang tidak memperhitungkan variasi demand secara sistematis. Tidak ada mekanisme formal untuk mengukur seberapa sensitif biaya terhadap perubahan parameter operasional seperti kapasitas per staf atau target service level, padahal informasi ini krusial untuk pengambilan keputusan kebijakan.

**Solusi:** Proyek ini memformulasikan masalah alokasi staf sebagai shift scheduling Mixed-Integer Program (varian set-covering Dantzig) dengan variabel staf tambahan sebagai katup pengaman kapasitas. Model diuji pada berbagai skenario empiris dan parametrik, menghasilkan tabel dan kurva keputusan yang dapat langsung dipakai sebagai landasan diskusi kebijakan operasional.

---

## 🎥 Demo & Screenshots

> 📓 **Notebook:** [[nbviewer](https://nbviewer.org/github/[username]/workforce-scheduling-mip-sensitivity-analysis/blob/main/notebooks/workforce_allocation_mip_sensitivity_analysis.ipynb)] &nbsp;|&nbsp; [![Colab](https://img.shields.io/badge/Open%20in-Colab-F9AB00?style=flat-square&logo=googlecolab&logoColor=white)](https://colab.research.google.com/github/[username]/workforce-scheduling-mip-sensitivity-analysis/blob/main/notebooks/workforce_allocation_mip_sensitivity_analysis.ipynb)

<br>

| Profil Demand per Jam (Rata-rata Tahunan) | Alokasi Staf Optimal vs Kebutuhan — Baseline |
|:---:|:---:|
| 📷 *PASTE gambar output Section 7.2 di sini* | 📷 *PASTE gambar output Section 11.3 di sini* |
| *Pola bimodal dengan puncak jam 08:00 (~1.050 unit) dan jam 18:00 (~1.554 unit)* | *Distribusi staf reguler (biru) dan staf tambahan (oranye) vs kebutuhan aktual per jam* |

| Tornado Chart — Sensitivitas Parameter | Trade-off Frontier — Biaya vs Service Level |
|:---:|:---:|
| 📷 *PASTE gambar output Section 13.2 di sini* | 📷 *PASTE gambar output Section 14.2 di sini* |
| *Urutan dampak perubahan ±20% per parameter terhadap total biaya operasional* | *Kurva biaya optimal sebagai fungsi target service level, rentang 70%–100%* |

---

## 🛠️ Tech Stack

| Layer | Teknologi | Peran dalam Project |
|:---:|:---:|---|
| Language | Python 3.10+ | Bahasa utama seluruh logika project |
| Environment | Jupyter Notebook | Eksekusi interaktif, narasi analitik, dan visualisasi inline |
| Optimization | PuLP 2.7+ | Formulasi MIP dan penyelesaian menggunakan CBC solver bawaan |
| Data Fetching | ucimlrepo | Akses langsung UCI ML Repository dengan fallback ke CSV lokal |
| Data Processing | Pandas, NumPy | Manipulasi dataframe, agregasi demand, translasi kebutuhan staf |
| Visualization | Matplotlib, Seaborn | Profil demand, staffing chart, tornado chart, trade-off frontier |
| Version Control | Git + GitHub | Source control dan distribusi portofolio |

<details>
<summary>📐 Pipeline Diagram</summary>
<br>

```
UCI Repository API (id=560)                                        EDA & Demand Profiling
Seoul Bike Sharing, 8.760 rows  ---►  Data Preparation  ---►     Hourly, Seasonal,
+ Local CSV Fallback                   Date parsing               Holiday, Heatmap,
                                       Non-op filter              Weekday/Weekend
                                       8.760 --► 8.465 rows
                                                                          |
                                   +------------------------------------------+
                                   |  Demand Derivation                        |
                                   |  required_staff(h) = ceil(demand(h) / 50) |
                                   +------------------------------------------+
                                                                          |
                                   +------------------------------------------+
                                   |  Mixed-Integer Programming (PuLP + CBC)   |
                                   |  min  S cost_reg * x_s  +  S cost_ot * o_h|
                                   |  s.t. coverage >= R_h * alpha, x_s <= 20  |
                                   +------------------------------------------+
                                                  |
          +----------------------------+----------+---------------------------+
          |                            |                                      |
          v                            v                                      v
 Sensitivity Analysis (12-13)   Trade-off Frontier (14)         LP Relaxation (15)
 Empirical: 4 musim + Holiday   Service Level 70%-100%          Shadow price per jam
 Parametric: Tornado Chart +-20% 7-point cost-service mapping   Integrality gap check
```

</details>

---

## 📁 Project Structure

```
workforce-scheduling-mip-sensitivity-analysis/
|
+-- notebooks/
|   +-- workforce_allocation_mip_sensitivity_analysis.ipynb   <- START HERE
|
+-- data/
|   +-- raw/
|       +-- SeoulBikeData.csv       # Fallback lokal jika UCI API tidak tersedia
|
+-- docs/
|   +-- img/                        # Ekspor chart dari notebook, simpan di sini
|       +-- 01_demand_distribution.png
|       +-- 02_hourly_demand_profile.png
|       +-- 03_seasonal_demand_profile.png
|       +-- 04_holiday_vs_noholiday.png
|       +-- 05_heatmap_demand_season.png
|       +-- 06_staffing_baseline.png
|       +-- 07_cost_comparison_scenarios.png
|       +-- 08_tornado_chart.png
|       +-- 09_tradeoff_frontier.png
|       +-- 10_shadow_price_per_hour.png
|
+-- .gitignore
+-- requirements.txt
+-- LICENSE
+-- README.md
```

> **Entry point:** Buka `notebooks/workforce_allocation_mip_sensitivity_analysis.ipynb` dan jalankan sel dari atas ke bawah. Notebook ini self-contained — mulai dari data loading hingga seluruh sensitivity analysis bisa dieksekusi dalam satu sesi tanpa file eksternal tambahan.

---

## 🚀 Getting Started

### Prerequisites

| Requirement | Versi Minimum | Verifikasi |
|---|:---:|---|
| Python | 3.10+ | `python --version` |
| pip | 23.0+ | `pip --version` |
| Git | 2.30+ | `git --version` |

### Installation

```bash
# 1. Clone repository
git clone https://github.com/[username]/workforce-scheduling-mip-sensitivity-analysis.git
cd workforce-scheduling-mip-sensitivity-analysis

# 2. Buat dan aktifkan virtual environment
python -m venv venv

source venv/bin/activate          # macOS / Linux
# venv\Scripts\activate           # Windows (CMD)
# venv\Scripts\Activate.ps1       # Windows (PowerShell)

# 3. Install semua dependensi
pip install -r requirements.txt
```

### Configuration

Tidak ada konfigurasi tambahan yang diperlukan. Project siap dijalankan langsung setelah instalasi.

Dataset diambil otomatis dari UCI Repository saat notebook dieksekusi menggunakan `ucimlrepo`. Jika koneksi API tidak tersedia, notebook secara otomatis beralih ke file lokal `data/raw/SeoulBikeData.csv` — sumber data yang sama, tanpa perubahan logika apapun.

---

## ▶️ Usage

### Menjalankan Notebook

```bash
jupyter notebook
# Buka: notebooks/workforce_allocation_mip_sensitivity_analysis.ipynb
```

Eksekusi sel secara berurutan dari atas ke bawah. Setiap section dilengkapi narasi analitik yang menjelaskan keputusan metodologis sebelum dan sesudah kode dieksekusi.

```
Section  1  -->  Import Library
Section  2  -->  Konfigurasi dan Reproducibility
Section  3  -->  Load Dataset (UCI API + fallback CSV lokal)
Section  4  -->  Data Overview
Section  5  -->  Data Quality Assessment
Section  6  -->  Data Cleaning
Section  7  -->  Exploratory Data Analysis (6 visualisasi)
Section  8  -->  Definisi Parameter Optimasi
Section  9  -->  Derivasi Kebutuhan Staf
Section 10  -->  Formulasi dan Implementasi MIP
Section 11  -->  Validasi Solusi Baseline
Section 12  -->  Sensitivity Analysis: Skenario Empiris (Musim + Holiday)
Section 13  -->  Sensitivity Analysis: Parametrik (Tornado Chart)
Section 14  -->  Trade-off Frontier: Biaya vs Service Level
Section 15  -->  Diagnostic Tambahan: LP Relaxation dan Shadow Price
Section 16  -->  Kesimpulan
Section 17  -->  Catatan dan Temuan Utama
```

> Section 11 sampai 15 dapat dijalankan ulang secara mandiri setelah `solve_model()` didefinisikan di Section 10.

---

## 📊 Dataset

### Metadata

| Atribut | Detail |
|---|---|
| **Sumber** | UCI Machine Learning Repository |
| **Link** | [Seoul Bike Sharing Demand (id=560)](https://archive.ics.uci.edu/dataset/560/seoul+bike+sharing+demand) |
| **Ukuran awal** | 8.760 baris x 14 kolom |
| **Ukuran operasional** | 8.465 baris (295 baris non-operasional difilter) |
| **Format** | CSV |
| **Lisensi** | CC BY 4.0 |
| **Cakupan Waktu** | 2017-12-01 hingga 2018-11-30 (1 tahun penuh) |
| **DOI** | 10.24432/C5F62R |

### Data Dictionary (Kolom Kunci)

| Kolom | Tipe | Deskripsi | Contoh Nilai |
|---|:---:|---|---|
| `date` | `object` | Tanggal observasi | `01/12/2017` |
| `rented_bike_count` | `int64` | **Variabel utama** — jumlah sepeda yang disewa per jam | `254`, `0` |
| `hour` | `int64` | Jam observasi dalam sehari (0–23) | `8`, `18` |
| `temperature` | `float64` | Suhu udara (°C) | `-6.2`, `32.4` |
| `humidity` | `int64` | Kelembapan relatif (%) | `36`, `91` |
| `seasons` | `object` | Musim saat observasi | `Winter`, `Summer` |
| `holiday` | `object` | Status hari libur | `Holiday`, `No Holiday` |
| `functioning_day` | `object` | Status operasional layanan — dipakai sebagai filter awal | `Yes`, `No` |

### Reproduksi Data

```bash
# Dataset diunduh otomatis saat notebook dijalankan — tidak ada langkah manual

# Jika koneksi API tidak tersedia, gunakan file lokal:
# data/raw/SeoulBikeData.csv  (sumber sama: UCI id=560)
```

> **Catatan filter:** 295 baris dengan `functioning_day = No` dan `rented_bike_count = 0` dikeluarkan sebelum analisis karena merepresentasikan jam layanan tutup, bukan demand rendah. Menyertakan baris ini dalam profil demand akan membuat estimasi kebutuhan staf bias ke bawah.

---

## 📈 Results & Performance

### Hasil Optimasi per Skenario

| Skenario | Total Biaya | Staf Reguler | Jam Staf Tambahan |
|---|:---:|:---:|:---:|
| **Baseline** (rata-rata tahunan) | Rp10.512.500 | 41 orang | 77 |
| Winter | Rp3.475.000 | 13 orang | 28 |
| Spring | Rp10.862.500 | 43 orang | 77 |
| Autumn | Rp13.437.500 | 47 orang | 123 |
| Summer | Rp15.150.000 | 48 orang | 160 |
| Holiday | Rp7.562.500 | 34 orang | 33 |
| No Holiday | Rp10.575.000 | 42 orang | 72 |

*"Jam Staf Tambahan" adalah jumlah unit staf tambahan yang dijumlahkan lintas 24 jam dalam satu hari representatif.*

### Trade-off Biaya vs Service Level

| Target Service Level | Total Biaya | Penghematan vs 100% |
|:---:|:---:|:---:|
| 70% | Rp7.662.500 | -27,1% |
| 80% | Rp8.637.500 | -17,8% |
| 90% | Rp9.700.000 | -7,7% |
| 95% | Rp10.250.000 | -2,5% |
| **100%** | **Rp10.512.500** | **Baseline** |

### Temuan Utama

1. **Solusi MIP baseline optimal** dengan total biaya Rp10.512.500 per hari representatif, terdiri dari biaya reguler Rp7.625.000 dan biaya staf tambahan Rp2.887.500 (sekitar 27,5% dari total). Dari 41 staf reguler, Shift Malam menyerap paling banyak (20 orang) meski berdurasi hanya 7 jam, karena menanggung puncak demand jam 18:00 yang sendirian membutuhkan 32 orang.

2. **Variasi biaya antarmusim mencapai 4,4 kali lipat**: dari Rp3.475.000 di Winter hingga Rp15.150.000 di Summer, konsisten dengan selisih demand musiman sekitar 4,6:1 (226 vs 1.034 unit/jam rata-rata). Perencanaan staf statis sepanjang tahun berisiko menghasilkan overstaffing besar di Winter dan understaffing serius di Summer secara bersamaan.

3. **Kapasitas per staf adalah parameter paling sensitif**: penurunan kapasitas 20% menaikkan total biaya +25,3%, sedikit melampaui dampak kenaikan demand 20% (+20,8%) maupun kenaikan tarif staf tambahan 20% (+5,3%). Batas staf per shift hampir tidak berpengaruh (di bawah 2%), yang berlawanan dengan intuisi manajemen yang biasanya menyoroti headcount cap sebagai penggerak biaya utama.

4. **Menurunkan target service level dari 100% ke 70% menghemat 27,1% biaya** (dari Rp10.512.500 menjadi Rp7.662.500), namun penghematan ini datang dengan konsekuensi eksplisit berupa kekurangan staf di sejumlah jam. Kurva frontier ini dirancang sebagai alat diskusi dengan pemangku kebijakan operasional, bukan rekomendasi tunggal.

5. **Tidak ada integrality gap** antara solusi MIP dan LP relaxation (keduanya Rp10.512.500), mengonfirmasi bahwa solusi integer sudah sepenuhnya optimal tanpa perlu rounding heuristic. Shadow price pada batas kapasitas Shift Malam bernilai -Rp12.500, menandakan pelonggaran batas shift tersebut berpotensi menurunkan biaya dengan mensubstitusi sebagian staf tambahan yang lebih mahal.

> 📂 Seluruh output numerik, tabel, dan visualisasi tersedia langsung di dalam notebook.

---

## ⚠️ Notes / Limitations

- **Scope:** Analisis ini menggunakan data bike-sharing Seoul periode Desember 2017 sampai November 2018. Profil demand, pola musiman, dan hasil optimasi tidak dapat digeneralisasi langsung ke sistem bike-sharing di kota atau negara lain tanpa kalibrasi ulang terhadap kondisi lokal.

- **Parameter Asumsi:** Seluruh parameter biaya dan kapasitas operasional (upah Rp25.000/jam, kapasitas 50 transaksi/staf/jam, batas 20 staf per shift, premium 1,5× untuk staf tambahan) bukan berasal dari dataset. Parameter ini adalah asumsi operasional yang ditetapkan secara eksplisit dan didokumentasikan di Section 8. Sensitivitas terhadap perubahan masing-masing parameter diperiksa secara sistematis di Section 13.

- **Representative Day:** Profil demand yang menjadi input model merupakan rata-rata historis per kelompok (musim atau status holiday), bukan demand satu hari aktual. Nilai kebutuhan staf per jam sebaiknya dibaca sebagai estimasi rata-rata, bukan angka pasti harian.

- **Model Scope:** Project ini adalah model shift scheduling dan analisis sensitivitas, bukan sistem prediksi demand. Tidak ada komponen machine learning atau time series forecasting di dalamnya.

- **Skalabilitas:** Model mencakup 3 shift dan 24 slot jam dalam satu hari representatif. Ekstensi ke horizon perencanaan mingguan atau bulanan membutuhkan reformulasi dan kemungkinan penggantian solver.

- **Dependensi Platform:** Diuji menggunakan Python 3.10+ dan PuLP 2.7+ dengan CBC solver bawaan. Kompatibilitas pada versi Python di bawah 3.10 atau solver alternatif tidak diuji.

---

## 📄 License

Distributed under the **MIT License**.
See [LICENSE](LICENSE) for full details.

---

## 📬 Contact

**[Nama Lengkap]**

[![GitHub](https://img.shields.io/badge/GitHub-@[username]-181717?style=flat-square&logo=github)](https://github.com/[username])
[![LinkedIn](https://img.shields.io/badge/LinkedIn-[nama]-0A66C2?style=flat-square&logo=linkedin)](https://linkedin.com/in/[username])
[![Email](https://img.shields.io/badge/Email-[email]-D14836?style=flat-square&logo=gmail)](mailto:[email])

<br>

> 💬 Menemukan bug atau punya saran? [Buka issue baru](https://github.com/[username]/workforce-scheduling-mip-sensitivity-analysis/issues/new).

---

<div align="center">
  <sub>
    ⭐ Jika project ini bermanfaat, pertimbangkan untuk memberikan star!
    <br>
    Made with ❤️ by <a href="https://github.com/[username]">[Nama]</a> · Last updated: 2026-06
  </sub>
</div>
