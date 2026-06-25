# Multivariate Analysis of Wine Characteristics

### Analyzing the chemical profile differences of three Italian wine cultivars using a multivariate statistics pipeline on the UCI Wine Dataset to statistically identify the main discriminating variables.

## 📝 Table of Contents

1. [Overview](#-overview)
2. [Problem Statement](#-problem-statement)
3. [Dataset](#-dataset)
4. [Tech Stack](#-tech-stack)
6. [Screenshots](#-screenshots)
7. [Results and Performance](#-results-and-performance)
8. [Getting Started](#-getting-started)
9. [Limitations](#-limitations)
---

## 📌 Overview

This project is a multivariate statistical analysis of the Wine Dataset from the UCI Machine Learning Repository, containing 178 samples of Italian wine from three different cultivars with 13 chemical variables. The analysis pipeline covers data exploration, multivariate assumption testing (normality and covariance homogeneity), formal hypothesis testing (Hotelling's T² and MANOVA), as well as further analysis using PCA and LDA. The primary audience for this analysis is technical reviewers, academics, and anyone interested in seeing a systematic implementation of multivariate statistics within a single structured workflow. The output is a fully annotated notebook with 14 visualizations, summary tables of results for each statistical test, and an interpretation of the findings from both a statistical and practical standpoint.

## ❓ Problem Statement

**Context:** The Wine Dataset from the UCI ML Repository (DOI: 10.24432/C5PC7J) contains the chemical analysis results of 178 Italian wine samples originating from three different cultivars within the same region. This dataset has 13 continuous chemical variables as features and one categorical target variable (1, 2, 3). The central question is whether the three wine classes have significantly different multivariate chemical profiles, and if so, which variables contribute most to that difference.

**Gap:** Although this dataset is frequently used as a machine learning classification benchmark, a systematic multivariate statistical analysis covering formal assumption testing, hypothesis testing with multiple methods, effect size quantification, and identification of the key discriminating variables is rarely carried out comprehensively within a single coherent and well-documented analytical workflow.

**Solution:** This project implements an end-to-end multivariate analysis pipeline, covering: comprehensive EDA, multivariate normality testing via the Henze-Zirkler Test, multivariate outlier detection via Mahalanobis Distance, Hotelling's T² for pairwise class comparisons, Box's M Test for covariance homogeneity, MANOVA with Wilks' Lambda along with partial η², post-hoc univariate ANOVA, as well as PCA and LDA as further analysis. The result is strong statistical evidence that the three wine classes differ significantly, with Flavanoids, Alcohol, and Total_phenols as the most discriminative variables.

## 📊 Dataset

### Metadata

| Attribute | Detail |
|---|---|
| **Source** | UCI Machine Learning Repository |
| **Link** | [Wine Dataset (ID: 109)](https://archive.ics.uci.edu/dataset/109/wine) |
| **DOI** | [10.24432/C5PC7J](https://doi.org/10.24432/C5PC7J) |
| **Size** | `178` rows × `14` columns (`13` features + `1` target) |
| **Format** | Accessed via API (`ucimlrepo.fetch_ucirepo(id=109)`) — no local file required |
| **License** | CC BY 4.0 |
| **Dataset Creation Year** | 1992 |
| **Creator(s)** | Stefan Aeberhard, M. Forina |
| **Class Distribution** | Class 1: `59` obs (`33.1%`) · Class 2: `71` obs (`39.9%`) · Class 3: `48` obs (`27.0%`) |
| **Missing Values** | None (`0` out of `178` observations) |

### Data Dictionary (Key Columns)

| Column | Data Type | Description | Example Value |
|---|:---:|---|---|
| `class` | `int` | **Target variable** — wine class label based on cultivar (`1`, `2`, or `3`) | `1` |
| `Flavanoids` | `float` | Main phenol subgroup affecting wine color and flavor (g/L) — **most discriminative variable** (η² = `0.7278`) | `2.76` |
| `Alcohol` | `float` | Alcohol content (%) — 2nd strongest discriminator (η² = `0.6069`) | `13.20` |
| `Total_phenols` | `float` | Total phenolic compounds, related to bitterness and antioxidant properties (g/L) — 3rd strongest discriminator (η² ≈ `0.517`) | `2.80` |
| `Proline` | `int` | Proline amino acid content (mg/L) — variable with the largest variance (std = `314.91`) | `1050` |
| `0D280_0D315_of_diluted_wines` | `float` | OD280/OD315 ratio for protein and phenol purity — strongly correlated with Flavanoids (r = `0.79`) | `3.40` |
| `Color_intensity` | `float` | Wine color intensity measured photometrically | `4.38` |
| `Malicacid` | `float` | Malic acid content (g/L) — 4th discriminator (η² ≈ `0.297`) | `1.78` |

## 🛠️ Tech Stack

| Layer | Technology | Role in the Project |
|:---:|:---:|---|
| Language | Python 3.12 | Main language for the entire analysis pipeline |
| Environment | Google Colab / Jupyter Notebook | Interactive execution, step-by-step exploration, and annotated output |
| Data Access | ucimlrepo `0.0.7` | Fetches the dataset directly from the UCI ML Repository via API — no local CSV file needed |
| Data Processing | Pandas `2.2.2`, NumPy `2.0.2` | DataFrame manipulation, covariance matrix calculations, and statistical transformations |
| Statistical Testing | SciPy `1.16.3`, Pingouin `0.6.1`, Statsmodels `0.14.6` | Henze-Zirkler test, Hotelling's T², Box's M Test, and MANOVA (Wilks' Lambda) |
| Visualization | Matplotlib `3.10.0`, Seaborn `0.13.2` | 14 visualizations: histogram+KDE, boxplot, correlation heatmap, pairplot, PCA/LDA scatter plots, and confusion matrix |
| ML / Dimensionality Reduction | Scikit-learn `1.6.1` | StandardScaler for normalization, PCA for dimensionality reduction, LDA for discriminant analysis |
| Version Control | Git + GitHub | Source control and portfolio publishing |

## 🎥 Screenshots

### Correlation Heatmap Across Chemical Variables

<p align="center">
  <img width="480" height="380" alt="image"
       src="https://github.com/user-attachments/assets/c64e78fc-37dc-4706-a9c3-c65a9064597a" />
</p>

The 13×13 heatmap, using an RdYlGn colormap, reveals six variable pairs with strong correlation (r > 0.60), most notably a cluster of phenolic variables among Flavanoids, Total_phenols, and OD280/OD315.

### Class Separation in 2D PCA Space

<p align="center">
  <img width="700" height="280" alt="image"
       src="https://github.com/user-attachments/assets/1781b690-600b-4a7c-93fd-468ab574558c" />
</p>

The 2D PCA scatter plot shows PC1 explaining `36.20%` and PC2 explaining `19.21%` of the variance. Separation between the three classes is already visible in two-dimensional space.

### Standardized Mean Profile per Class (MANOVA)

<p align="center">
  <img width="700" height="280" alt="image"
       src="https://github.com/user-attachments/assets/507b9ba9-e706-4eda-b270-c9e012420f9d" />
</p>

This line plot shows the standardized (z-score) mean profile for the three wine classes. Class 1 is dominantly high in Flavanoids and Alcohol, while Class 3 is dominantly low. The sharpest distinguishing pattern appears in the phenolic variables.

### LDA: Class Separation and Confusion Matrix

<p align="center">
  <img width="700" height="280" alt="image"
       src="https://github.com/user-attachments/assets/cfec45df-3207-4a0b-9292-dfdc2e985fc7" />
</p>

The 2D LDA scatter plot, together with a confusion matrix showing a fully correct diagonal (59/59, 71/71, 48/48), reflects a resubstitution accuracy of `100.00%` using all 13 features.

## 📈 Results & Performance

### Key Findings

1. **All three wine classes differ significantly across every pairwise combination**. Hotelling's T² produced extremely small p-values for all three pairs: Class 1 vs 2 (`T² = 799.66`, `p = 1.35e-43`), Class 1 vs 3 (`T² = 3075.74`, `p = 8.15e-63`), and Class 2 vs 3 (`T² = 800.30`, `p = 7.46e-41`). The largest multivariate difference occurs between Class 1 and Class 3, with a T² value almost 4 times larger than the other two pairs.

2. **MANOVA confirms a strong difference with a very large effect size**. `Wilks' Lambda = 0.2067`, `F = 93.21`, `p = 7.55e-55`. `Partial η² = 0.7933`, meaning more than `79.33%` of the combined variability in the chemical variables can be explained by differences between wine classes. This value far exceeds the "Large Effect Size" threshold (`η² > 0.14`) under Cohen's criteria.

3. **Flavanoids, Alcohol, and Total_phenols are the main discriminating variables**. Post-hoc univariate ANOVA shows Flavanoids (`η² = 0.7278`, `F = 233.93`) as the most discriminative variable between classes, followed by Alcohol (`η² = 0.6069`, `F = 135.08`) and Total_phenols (`η² ≈ 0.517`). All 7 variables tested show significant differences (`p < 0.05`).

4. **There are 6 variable pairs with strong correlation (|r| > 0.60)**. Flavanoids and Total_phenols have the highest Pearson correlation (`r = 0.8646`), followed by Flavanoids-OD280/OD315 (`r ≈ 0.79`) and Total_phenols-OD280/OD315 (`r ≈ 0.70`). This cluster of strong correlations is consistent with their chemical origin as a related group of phenolic compounds.

5. **Multivariate assumptions are not met, but the analysis remains defensible**. Henze-Zirkler Test: `HZ = 1.0743`, `p ≈ 0.000` (not multivariate normal). Box's M Test: `χ² = 209.99`, `df = 56`, `p = 1.15e-19` (covariance not homogeneous). Both conditions are addressed on the grounds that the sample size `n = 178` is sufficiently large and the class distribution is relatively balanced (the CLT applies), along with the presence of `12` multivariate outliers (`6.74%`) that do not dominate the data.

6. **PCA and LDA confirm class separability both visually and quantitatively**. The 2-component PCA captures `55.41%` of the overall variance (`PC1 = 36.20%`, `PC2 = 19.21%`), with fairly clear visual separation between classes. LDA produces optimal separation with LD1 explaining `68.75%` of the between-class variance. Resubstitution accuracy reaches `100.00%` (see limitations below).

## 🚀 Getting Started

> This project is part of a larger collection repository. Clone the collection repo first (see the root README), then navigate into this folder.

1. Navigate to this project folder:
```bash
cd multivariate-statistical-analysis
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

4. Run the notebook:
```bash
jupyter notebook notebooks/multivariate_statistical_analysis_wine.ipynb
```
Execute the cells sequentially from top to bottom.

## ⚠️ Limitations

- **Scope:** This analysis covers a single specific dataset containing `178` Italian wine samples from one geographic region. These findings are not intended to be generalized to wines from other regions, countries, or production processes, since the chemical characteristics of wine are heavily influenced by terroir.

- **Analysis Assumptions:** The assumptions of multivariate normality (Henze-Zirkler, `p ≈ 0.000`) and covariance homogeneity (Box's M, `p = 1.15e-19`) are not met. MANOVA and Hotelling's T² were still carried out on the grounds that `n = 178` is large enough to invoke the Central Limit Theorem and that the class distribution is relatively balanced. This does not affect the direction of the conclusions, but it should be taken into account in the context of formal inference.

- **LDA Resubstitution Accuracy:** The `100.00%` LDA accuracy is a resubstitution result, meaning the model was evaluated on the same data used for training. This figure is optimistic and likely to be lower on new data. It reflects the strength of feature separability rather than an estimate of the model's generalization ability.

- **Generalization:** The findings in this project have not been validated against other wine samples outside this UCI dataset. External validation with independent data is required before these findings are used as a basis for analytical decisions outside the context of this dataset.
