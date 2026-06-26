<div align="center">

# Pemodelan Dinamika Harga Sembako di Jawa Timur Menggunakan 9 Konsep Matematika Diskrit

### Menganalisis pola lonjakan harga 10 komoditas sembako menjelang Tahun Baru dan Lebaran menggunakan kerangka teori matematika diskrit terintegrasi — dari FSM dengan threshold adaptif, Markov Chains multi-step, hingga operasi himpunan dan kombinatorika atas data resmi SISKAPERBAPO Jawa Timur.

<br>

[![Python](https://img.shields.io/badge/python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/jupyter-notebook-F37626?style=flat-square&logo=jupyter&logoColor=white)](notebooks/)

</div>

---

## 📌 Overview

Proyek ini menerapkan sembilan konsep matematika diskrit secara terintegrasi untuk memodelkan dinamika harga 10 komoditas sembako di Jawa Timur selama periode menjelang Tahun Baru dan Lebaran, menggunakan data mingguan SISKAPERBAPO (rentang minggu −4 hingga +4 per event). Dari `36 baris × 13 kolom` wide-format yang dimuat, transformasi `melt` menghasilkan `360 observasi` yang setelah deduplication berdasarkan kombinasi event-komoditas-minggu menjadi `180 observasi` valid untuk dianalisis. Pipeline analitik yang dibangun mengklasifikasikan setiap observasi ke dalam 4 state pasar diskrit via FSM dengan threshold adaptif berbasis kuantil, memvalidasi formula prediksi rekursif secara matematis via induksi, dan mengkuantifikasi probabilitas transisi kondisi pasar dengan Markov Chains. Output akhir berupa `11 visualisasi komprehensif`, matriks transisi state, Venn diagram operasi himpunan, dan file `rekomendasi_per_komoditas.csv` yang mengklasifikasikan 10 komoditas ke dalam 4 level risiko dengan estimasi potensi penghematan per komoditas.

---

## ❓ Problem Statement

**Konteks:** Lonjakan harga bahan pokok menjelang Lebaran dan Tahun Baru adalah fenomena berulang yang berdampak pada daya beli masyarakat Jawa Timur. Data resmi SISKAPERBAPO mencatat harga harian per komoditas, namun selama ini dimanfaatkan hanya untuk pelaporan deskriptif — tanpa pemodelan formal yang dapat menghasilkan aturan keputusan terukur atau prediksi probabilistik.

**Gap:** Analisis deskriptif tidak mampu menjawab pertanyaan kritis seperti: berapa probabilitas kondisi pasar bergerak dari Normal ke PUNCAK dalam 3 minggu? Komoditas mana yang secara konsisten spike di kedua event, dan mana yang spike hanya pada satu event? Seberapa akurat model rekursif sederhana dalam memprediksi harga minggu depan? Pertanyaan-pertanyaan ini membutuhkan kerangka matematis diskrit yang formal, bukan sekadar observasi visual pola harga.

**Solusi:** Pipeline analitik ini mengintegrasikan 9 konsep matematika diskrit — FSM M = (Q, Σ, δ, q₀, F) untuk klasifikasi kondisi pasar real-time dengan threshold adaptif berbasis Q25/Q50/Q75, formula rekursif P(n) = P(n-1) × (1+r) yang divalidasi via induksi matematis pada 20/20 kombinasi, graf transisi probabilistik G = (V, E) dengan `|V|=4, |E|=11, density=0.917`, dan Markov Chains untuk prediksi multi-step. Outputnya adalah sistem pemodelan terintegrasi yang menghasilkan rekomendasi strategis per komoditas yang dapat langsung dioperasionalkan.

---

## 📊 Dataset

### Metadata

| Atribut | Detail |
|---|---|
| **Sumber** | SISKAPERBAPO — Sistem Informasi Ketersediaan dan Perkembangan Harga Bahan Pokok Provinsi Jawa Timur |
| **Format** | Excel (`.xlsx`) — wide format |
| **Ukuran (wide)** | `36 baris × 13 kolom` (2 event × 18 tanggal, 10 kolom komoditas + 3 kolom metadata) |
| **Ukuran (long, post-melt)** | `360 observasi` → `180 observasi` setelah `drop_duplicates(['event','commodity','week'])` |
| **Komoditas** | 10 jenis: Beras, Gula Pasir, Minyak Goreng, Daging Sapi, Daging Ayam, Telur Ayam, Bawang Merah, Bawang Putih, Cabai Merah, Cabai Rawit |
| **Event** | 2 event: Lebaran, Tahun Baru |
| **Cakupan Waktu** | 2024–2025; minggu ke-(−4) hingga ke-(+4) relatif terhadap hari H |
| **Kombinasi Analisis** | `20 kombinasi` (10 komoditas × 2 event, via `itertools.product`) |

> ⚠️ Dataset tidak dicommit dalam repositori ini — data bersumber dari sistem informasi resmi pemerintah Jawa Timur. Tempatkan file `dataset_harga_sembako_2024_2025.xlsx` di direktori `data/` sebelum menjalankan notebook. Lihat `data/README.md` untuk instruksi lengkap.

### Data Dictionary — Long Format (Post-Transform)

| Kolom | Tipe Data | Deskripsi | Contoh Nilai |
|---|:---:|---|---|
| `event` | `str` | Jenis hari besar yang menjadi acuan analisis | `"Lebaran"`, `"Tahun Baru"` |
| `week` | `int` | Posisi minggu relatif terhadap hari H | `-4`, `0`, `+3` |
| `date` | `date` | Tanggal rekap dalam minggu tersebut | `2024-03-27` |
| `commodity` | `str` | Nama komoditas (10 jenis) | `"Bawang Merah"`, `"Beras"` |
| `price` | `float` | Harga rata-rata komoditas (Rp) | `27532.0`, `14938.0` |
| `base_price` | `float` | **Variabel turunan** — harga baseline (minggu paling awal per event-komoditas) | `27532.0` |
| `increase_pct` | `float` | **Variabel turunan** — % kenaikan relatif terhadap `base_price` | `-3.18`, `+38.1` |
| `state` | `str` | **Variabel turunan** — klasifikasi FSM berbasis threshold adaptif | `"Normal"`, `"PUNCAK"` |
| `T1`, `T2`, `T3` | `float` | **Variabel turunan** — threshold Siaga/Peringatan/PUNCAK per kombinasi event-komoditas | `2.0`, `5.0`, `10.0` |

---

## 🛠️ Tech Stack

| Layer | Teknologi | Peran dalam Proyek |
|:---:|:---:|---|
| Language | Python 3.10+ | Bahasa utama seluruh pipeline analisis |
| Environment | Jupyter Notebook / Google Colab | Eksekusi interaktif bertahap dengan output inline per section |
| Data Processing | Pandas, NumPy | Transformasi wide-to-long (`melt`), deduplication, komputasi statistik, matrix operations |
| Visualization | Matplotlib, Seaborn | 11 visualisasi: stacked bar state, heatmap transisi Markov, Venn diagram, NetworkX layout, dashboard komprehensif |
| Graph Analysis | NetworkX | Konstruksi `DiGraph`, shortest path via `nx.shortest_path()`, density via `nx.density()` |
| Mathematics | `math`, `itertools`, `functools` | Kombinatorika (`comb`, `factorial`), GCD/LCM (`reduce(gcd,...)`), generator (`product`, `combinations`) |
| Linear Algebra | NumPy `linalg` | Matrix exponentiation Markov (`matrix_power`), eigendecomposition steady-state (`eig`) |
| Version Control | Git + GitHub | Source control dan dokumentasi perubahan |

---

## 🎥 Screenshots

<br>

| Dashboard Komprehensif — 11 Panel, 9 Konsep | Graf Transisi FSM dengan Shortest Path |
|:---:|:---:|
| (Screenshot: output Section 10 — dashboard 4×3 grid berisi: panel recurrence tren 5 komoditas paling volatile dengan zona AMAN/BAHAYA/PEMULIHAN berwarna, pie chart FSM 81.7% Normal, graf NetworkX, bar chart Boolean logic, C(4,k), heatmap Markov P², dan truth table Boolean) | (Screenshot: panel bawah Section 4 — directed graph NetworkX dengan 4 node berwarna: hijau Normal, kuning Siaga, oranye Peringatan, merah PUNCAK; edge berlabel probabilitas; highlight merah shortest path Normal → Siaga → Peringatan → PUNCAK) |
| *Integrasi visual seluruh hasil analisis — Section 10* | *Representasi G=(V,E): density 0.917, P(Normal→PUNCAK) = 0.42% via 3 transisi* |

| Venn Diagram Operasi Himpunan | PMF dan Markov Chains Multi-Step |
|:---:|:---:|
| (Screenshot: output Section 8 — Venn diagram dua lingkaran biru Lebaran dan merah muda Tahun Baru; area A-B berisi Daging Ayam/Daging Sapi/Gula Pasir \|3\|; area A∩B berisi Bawang Merah/Bawang Putih \|2\|; box (A∪B)ᶜ berisi Telur Ayam/Cabai Rawit/dll \|5\|) | (Screenshot: 4-panel Section 9 — bar chart PMF dengan P(<0%)=0.633 tertinggi; heatmap P² dan P³ berwarna YlOrRd; bar grouped chart evolusi probabilitas P¹/P²/P³ dari state Normal) |
| *A ∩ B = {Bawang Merah, Bawang Putih}; 5 komoditas tidak pernah spike (complement)* | *E(X) = −12.76%; P³(Normal→PUNCAK) = 0.0042; steady-state Normal = 57.8%* |

---

## 📈 Results & Performance

### Ringkasan Kuantitatif per Konsep

| # | Konsep | Metrik Kunci | Hasil Aktual |
|:---:|---|---|---|
| 1 | **Finite State Machine** | Distribusi state (N = 180) | Normal `81.7%` · Siaga `10.6%` · Peringatan `4.4%` · PUNCAK `3.3%` |
| 2 | **Recurrence Relations** | Akurasi prediksi out-of-sample (N = 20) | Rata-rata `95.17%` (error `4.83%`) |
| 3 | **Induksi Matematis** | Konsistensi empiris formula P(n) | `20/20` kombinasi, error `0.0000%` |
| 4 | **Teori Graf** | Density · Shortest path | `0.917` · Normal→PUNCAK: 3 transisi, P = `0.42%` |
| 5 | **Boolean Logic** | Distribusi predikat (N = 180) | KRITIS `13` (7.2%) · AMAN_BELI `39` (21.7%) · HINDARI `6` (3.3%) |
| 6 | **Kombinatorika** | Total skenario faktor (n = 4) | `15` skenario (2⁴ − 1) ✓ |
| 7 | **Teori Bilangan** | GCD timing peak · Pola modulo 3 | GCD = `1` (tidak ada siklus umum) · tidak signifikan (Δ `1.81%`) |
| 8 | **Teori Himpunan** | Kardinalitas operasi set | \|A∩B\| = `2` · \|A∪B\| = `5` · \|(A∪B)ᶜ\| = `5` |
| 9 | **Peluang Diskrit** | E(X) global · Steady-state Markov | `−12.76%` · Normal dominan `57.8%` |

### Output Rekomendasi per Komoditas (`rekomendasi_per_komoditas.csv`)

| Komoditas | Tingkat Risiko | Prioritas | Potensi Hemat | Timing Beli |
|---|:---:|:---:|:---:|---|
| Bawang Merah | **SANGAT TINGGI** | P1 | `25.4%` | Beli 4+ minggu sebelum event |
| Bawang Putih | TINGGI | P2 | `7.1%` | Beli 3–4 minggu sebelum event |
| Daging Ayam | SEDANG | P3 | `9.1%` | Beli 2–3 minggu sebelum event |
| Beras | RENDAH | P4 | `2.5%` | Timing fleksibel |
| Daging Sapi | RENDAH | P4 | `2.2%` | Timing fleksibel |
| Cabai Rawit | RENDAH | P4 | `5.2%` | Timing fleksibel |
| Gula Pasir | RENDAH | P4 | `0.0%` | Timing fleksibel |
| Minyak Goreng | RENDAH | P4 | `0.0%` | Timing fleksibel |
| Telur Ayam | RENDAH | P4 | `0.0%` | Timing fleksibel |
| Cabai Merah | RENDAH | P4 | `0.0%` | Timing fleksibel |

> Tingkat risiko dihitung dari composite score: kenaikan maksimum (bobot 0.6) + Coefficient of Variation × 20 + frekuensi state krisis (bobot 0.4). Potensi hemat = selisih harga median zona aman (minggu ≤ −2) vs Q75 zona bahaya (minggu −1 s/d +1).

### Matriks Probabilitas Transisi — Teori Graf (Section 4)

|  | → Normal | → Siaga | → Peringatan | → PUNCAK |
|---|:---:|:---:|:---:|:---:|
| **Normal →** | `0.947` | `0.053` | `0.000` | `0.000` |
| **Siaga →** | `0.176` | `0.588` | `0.235` | `0.000` |
| **Peringatan →** | `0.000` | `0.167` | `0.500` | `0.333` |
| **PUNCAK →** | `0.000` | `0.167` | `0.167` | `0.667` |

> Dibangun dari 160 transisi empiris (10 komoditas × 2 event × 8 langkah). Graf G = (V, E) dengan |V| = 4, |E| = 11, density = `0.917`.

### Temuan Analitik Utama

1. **Kenaikan harga hari besar bersifat selektif, bukan universal.** Dari 180 observasi, `81.7%` berada dalam state Normal dan hanya `3.3%` (6 observasi) mencapai PUNCAK. PMF menunjukkan `63.3%` observasi memiliki harga di bawah baseline minggu ke-(−4). Fenomena "harga naik menjelang hari besar" hanya terjadi pada segmen komoditas tertentu, tidak pada seluruh pasar.

2. **Bawang Merah adalah komoditas dengan volatilitas ekstrem.** Dari 18 observasi Bawang Merah, `55.56%` masuk state Peringatan atau PUNCAK (10 observasi), E(X) = `+23.05%` dari baseline Rp 27,532, dan kondisi KRITIS tercatat `50.0%` dari waktu (9/18 observasi), dengan contoh puncak di minggu 0 pada state Peringatan dengan kenaikan `38.1%`. Potensi penghematan `25.4%` tersedia jika pembelian dilakukan di zona aman. Berbanding terbalik, Telur Ayam mencatat `100%` state Normal di seluruh 18 observasi dengan E(X) = `−30.01%` — tidak memerlukan strategi pembelian khusus.

3. **Formula recurrence P(n) = P(n-1) × (1+r) sangat akurat untuk komoditas stabil, terbatas untuk bumbu.** Beras: akurasi `99.85%` (prediksi Rp 14,897 vs aktual Rp 14,938, selisih Rp 41). Cabai Merah: akurasi `80.25%` (prediksi Rp 25,471 vs aktual Rp 34,454, selisih Rp 8,983). Rata-rata global `95.17%` dari 20 validasi out-of-sample. Komoditas dengan growth rate `r` yang konsisten menghasilkan prediksi sangat akurat; komoditas volatile memerlukan model adaptif dengan `r` dinamis.

4. **Induksi matematis memverifikasi konsistensi formula secara absolut.** Dari 20/20 kombinasi komoditas-event, error verifikasi relasi P(n) = P(n-1) × (1+r) adalah `0.0000%` pada seluruh transisi yang dicek. Contoh: Bawang Merah pada Lebaran — P(−4) = Rp 27,532 → P(−3) = Rp 27,532 × 1.0010 = Rp 27,559 ✓ → P(−1) = Rp 27,532 × 1.0010 × 1.0003 × 1.0803 = Rp 29,783 ✓. Formula P(n) = P(0) × ∏[i=0 to n-1](1+rᵢ) terbukti valid secara matematis untuk semua n ∈ ℕ.

5. **State PUNCAK tidak dapat dicapai dalam kurang dari 3 minggu dari kondisi Normal.** P¹(Normal→PUNCAK) = `0.00%`, P²(Normal→PUNCAK) = `0.00%`, P³(Normal→PUNCAK) = `0.42%`. Dari state Normal, probabilitas masih berada di Normal setelah 2 langkah mencapai `0.905`. Temuan ini memberikan dasar kuantitatif untuk rekomendasi zona aman pembelian: 3+ minggu sebelum event masih aman secara probabilistik.

6. **Hanya 2 dari 10 komoditas menunjukkan spike konsisten di kedua event (A ∩ B).** A = himpunan spike Lebaran: {Bawang Putih, Bawang Merah, Daging Sapi, Daging Ayam, Gula Pasir} (|A| = 5). B = himpunan spike Tahun Baru: {Bawang Merah, Bawang Putih} (|B| = 2). A ∩ B = {Bawang Merah, Bawang Putih} (|A ∩ B| = 2). (A ∪ B)ᶜ = {Beras, Minyak Goreng, Telur Ayam, Cabai Merah, Cabai Rawit} — 5 komoditas tidak pernah melebihi threshold spike di event manapun. Verifikasi: |A ∪ B| = 5 + 2 − 2 = 5 ✓; |U| = 5 + 5 = 10 ✓.

7. **Teori bilangan mengidentifikasi ketiadaan siklus umum dan pola pricing presisi.** GCD timing peak = `1` (tidak ada siklus berulang yang seragam antar komoditas), LCM = `12`. Pola modulo 3 tidak signifikan (rentang perbedaan rata-rata kenaikan antar kelas ekuivalensi hanya `1.81%`). Perubahan harga bersifat presisi (non-kelipatan besar): hanya `15.1%` perubahan harga yang merupakan kelipatan Rp 10, dengan divisibilitas terbanyak di Rp 50.

8. **PMF dan Markov steady-state menunjukkan pasar cenderung pulih ke kondisi Normal.** Distribusi steady-state: Normal `57.8%`, yang lebih tinggi dari distribusi empiris `81.7%` — namun secara arah konsisten bahwa Normal adalah kondisi dominan jangka panjang. Dari state Peringatan, probabilitas pemulihan ke Normal dalam 2 langkah adalah `P²[Peringatan→Normal]` = `0.029` (rendah), menunjukkan bahwa sekali masuk Peringatan, kondisi cenderung bertahan atau memburuk sebelum membaik.

> 📂 Output rekomendasi per komoditas tersimpan di `outputs/rekomendasi_per_komoditas.csv`.

---

## ⚠️ Notes / Limitations

- **Interpretasi E(X) = −12.76%:** Nilai ini diukur relatif terhadap baseline minggu ke-(−4) dan merepresentasikan rata-rata perubahan harga di seluruh 9 minggu pengamatan untuk semua komoditas. Nilai negatif didominasi oleh komoditas stabil yang harganya stagnan atau turun dari titik acuan awal (terutama Telur Ayam dengan E(X) = −30.01%). Komoditas volatile seperti Bawang Merah tetap menunjukkan E(X) positif (+23.05%).

- **Scope:** Analisis terbatas pada Tahun Baru 2024–2025 dan Lebaran 2025 dengan rentang 9 minggu per event. Fenomena musiman lain (Idul Adha, shock harga BBM, bencana alam) tidak tercakup dan dapat menghasilkan pola state yang berbeda secara struktural.

- **Data:** Dataset merepresentasikan rata-rata harga tingkat provinsi Jawa Timur. Variasi antar kabupaten/kota dan granularitas harian tidak tersedia dalam dataset ini. Proses `drop_duplicates` dari 360 ke 180 observasi mengindikasikan adanya dua entri tanggal per minggu dalam data asli — yang pertama dipertahankan per kombinasi event-komoditas-minggu.

- **Asumsi Model:** Threshold FSM bersifat adaptif per kombinasi event-komoditas (bukan threshold absolut), sehingga angka state antara komoditas yang berbeda tidak dapat dibandingkan secara langsung. Formula recurrence mengasumsikan `r` konstan dalam periode estimasi — asumsi ini tidak terpenuhi untuk bumbu volatil (Cabai Merah error `19.75%`).

- **Keterbatasan Teknis:** Notebook menggunakan mode interaktif (`input()` di Section 0) yang tidak kompatibel dengan `Run All`. Jalankan section secara berurutan dan ikuti prompt konfigurasi yang muncul. Output yang dihasilkan bergantung pada pilihan konfigurasi yang dipilih di Section 0.

---

<div align="center">
  <sub>
    ⭐ Jika analisis ini bermanfaat, pertimbangkan untuk memberikan star!
    <br>
    Made by
    <a href="https://github.com/[username]">Ahmad Kenzy Farzaq</a>,
    · 2025
  </sub>
</div>
