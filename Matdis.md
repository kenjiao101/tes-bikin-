# Modeling Staple Food Price Dynamics in East Java Using 9 Discrete Mathematics Concepts

### Analyzing price spike patterns for 10 staple food commodities ahead of Tahun Baru (New Year) and Lebaran (Eid Al-Fitr) using an integrated discrete mathematics framework from FSMs with adaptive thresholds and multi-step Markov Chains to set operations and combinatorics, applied to official SISKAPERBAPO East Java data.

## 📝 Table of Contents

1. [Overview](#-overview)
2. [Problem Statement](#-problem-statement)
3. [Dataset](#-dataset)
4. [Tech Stack](#-tech-stack)
5. [Getting Started](#-getting-started)
6. [Usage](#-usage)
7. [Screenshots](#-screenshots)
8. [Results and Performance](#-results-and-performance)
9. [Limitations](#-limitations)
---

## 📌 Overview

This project applies nine discrete mathematics concepts in an integrated way to model the price dynamics of 10 staple food commodities in East Java during the periods leading up to Tahun Baru (New Year) and Lebaran (Eid Al-Fitr), using weekly SISKAPERBAPO data (covering weeks −4 through +4 relative to each event). From the `36 baris × 13 kolom` wide-format data that was loaded, a `melt` transformation produces `360 observations`, which after deduplication based on the event-commodity-week combination become `180 observations` valid for analysis. The analytical pipeline classifies each observation into one of 4 discrete market states via an FSM with adaptive, quantile-based thresholds, mathematically validates the recursive prediction formula via induction, and quantifies market state transition probabilities using Markov Chains. The final output consists of `11 visualisasi komprehensif`, a state transition matrix, a Venn diagram of set operations, and a `rekomendasi_per_komoditas.csv` file that classifies the 10 commodities into 4 risk levels with estimated potential savings per commodity.

## ❓ Problem Statement

**Context:** Staple food price spikes ahead of Lebaran and Tahun Baru are a recurring phenomenon that affects the purchasing power of the East Java population. Official SISKAPERBAPO data records daily prices per commodity, but it has so far only been used for descriptive reporting, without formal modeling capable of producing measurable decision rules or probabilistic predictions.

**Gap:** Descriptive analysis cannot answer critical questions such as: what is the probability of a market condition moving from Normal to PUNCAK within 3 weeks? Which commodities consistently spike across both events, and which spike in only one? How accurate is a simple recursive model at predicting next week's price? These questions require a formal discrete mathematical framework, not merely a visual observation of price patterns.

**Solution:** This analytical pipeline integrates 9 discrete mathematics concepts — an FSM M = (Q, Σ, δ, q₀, F) for real-time market condition classification with adaptive thresholds based on Q25/Q50/Q75, a recursive formula P(n) = P(n-1) × (1+r) validated mathematically via induction across 20/20 combinations, a probabilistic transition graph G = (V, E) with `|V|=4, |E|=11, density=0.917`, and Markov Chains for multi-step prediction. The output is an integrated modeling system that produces strategic, directly actionable recommendations per commodity.

## 📊 Dataset

### Metadata

| Attribute | Detail |
|---|---|
| **Source** | SISKAPERBAPO — East Java Province's Information System for the Availability and Price Development of Staple Goods |
| **Format** | Excel (`.xlsx`) — wide format |
| **Size (wide)** | `36 baris × 13 kolom` (2 events × 18 dates, 10 commodity columns + 3 metadata columns) |
| **Size (long, post-melt)** | `360 observations` → `180 observations` after `drop_duplicates(['event','commodity','week'])` |
| **Commodities** | 10 types: Beras, Gula Pasir, Minyak Goreng, Daging Sapi, Daging Ayam, Telur Ayam, Bawang Merah, Bawang Putih, Cabai Merah, Cabai Rawit |
| **Event** | 2 events: Lebaran, Tahun Baru |
| **Time Coverage** | 2024–2025; week (−4) through week (+4) relative to the event day |
| **Analysis Combinations** | `20 combinations` (10 commodities × 2 events, via `itertools.product`) |

### Data Dictionary — Long Format (Post-Transform)

| Column | Data Type | Description | Example Value |
|---|:---:|---|---|
| `event` | `str` | Type of holiday used as the analysis reference | `"Lebaran"`, `"Tahun Baru"` |
| `week` | `int` | Week position relative to the event day | `-4`, `0`, `+3` |
| `date` | `date` | Recap date within that week | `2024-03-27` |
| `commodity` | `str` | Commodity name (10 types) | `"Bawang Merah"`, `"Beras"` |
| `price` | `float` | Average commodity price (Rp) | `27532.0`, `14938.0` |
| `base_price` | `float` | **Derived variable** — baseline price (earliest week per event-commodity) | `27532.0` |
| `increase_pct` | `float` | **Derived variable** — % increase relative to `base_price` | `-3.18`, `+38.1` |
| `state` | `str` | **Derived variable** — FSM classification based on adaptive thresholds | `"Normal"`, `"PUNCAK"` |
| `T1`, `T2`, `T3` | `float` | **Derived variable** — Siaga/Peringatan/PUNCAK thresholds per event-commodity combination | `2.0`, `5.0`, `10.0` |

## 🛠️ Tech Stack

| Layer | Technology | Role in the Project |
|:---:|:---:|---|
| Language | Python 3.10+ | Main language for the entire analysis pipeline |
| Environment | Jupyter Notebook / Google Colab | Step-by-step interactive execution with inline output per section |
| Data Processing | Pandas, NumPy | Wide-to-long transformation (`melt`), deduplication, statistical computation, matrix operations |
| Visualization | Matplotlib, Seaborn | 11 visualizations: state stacked bar, Markov transition heatmap, Venn diagram, NetworkX layout, comprehensive dashboard |
| Graph Analysis | NetworkX | Construction of `DiGraph`, shortest path via `nx.shortest_path()`, density via `nx.density()` |
| Mathematics | `math`, `itertools`, `functools` | Combinatorics (`comb`, `factorial`), GCD/LCM (`reduce(gcd,...)`), generators (`product`, `combinations`) |
| Linear Algebra | NumPy `linalg` | Markov matrix exponentiation (`matrix_power`), steady-state eigendecomposition (`eig`) |
| Version Control | Git + GitHub | Source control and change documentation |

## 🎥 Screenshots

### FSM Transition Graph with Shortest Path

<p align="center">
  <img width="680" height="450" alt="image"
       src="https://github.com/user-attachments/assets/612ddabe-ec83-4c5c-8c11-75ef80c5ba9a" />
</p>

A representation of G=(V,E): density 0.917, P(Normal→PUNCAK) = 0.42% via 3 transitions.

### Venn Diagram of Set Operations

<p align="center">
  <img width="450" height="470" alt="image"
       src="https://github.com/user-attachments/assets/de6cb4f8-b8a7-4746-94d0-8d1260ef3723" />
</p>

A ∩ B = {Bawang Merah, Bawang Putih}; 5 commodities never spike (the complement).

### PMF and Multi-Step Markov Chains

<p align="center">
  <img width="705" height="550" alt="image"
       src="https://github.com/user-attachments/assets/463a2ac1-bdb0-43a6-af46-b1b63b4506ee" />
</p>

E(X) = −12.76%; P³(Normal→PUNCAK) = 0.0042; steady-state Normal = 57.8%.

### Comprehensive Dashboard — 11 Panels, 9 Concepts

<p align="center">
  <img width="900" height="1100" alt="image"
       src="https://github.com/user-attachments/assets/fda92fe4-ac8b-4ea9-8f48-8a3e92b6ca4a" />
</p>

A visual integration of all analysis results (Section 10).

## 📈 Results and Performance

### Quantitative Summary by Concept

| # | Concept | Key Metric | Actual Result |
|:---:|---|---|---|
| 1 | **Finite State Machine** | State distribution (N = 180) | Normal `81.7%` · Siaga `10.6%` · Peringatan `4.4%` · PUNCAK `3.3%` |
| 2 | **Recurrence Relations** | Out-of-sample prediction accuracy (N = 20) | Average `95.17%` (error `4.83%`) |
| 3 | **Mathematical Induction** | Empirical consistency of formula P(n) | `20/20` combinations, error `0.0000%` |
| 4 | **Graph Theory** | Density · Shortest path | `0.917` · Normal→PUNCAK: 3 transitions, P = `0.42%` |
| 5 | **Boolean Logic** | Predicate distribution (N = 180) | KRITIS `13` (7.2%) · AMAN_BELI `39` (21.7%) · HINDARI `6` (3.3%) |
| 6 | **Combinatorics** | Total factor scenarios (n = 4) | `15` scenarios (2⁴ − 1) ✓ |
| 7 | **Number Theory** | GCD of peak timing · Modulo-3 pattern | GCD = `1` (no common cycle) · not significant (Δ `1.81%`) |
| 8 | **Set Theory** | Cardinality of set operations | \|A∩B\| = `2` · \|A∪B\| = `5` · \|(A∪B)ᶜ\| = `5` |
| 9 | **Discrete Probability** | Global E(X) · Markov steady-state | `−12.76%` · Normal dominant at `57.8%` |

### Per-Commodity Recommendation Output (`rekomendasi_per_komoditas.csv`)

| Commodity | Risk Level | Priority | Potential Savings | Buying Timing |
|---|:---:|:---:|:---:|---|
| Bawang Merah | **SANGAT TINGGI** | P1 | `25.4%` | Buy 4+ weeks before the event |
| Bawang Putih | TINGGI | P2 | `7.1%` | Buy 3–4 weeks before the event |
| Daging Ayam | SEDANG | P3 | `9.1%` | Buy 2–3 weeks before the event |
| Beras | RENDAH | P4 | `2.5%` | Flexible timing |
| Daging Sapi | RENDAH | P4 | `2.2%` | Flexible timing |
| Cabai Rawit | RENDAH | P4 | `5.2%` | Flexible timing |
| Gula Pasir | RENDAH | P4 | `0.0%` | Flexible timing |
| Minyak Goreng | RENDAH | P4 | `0.0%` | Flexible timing |
| Telur Ayam | RENDAH | P4 | `0.0%` | Flexible timing |
| Cabai Merah | RENDAH | P4 | `0.0%` | Flexible timing |

> Risk level is calculated from a composite score: maximum increase (weight 0.6) + Coefficient of Variation × 20 + frequency of crisis states (weight 0.4). Potential savings = the difference between the median price in the safe zone (week ≤ −2) and the Q75 price in the danger zone (week −1 to +1).

### Transition Probability Matrix — Graph Theory (Section 4)

|  | → Normal | → Siaga | → Peringatan | → PUNCAK |
|---|:---:|:---:|:---:|:---:|
| **Normal →** | `0.947` | `0.053` | `0.000` | `0.000` |
| **Siaga →** | `0.176` | `0.588` | `0.235` | `0.000` |
| **Peringatan →** | `0.000` | `0.167` | `0.500` | `0.333` |
| **PUNCAK →** | `0.000` | `0.167` | `0.167` | `0.667` |

> Built from 160 empirical transitions (10 commodities × 2 events × 8 steps). Graph G = (V, E) with |V| = 4, |E| = 11, density = `0.917`.

### Key Analytical Findings

1. **Holiday price increases are selective, not universal.** Out of 180 observations, `81.7%` fall within the Normal state and only `3.3%` (6 observations) reach PUNCAK. The PMF shows that `63.3%` of observations have a price below the week-(−4) baseline. The phenomenon of "prices rising ahead of a holiday" only occurs for certain commodity segments, not across the entire market.

2. **Bawang Merah is the commodity with extreme volatility.** Of 18 Bawang Merah observations, `55.56%` fall into the Peringatan or PUNCAK state (10 observations), E(X) = `+23.05%` from a baseline of Rp 27,532, and the KRITIS condition is recorded `50.0%` of the time (9/18 observations), with a peak example at week 0 in the Peringatan state showing an increase of `38.1%`. A potential savings of `25.4%` is available if purchases are made during the safe zone. By contrast, Telur Ayam records `100%` Normal state across all 18 observations with E(X) = `−30.01%`, requiring no special buying strategy.

3. **The recurrence formula P(n) = P(n-1) × (1+r) is highly accurate for stable commodities but limited for spices.** Beras: `99.85%` accuracy (predicted Rp 14,897 vs. actual Rp 14,938, a difference of Rp 41). Cabai Merah: `80.25%` accuracy (predicted Rp 25,471 vs. actual Rp 34,454, a difference of Rp 8,983). The global average is `95.17%` across 20 out-of-sample validations. Commodities with a consistent growth rate `r` yield highly accurate predictions; volatile commodities require an adaptive model with a dynamic `r`.

4. **Mathematical induction absolutely verifies the consistency of the formula.** Across 20/20 commodity-event combinations, the verification error for the relation P(n) = P(n-1) × (1+r) is `0.0000%` for every transition checked. Example: Bawang Merah at Lebaran — P(−4) = Rp 27,532 → P(−3) = Rp 27,532 × 1.0010 = Rp 27,559 ✓ → P(−1) = Rp 27,532 × 1.0010 × 1.0003 × 1.0803 = Rp 29,783 ✓. The formula P(n) = P(0) × ∏[i=0 to n-1](1+rᵢ) is mathematically proven valid for all n ∈ ℕ.

5. **The PUNCAK state cannot be reached in fewer than 3 weeks from a Normal condition.** P¹(Normal→PUNCAK) = `0.00%`, P²(Normal→PUNCAK) = `0.00%`, P³(Normal→PUNCAK) = `0.42%`. From the Normal state, the probability of still being in Normal after 2 steps reaches `0.905`. This finding provides a quantitative basis for the safe-zone buying recommendation: 3+ weeks before an event is still probabilistically safe.

6. **Only 2 of the 10 commodities show a consistent spike across both events (A ∩ B).** A = the Lebaran spike set: {Bawang Putih, Bawang Merah, Daging Sapi, Daging Ayam, Gula Pasir} (|A| = 5). B = the Tahun Baru spike set: {Bawang Merah, Bawang Putih} (|B| = 2). A ∩ B = {Bawang Merah, Bawang Putih} (|A ∩ B| = 2). (A ∪ B)ᶜ = {Beras, Minyak Goreng, Telur Ayam, Cabai Merah, Cabai Rawit}, meaning these 5 commodities never exceed the spike threshold at either event. Verification: |A ∪ B| = 5 + 2 − 2 = 5 ✓; |U| = 5 + 5 = 10 ✓.

7. **Number theory identifies the absence of a common cycle and a precise pricing pattern.** The GCD of peak timing = `1` (no uniform recurring cycle across commodities), LCM = `12`. The modulo-3 pattern is not significant (the range of average increase differences between equivalence classes is only `1.81%`). Price changes are precise (not large round numbers): only `15.1%` of price changes are multiples of Rp 10, with the highest divisibility at Rp 50.

8. **The PMF and the Markov steady-state show that the market tends to recover toward a Normal condition.** The steady-state distribution: Normal `57.8%`, which is higher than the empirical distribution of `81.7%`, though directionally consistent with Normal being the dominant condition in the long run. From the Peringatan state, the probability of recovering to Normal within 2 steps is `P²[Peringatan→Normal]` = `0.029` (low), showing that once a commodity enters Peringatan, the condition tends to persist or worsen before improving.

> 📂 The per-commodity recommendation output is saved at `outputs/rekomendasi_per_komoditas.csv`. Note: the resulting output file depends on the configuration selected in Section 0.

## 🚀 Getting Started

> This project is part of a larger collection repository. Clone the collection repo first (see the root README), then navigate into this folder.

1. Navigate to this project folder:
```bash
cd discrete-math-price-dynamics-jatim
```

2. Create a virtual environment (Optional):
```bash
python -m venv venv
source venv/bin/activate   # Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. The dataset are already included in this repository. After cloning or downloading the repository, the file will already be available locally.

5. **Important, reproducibility note**: the original notebook was developed in Google Colab and uses the file upload widget (`google.colab.files.upload()`). To run it locally (Jupyter/VS Code), replace that cell with direct file reading from a local folder:
```bash
import pandas as pd
df_raw = pd.read_excel('data/dataset_harga_sembako_2024_2025.xlsx')
```

6. Run the notebook:
```bash
jupyter notebook notebooks/price_dynamics_discrete_math_analysis.ipynb
```
Execute the cells sequentially from top to bottom.

## ⚠️ Limitations

- **Interpretation of E(X) = −12.76%:** This value is measured relative to the week-(−4) baseline and represents the average price change across all 9 observation weeks for all commodities. The negative value is dominated by stable commodities whose prices are stagnant or declining from their initial reference point (especially Telur Ayam, with E(X) = −30.01%). Volatile commodities such as Bawang Merah still show a positive E(X) (+23.05%).

- **Scope:** The analysis is limited to Tahun Baru 2024–2025 and Lebaran 2025, with a 9-week range per event. Other seasonal phenomena (Idul Adha, fuel price shocks, natural disasters) are not covered and could produce structurally different state patterns.

- **Technical Limitation:** The notebook uses an interactive mode (`input()` in Section 0) that is not compatible with `Run All`. Run the sections sequentially and follow the configuration prompts that appear. The resulting output depends on the configuration choices selected in Section 0. However, this mode is not disruptive at all, and is in fact quite helpful if you want to look at a particular category or a specific commodity.
