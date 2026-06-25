# Analisis Multivariat Karakteristik Wine

### Menganalisis perbedaan profil kimia tiga cultivar wine Italia menggunakan pipeline statistika multivariat pada UCI Wine Dataset untuk mengidentifikasi variabel pembeda utama secara statistik.

---

## 📌 Overview

Project ini merupakan analisis statistika multivariat terhadap Wine Dataset dari UCI Machine Learning Repository, yang berisi 178 sampel wine Italia dari tiga cultivar berbeda dengan 13 variabel kimia. Pipeline analisis mencakup eksplorasi data, pengujian asumsi multivariat (normalitas dan homogenitas kovarian), uji hipotesis formal (Hotelling T² dan MANOVA), serta analisis lanjutan menggunakan PCA dan LDA. Target utama analisis ini adalah reviewer teknis, akademisi, dan siapapun yang ingin melihat implementasi statistika multivariat secara runtut dalam satu alur kerja yang terstruktur. Output berupa notebook beranotasi lengkap dengan 14 visualisasi, tabel ringkasan hasil setiap uji statistik, dan interpretasi temuan secara statistik maupun praktis.

---

## ❓ Problem Statement

**Konteks:** Wine Dataset dari UCI ML Repository (DOI: 10.24432/C5PC7J) berisi hasil analisis kimia 178 sampel wine Italia yang berasal dari tiga cultivar berbeda di satu region yang sama. Dataset ini memiliki 13 variabel kimia kontinu sebagai fitur dan satu variabel target kelas (1, 2, 3). Pertanyaan utamanya adalah, apakah ketiga kelas wine tersebut memiliki profil kimia multivariat yang berbeda secara signifikan, dan jika ya, variabel mana yang paling berkontribusi terhadap perbedaan tersebut?

**Gap:** Meskipun dataset ini sering dijadikan benchmark klasifikasi machine learning, analisis statistika multivariat yang sistematis mencakup pengujian asumsi formal, uji hipotesis dengan beberapa metode, kuantifikasi effect size, dan identifikasi variabel pembeda utama jarang dikerjakan secara menyeluruh dalam satu alur analisis yang runtut dan terdokumentasi.

**Solusi:** Project ini menerapkan pipeline analisis multivariat end-to-end, mencakup: EDA komprehensif, pengujian normalitas multivariat via Henze-Zirkler Test, deteksi outlier multivariat via Mahalanobis Distance, Hotelling T² untuk perbandingan pasangan kelas, Box's M untuk homogenitas kovarian, MANOVA dengan Wilks' Lambda beserta partial η², post-hoc ANOVA univariat, serta PCA dan LDA sebagai analisis lanjutan. Hasilnya berupa bukti statistik yang kuat bahwa ketiga kelas wine berbeda secara signifikan, dengan Flavanoids, Alcohol, dan Total_phenols sebagai variabel paling diskriminatif.

---

## 📊 Dataset

### Metadata

