<div align="center">

# Workforce Allocation Optimization & Sensitivity Analysis

### Optimizing the cost and allocation of operational workforce for Seoul's bike-sharing system through a Mathematical Programming approach with empirical, parametric, and service-level trade-off sensitivity analysis

<br>

[![License](https://img.shields.io/badge/license-MIT-3DA639?style=flat-square)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.10+-3776AB?style=flat-square&logo=python&logoColor=white)]()
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?style=flat-square&logo=jupyter&logoColor=white)](notebooks/)
[![PuLP](https://img.shields.io/badge/solver-PuLP%20%2B%20CBC-5C6BC0?style=flat-square)]()

</div>

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
10. [License](#-license)
11. [Contact](#-contact)
---

## 📌 Overview

This project implements a **Mixed-Integer Programming (MIP)** and **Linear Programming (LP)** model to optimize workforce allocation and cost for Seoul's bike-sharing system. The dataset used is the Seoul Bike Sharing Demand dataset from the UCI Repository (8,760 records spanning one full year, December 2017 through November 2018), which is processed into an hourly demand profile used as input for staffing requirements. The model is solved using the CBC solver via PuLP, then validated and tested across nine empirical scenarios based on season and holiday status, parametric sensitivity analysis via a tornado chart, a cost-versus-service-level trade-off curve, and additional diagnostics through LP relaxation with shadow prices. The final output is an operational decision-making framework that quantitatively shows the optimal cost per scenario, a priority order for validating assumptions, and the service-level threshold that corresponds to cost savings.

---

## ❓ Problem Statement

**Context:** Urban bike-sharing systems face a non-trivial staffing allocation challenge because demand is highly dynamic, bimodal within a single day, varies widely between seasons (a ratio of up to 4.6 times between Summer and Winter based on the 2017-2018 Seoul data), and is influenced by holiday status. Without a structured analytical framework, operators tend to rely on static approaches that result in overstaffing during quiet periods and understaffing during peak hours at the same time.

**Gap:** Commonly used workforce planning approaches rely on rules of thumb or manual planning that do not systematically account for demand variation. There is no formal mechanism to measure how sensitive cost is to changes in operational parameters such as capacity per staff member or target service level, even though this information is crucial for policy decision-making.

**Solution:** This project formulates the staffing allocation problem as a shift scheduling Mixed-Integer Program (a variant of Dantzig's set-covering formulation), with a supplemental staff variable acting as a capacity safety valve. The model is tested across various empirical and parametric scenarios, producing decision tables and curves that can be used directly as a basis for operational policy discussions.

---

## 📊 Dataset

### Metadata

| Attribute | Detail |
|---|---|
| **Source** | UCI Machine Learning Repository |
| **Link** | [Seoul Bike Sharing Demand (id=560)](https://archive.ics.uci.edu/dataset/560/seoul+bike+sharing+demand) |
| **Initial Size** | 8,760 rows x 14 columns |
| **Operational Size** | 8,465 rows (295 non-operational rows filtered out) |
| **Format** | CSV |
| **License** | CC BY 4.0 |
| **Time Coverage** | 2017-12-01 to 2018-11-30 (1 full year) |
| **DOI** | 10.24432/C5F62R |

### Data Dictionary (Key Columns)

| Column | Type | Description | Example Value |
|---|:---:|---|---|
| `date` | `object` | Observation date | `01/12/2017` |
| `rented_bike_count` | `int64` | **Main variable** — number of bikes rented per hour | `254`, `0` |
| `hour` | `int64` | Hour of observation within the day (0–23) | `8`, `18` |
| `temperature` | `float64` | Air temperature (°C) | `-6.2`, `32.4` |
| `humidity` | `int64` | Relative humidity (%) | `36`, `91` |
| `seasons` | `object` | Season at time of observation | `Winter`, `Summer` |
| `holiday` | `object` | Holiday status | `Holiday`, `No Holiday` |
| `functioning_day` | `object` | Service operational status — used as the initial filter | `Yes`, `No` |

### Data Reproduction

```bash
# The dataset is downloaded automatically when the notebook is run — no manual steps required

# If the API connection is unavailable, use the local file:
# data/raw/SeoulBikeData.csv  (same source: UCI id=560)
```

> **Filtering note:** 295 rows with `functioning_day = No` and `rented_bike_count = 0` are excluded before analysis because they represent hours when the service was closed, not low demand. Including these rows in the demand profile would bias the staffing requirement estimate downward.

---

## 🛠️ Tech Stack

| Layer | Technology | Role in the Project |
|:---:|:---:|---|
| Language | Python 3.10+ | Main language for all project logic |
| Environment | Jupyter Notebook | Interactive execution, analytical narrative, and inline visualization |
| Optimization | PuLP 2.7+ | MIP formulation and solving using the built-in CBC solver |
| Data Fetching | ucimlrepo | Direct access to the UCI ML Repository with a fallback to a local CSV |
| Data Processing | Pandas, NumPy | Dataframe manipulation, demand aggregation, translation into staffing requirements |
| Visualization | Matplotlib, Seaborn | Demand profile, staffing chart, tornado chart, trade-off frontier |
| Version Control | Git + GitHub | Source control and portfolio distribution |

---

## 🚀 Getting Started

### Prerequisites

| Requirement | Minimum Version | Verification |
|---|:---:|---|
| Python | 3.10+ | `python --version` |
| pip | 23.0+ | `pip --version` |
| Git | 2.30+ | `git --version` |

### Installation

1. Clone the repository
```bash
git clone https://github.com/kenzyfarzq/workforce-scheduling-optimization.git
cd workforce-scheduling-optimization
```
2. Create and activate a virtual environment
```bash
python -m venv venv

source venv/bin/activate          # macOS / Linux
# venv\Scripts\activate           # Windows (CMD)
# venv\Scripts\Activate.ps1       # Windows (PowerShell)
```
3. Install all dependencies

```bash
pip install -r requirements.txt
```

### Configuration

No additional configuration is required. The project is ready to run immediately after installation.

The dataset is automatically fetched from the UCI Repository when the notebook is executed using `ucimlrepo`. If the API connection is unavailable, the notebook automatically falls back to the local file `data/raw/SeoulBikeData.csv`, the same data source with no change in logic.

---

## ▶️ Usage

### Running the Notebook

```bash
jupyter notebook
# Open: notebooks/or_staffing_optimization_bikesharing.ipynb
```

Execute the cells sequentially from top to bottom. Each section includes an analytical narrative explaining the methodological decisions before and after the code is executed.

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

> Sections 11 through 15 can be re-run independently once `solve_model()` has been defined in Section 10.

---

## 🎥 Screenshots

### Hourly Demand Profile (Annual Average)

<p align="center">
  <img width="640" height="275" alt="image"
       src="https://github.com/user-attachments/assets/b75196d7-dd90-47cc-a3bd-4dc2a786ec1e" />
</p>

A bimodal pattern, peaking at 08:00 (~1,050 units) and at 18:00 (~1,554 units).

### Optimal Staff Allocation vs Requirement — Baseline

<p align="center">
  <img width="650" height="305" alt="image"
       src="https://github.com/user-attachments/assets/c0bc7aa2-ae9e-463b-bb96-0237879b36c3" />
</p>

The distribution of regular staff (blue) and supplemental staff (orange) against the actual hourly requirement.

### Tornado Chart — Parameter Sensitivity

<p align="center">
  <img width="650" height="350" alt="image"
       src="https://github.com/user-attachments/assets/fb8ad9ac-8e52-400d-9b4b-6a6d23e1eb82" />
</p>

The ranked impact of a ±20% change in each parameter on total operational cost.

### Trade-off Frontier — Cost vs Service Level

<p align="center">
  <img width="650" height="370" alt="image"
       src="https://github.com/user-attachments/assets/8edbdd39-fe0a-432f-99ce-e166d62618db" />
</p>

The optimal cost curve as a function of the target service level, across the 70%–100% range.

---

## 📈 Results and Performance

### Optimization Results by Scenario

| Scenario | Total Cost | Regular Staff | Supplemental Staff Hours |
|---|:---:|:---:|:---:|
| **Baseline** (annual average) | Rp10.512.500 | 41 people | 77 |
| Winter | Rp3.475.000 | 13 people | 28 |
| Spring | Rp10.862.500 | 43 people | 77 |
| Autumn | Rp13.437.500 | 47 people | 123 |
| Summer | Rp15.150.000 | 48 people | 160 |
| Holiday | Rp7.562.500 | 34 people | 33 |
| No Holiday | Rp10.575.000 | 42 people | 72 |

### Cost vs Service Level Trade-off

| Target Service Level | Total Cost | Savings vs 100% |
|:---:|:---:|:---:|
| 70% | Rp7.662.500 | -27,1% |
| 80% | Rp8.637.500 | -17,8% |
| 90% | Rp9.700.000 | -7,7% |
| 95% | Rp10.250.000 | -2,5% |
| **100%** | **Rp10.512.500** | **Baseline** |

### Key Findings

1. **The baseline MIP solution is optimal**, with a total cost of Rp10.512.500 per representative day, consisting of a regular cost of Rp7.625.000 and a supplemental staff cost of Rp2.887.500 (around 27,5% of the total). Of the 41 regular staff, the Night Shift absorbs the most (20 people) despite lasting only 7 hours, because it alone covers the 18:00 demand peak, which requires 32 people on its own.

2. **Cost variation between seasons reaches 4,4 times**: from Rp3.475.000 in Winter to Rp15.150.000 in Summer, consistent with a seasonal demand gap of around 4,6:1 (226 vs 1.034 units/hour on average). A static year-round staffing plan risks producing significant overstaffing in Winter and serious understaffing in Summer at the same time.

3. **Capacity per staff member is the most sensitive parameter**: a 20% reduction in capacity raises total cost by +25,3%, slightly exceeding the impact of a 20% increase in demand (+20,8%) or a 20% increase in the supplemental staff rate (+5,3%). The staff-per-shift cap has almost no effect (under 2%), which runs counter to management intuition that typically highlights the headcount cap as the main cost driver.

4. **Lowering the target service level from 100% to 70% saves 27,1% in cost** (from Rp10.512.500 to Rp7.662.500), but this saving comes with the explicit consequence of staff shortages during certain hours. This frontier curve is designed as a discussion tool with operational policy stakeholders, not as a single definitive recommendation.

5. **There is no integrality gap** between the MIP solution and the LP relaxation (both Rp10.512.500), confirming that the integer solution is already fully optimal without requiring any rounding heuristic. The shadow price on the Night Shift capacity constraint is -Rp12.500, indicating that relaxing that shift's limit could potentially reduce cost by substituting some of the more expensive supplemental staff.

> 📂 All numerical output, tables, and visualizations are available directly within the notebook.

---

## ⚠️ Limitations

- **Assumed Parameters:** All cost and operational capacity parameters (a wage of Rp25.000/hour, a capacity of 50 transactions/staff/hour, a cap of 20 staff per shift, a 1,5× premium for supplemental staff) are not derived from the dataset. These parameters are operational assumptions that are explicitly stated and documented in Section 8. Sensitivity to changes in each parameter is systematically examined in Section 13.

- **Representative Day:** The demand profile used as model input is a historical average per group (season or holiday status), not the demand of any single actual day. The hourly staffing requirement figures should be read as average estimates, not exact daily figures.

---

## 📄 License

Distributed under the **MIT License**.
See [LICENSE](LICENSE) for full details.

---

## 📬 Contact

**Ahmad Kenzy Farzaq**

[![GitHub](https://img.shields.io/badge/GitHub-kenzyfarzq-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/kenzyfarzq)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-kenzyfarzq-0A66C2?style=flat-square&logo=linkedin)](https://www.linkedin.com/in/kenzyfarzq-60b790320/)
[![Email](https://img.shields.io/badge/Email-kenzyfarzq@gmail.com-D14836?style=flat-square&logo=gmail)](mailto:kenzyfarzq@gmail.com)

<br>

> 💬 Found a bug or have a suggestion? [Open a new issue](https://github.com/kenzyfarzq/workforce-scheduling-optimization/issues/new).

---

<div align="center">
  <sub>
    ⭐ If you find this project useful, consider giving it a star!
    <br>
    Made by <a href="https://github.com/kenzyfarzq">kenzyfarzq</a> · 2026-06
  </sub>
</div>