| Atribut | Detail |
|---|---|
| **Sumber** | UCI Machine Learning Repository |
| **Link** | [Wine Dataset (ID: 109)](https://archive.ics.uci.edu/dataset/109/wine) |
| **DOI** | [10.24432/C5PC7J](https://doi.org/10.24432/C5PC7J) |
| **Ukuran** | `178` baris × `14` kolom (`13` fitur + `1` target) |
| **Format** | Diakses via API (`ucimlrepo.fetch_ucirepo(id=109)`) — tidak perlu file lokal |
| **Lisensi** | CC BY 4.0 |
| **Tahun Pembuatan Dataset** | 1992 |
| **Pembuat** | Stefan Aeberhard, M. Forina |
| **Distribusi Kelas** | Kelas 1: `59` obs (`33.1%`) · Kelas 2: `71` obs (`39.9%`) · Kelas 3: `48` obs (`27.0%`) |
| **Missing Values** | Tidak ada (`0` dari `178` observasi) |

### Data Dictionary (Kolom Kunci)

| Kolom | Tipe Data | Deskripsi | Contoh Nilai |
|---|:---:|---|---|
| `class` | `int` | **Variabel target** — label kelas wine berdasarkan cultivar (`1`, `2`, atau `3`) | `1` |
| `Flavanoids` | `float` | Subkelompok fenol utama yang mempengaruhi warna dan rasa wine (g/L) — **variabel paling diskriminatif** (η² = `0.7278`) | `2.76` |
| `Alcohol` | `float` | Kadar alkohol (%) — pembeda terkuat ke-2 (η² = `0.6069`) | `13.20` |
| `Total_phenols` | `float` | Total senyawa fenolik, berkaitan dengan rasa pahit dan antioksidan (g/L) — pembeda terkuat ke-3 (η² ≈ `0.517`) | `2.80` |
| `Proline` | `int` | Kandungan asam amino proline (mg/L) — variabel dengan varians terbesar (std = `314.91`) | `1050` |
| `0D280_0D315_of_diluted_wines` | `float` | Rasio OD280/OD315 untuk kemurnian protein dan fenol — berkorelasi kuat dengan Flavanoids (r = `0.79`) | `3.40` |
| `Color_intensity` | `float` | Intensitas warna wine yang diukur secara fotometrik | `4.38` |
| `Malicacid` | `float` | Kandungan asam malat (g/L) — pembeda ke-4 (η² ≈ `0.297`) | `1.78` |

---

## 🛠️ Tech Stack

| Layer | Teknologi | Peran dalam Project |
|:---:|:---:|---|
| Language | Python 3.12 | Bahasa utama seluruh pipeline analisis |
| Environment | Google Colab / Jupyter Notebook | Eksekusi interaktif, eksplorasi bertahap, dan output beranotasi |
| Data Access | ucimlrepo `0.0.7` | Fetch dataset langsung dari UCI ML Repository via API — tanpa file CSV lokal |
| Data Processing | Pandas `2.2.2`, NumPy `2.0.2` | Manipulasi DataFrame, kalkulasi matriks kovarian, dan transformasi statistik |
| Statistical Testing | SciPy `1.16.3`, Pingouin `0.6.1`, Statsmodels `0.14.6` | Uji Henze-Zirkler, Hotelling T², Box's M Test, dan MANOVA (Wilks' Lambda) |
| Visualization | Matplotlib `3.10.0`, Seaborn `0.13.2` | 14 visualisasi: histogram+KDE, boxplot, heatmap korelasi, pairplot, scatter PCA/LDA, dan confusion matrix |
| ML / Dimensionality Reduction | Scikit-learn `1.6.1` | StandardScaler untuk normalisasi, PCA untuk reduksi dimensi, LDA untuk analisis diskriminan |
| Version Control | Git + GitHub | Source control dan publikasi portofolio |

---

## 🎥 Demo & Screenshots

| Heatmap Korelasi Antar Variabel Kimia | Separasi Kelas pada Ruang PCA 2D |
|:---:|:---:|
| <img width="500" height="400" alt="image" src="https://github.com/user-attachments/assets/c64e78fc-37dc-4706-a9c3-c65a9064597a" /> | <img width="700" height="360" alt="image" src="https://github.com/user-attachments/assets/1781b690-600b-4a7c-93fd-468ab574558c" /> |
| *Heatmap 13×13 dengan colormap RdYlGn menunjukkan 6 pasangan dengan korelasi kuat (r > 0.60), terutama cluster variabel fenolik di kelompok Flavanoids, Total_phenols, dan OD280/OD315* | *Scatter PCA 2D — PC1 menjelaskan `36.20%` dan PC2 `19.21%` variansi. Pemisahan ketiga kelas mulai terlihat di ruang dua dimensi* |

| Profil Mean Terstandardisasi per Kelas (MANOVA) | LDA: Separasi Kelas dan Confusion Matrix |
|:---:|:---:|
| <img width="1648" height="590" alt="image" src="https://github.com/user-attachments/assets/507b9ba9-e706-4eda-b270-c9e012420f9d" /> | <img width="700" height="360" alt="image" src="https://github.com/user-attachments/assets/cfec45df-3207-4a0b-9292-dfdc2e985fc7" /> |
| *Line plot profil mean z-score tiga kelas wine. Kelas 1 dominan tinggi di Flavanoids dan Alcohol; Kelas 3 dominan rendah. Pola pembedaan paling tajam terlihat pada variabel fenol* | *Scatter LDA 2D + confusion matrix penuh diagonal (59/59, 71/71, 48/48) dari akurasi resubstitusi `100.00%` menggunakan seluruh 13 fitur* |

---

## 📈 Results & Performance

### Temuan Utama

1. **Ketiga kelas wine berbeda secara signifikan pada seluruh kombinasi pasangan** — Hotelling T² menghasilkan p-value yang sangat kecil untuk semua tiga pasangan: Kelas 1 vs 2 (`T² = 799.66`, p = `1.35e-43`), Kelas 1 vs 3 (`T² = 3075.74`, p = `8.15e-63`), dan Kelas 2 vs 3 (`T² = 800.30`, p = `7.46e-41`). Perbedaan multivariat terbesar terjadi antara Kelas 1 dan Kelas 3, dengan nilai T² hampir 4x lipat dibanding dua pasangan lainnya.

2. **MANOVA mengkonfirmasi perbedaan yang kuat dengan effect size sangat besar** — Wilks' Lambda = `0.2067`, F = `93.21`, p = `7.55e-55`. Partial η² = `0.7933`, artinya lebih dari `79.33%` variabilitas kombinasi variabel kimia dapat dijelaskan oleh perbedaan kelas wine. Nilai ini jauh melampaui ambang batas "Large Effect Size" (η² > `0.14`) berdasarkan kriteria Cohen.

3. **Flavanoids, Alcohol, dan Total_phenols adalah variabel pembeda utama** — Post-hoc ANOVA univariat menunjukkan Flavanoids (η² = `0.7278`, F = `233.93`) sebagai variabel paling diskriminatif antar kelas, diikuti Alcohol (η² = `0.6069`, F = `135.08`) dan Total_phenols (η² ≈ `0.517`). Seluruh 7 variabel yang diuji menunjukkan perbedaan signifikan (`p < 0.05`).

4. **Terdapat 6 pasangan variabel dengan korelasi kuat (|r| > 0.60)** — Flavanoids dan Total_phenols memiliki korelasi Pearson tertinggi (`r = 0.8646`), diikuti Flavanoids-OD280/OD315 (`r ≈ 0.79`) dan Total_phenols-OD280/OD315 (`r ≈ 0.70`). Cluster korelasi kuat ini konsisten dengan asal-usul kimianya sebagai kelompok senyawa fenolik yang berkaitan.

5. **Asumsi multivariat tidak terpenuhi namun analisis tetap dapat dipertahankan** — Henze-Zirkler Test: HZ = `1.0743`, p ≈ `0.000` (tidak normal multivariat). Box's M Test: χ² = `209.99`, df = `56`, p = `1.15e-19` (kovarian tidak homogen). Kedua kondisi ini ditangani dengan justifikasi ukuran sampel `n = 178` yang cukup besar dan distribusi antar kelas yang relatif seimbang (CLT berlaku), serta adanya `12` outlier multivariat (`6.74%`) yang tidak mendominasi data.

6. **PCA dan LDA mengkonfirmasi separabilitas kelas secara visual dan kuantitatif** — PCA 2 komponen menangkap `55.41%` variansi keseluruhan (PC1 = `36.20%`, PC2 = `19.21%`), dengan pemisahan visual yang cukup jelas antar kelas. LDA menghasilkan pemisahan optimal dengan LD1 menjelaskan `68.75%` variansi antar kelas. Akurasi resubstitusi mencapai `100.00%` (lihat keterbatasan di bawah).

---

## ⚠️ Notes / Limitations

- **Scope:** Analisis ini mencakup satu dataset spesifik berisi `178` sampel wine Italia dari satu region geografis. Temuan ini tidak dimaksudkan untuk digeneralisasi ke wine dari region, negara, atau proses produksi lain karena karakteristik kimia wine sangat dipengaruhi oleh faktor terroir.

- **Asumsi Analisis:** Asumsi normalitas multivariat (Henze-Zirkler, p ≈ `0.000`) dan homogenitas kovarian (Box's M, p = `1.15e-19`) tidak terpenuhi. MANOVA dan Hotelling T² tetap dijalankan berdasarkan pertimbangan bahwa `n = 178` cukup besar untuk menerapkan Central Limit Theorem dan distribusi antar kelas relatif seimbang. Kondisi ini tidak memengaruhi arah kesimpulan, namun perlu diperhatikan dalam konteks inferensi formal.

- **LDA Resubstitution Accuracy:** Akurasi LDA `100.00%` adalah hasil resubstitusi, yaitu model dievaluasi pada data yang sama dengan data pelatihan. Nilai ini bersifat optimistis dan kemungkinan lebih rendah pada data baru. Angka ini mencerminkan kekuatan separabilitas fitur, bukan estimasi kemampuan generalisasi model.

- **Generalisasi:** Temuan dalam project ini belum divalidasi pada sampel wine lain di luar dataset UCI ini. Diperlukan validasi eksternal dengan data independen sebelum temuan ini digunakan sebagai dasar keputusan analitis di luar konteks dataset ini.
