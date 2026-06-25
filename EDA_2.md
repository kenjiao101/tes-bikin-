# Digital Divide Indonesia: Statistical Analysis of the Relationship Between Poverty and Internet Access Across Provinces

An applied statistics study examining the digital divide in Indonesia in 2024, using Pearson correlation, linear regression with full classical assumption testing, and chi-square testing with Cramer's V and Fisher's Exact Test.

## 📝 Table of Contents

1. [Overview](#-overview)
2. [Problem Statement](#-problem-statement)
3. [Objectives](#-objectives)
4. [Dataset](#-dataset)
5. [Tech Stack](#-tech-stack)
6. [Methodology](#-methodology)
7. [Workflow](#-workflow)
8. [Results and Performance](#-results-and-performance)
9. [Getting Started](#-getting-started)
10. [Limitations](#-limitations)
---

## 📌 Overview

This project examines whether the poverty rate in a province is associated with the household internet access rate in that province, using official BPS (Statistics Indonesia) data from 2024 covering 37 provinces in Indonesia. The analysis is carried out in six sequential stages: data collection, data cleaning and merging, data exploration, variable categorization, regression modeling with classical assumption testing, and chi-square testing with effect size and corrective tests when statistical assumptions are not met.

What sets this project apart from a simple correlation analysis is its methodological consistency. Whenever a statistical assumption fails to be met (homoscedasticity, minimum expected frequency in the contingency table), this project does not ignore it, but instead switches to a more appropriate method (Fisher's Exact Test) or explicitly records it as a model limitation.

## ❓ Problem Statement

Internet access is often assumed to be an indicator of digital equality, but its distribution in Indonesia is suspected to be uneven and correlated with the socioeconomic conditions of a region. Questions this study aims to answer:

- Is there a statistically significant relationship between a province's poverty rate and its household internet access rate?
- If so, how strong is the relationship, and is the result consistent when tested using both a numerical approach (regression/correlation) and a categorical approach (chi-square)?
- Which provinces deviate the most from the general pattern, and what are the implications for the national digital divide?

## 🎯 Objectives

- Measure the strength and direction of the relationship between poverty percentage and internet access percentage using Pearson correlation and simple linear regression.
- Validate this relationship from a categorical data perspective using chi-square testing, Cramer's V, and Fisher's Exact Test.
- Identify the provinces that are the main drivers behind the significance of this relationship.
- Present all results with full transparency regarding violations of statistical assumptions, rather than reporting only significant findings.

## 📊 Dataset

Two official datasets from BPS (Statistics Indonesia), 2024:

| Dataset | Description | Initial Dimensions | Link |
|---|---|---|---|
| Internet Access | Percentage of households that accessed the internet in the last 3 months, by province and area classification (urban/rural/total) | 39 rows x 4 columns | [BPS - Internet Access](https://www.bps.go.id/id/statistics-table/2/Mzk4IzI=/persentase-rumah-tangga-yang-pernah-mengakses-internet-dalam-3-bulan-terakhir-menurut-provinsi-dan-klasifikasi-daerah.html) |
| Poverty | Percentage of poor population (P0) by province and area | 39 rows x 10 columns | [BPS - Poor Population](https://www.bps.go.id/id/statistics-table/2/MTkyIzI=/persentase-penduduk-miskin--p0--menurut-provinsi-dan-daerah.html) |

Data collection notes:
- From the poverty dataset, the column used corresponds to the March 2024 (Semester I) period, chosen to maintain temporal consistency with the internet access survey period.
- The raw CSV files are not included directly in this repository. Download them from the two links above, then place them in `data/raw/`. It is recommended to rename the files to something more concise, such as `internet_access_2024.csv` and `poverty_rate_2024.csv`, and adjust the path in the notebook accordingly.

## 🛠️ Tech Stack

- Python 3
- pandas, numpy (data manipulation)
- matplotlib, seaborn (visualization)
- scipy.stats (statistical testing: Pearson, Shapiro-Wilk, chi-square, Fisher's Exact Test, and others)
- scikit-learn (LinearRegression, r2_score, for inferential purposes)
- Google Colab (original development environment)

## 🔬 Methodology

The statistical approach was chosen based on the type of data and the need for cross-validation:

1. **Pearson Correlation** to measure the strength and direction of the linear relationship between two continuous numerical variables.
2. **Simple Linear Regression (OLS)** to measure how much of the variation in internet access can be explained by the poverty rate, accompanied by model significance testing (F-test) and coefficient testing (t-test).
3. **Classical regression assumption tests**: residual normality (Shapiro-Wilk), homoscedasticity (Breusch-Pagan), and autocorrelation (Durbin-Watson), to ensure the validity of model interpretation rather than simply reporting R².
4. **Quartile-based categorization** to convert both numerical variables into Low/Medium/High categories, serving as the basis for categorical analysis.
5. **Chi-square test of independence** and **Cramer's V** to validate the relationship from a categorical perspective, as an independent comparison to the regression results.
6. **Fisher's Exact Test** used as a substitute for chi-square on 2x2 tables when the minimum expected frequency assumption (≥5) is not met in the majority of contingency table cells.
7. **Post-hoc pairwise comparison with Bonferroni correction** to identify which category pairs are individually significant after the overall chi-square test proves significant.
8. **Goodness-of-fit test** to check whether the category distributions for poverty and internet access are independently uniform or non-uniform, separate from the relationship between the two.

## 📐 Workflow

The notebook follows six sequential sections, designed to be run from top to bottom without skipping:

1. **Data Collection**: importing libraries, uploading two CSV files, checking initial dimensions.
2. **Data Cleaning and Transformation**: renaming columns, removing national aggregate rows ("INDONESIA"), standardizing province names, data type conversion, merging datasets (inner join), handling missing values, checking for duplicates and outliers (IQR method).
3. **Data Exploration**: descriptive statistics, univariate and bivariate analysis, identifying top/bottom provinces, outlier detection (Z-score).
4. **Variable Definition and Categorization**: determining dependent/independent variables, quartile-based categorization, crosstabulation, category distribution visualization.
5. **Predictive Modeling and Correlation Testing**: Pearson correlation, linear regression, F-test and t-test, classical assumption testing, diagnostic visualization.
6. **Chi-Square Testing**: independence testing, Cramer's V, Fisher's Exact Test, post-hoc pairwise comparison, goodness-of-fit test, comparison of categorical vs. numerical results.

## 📈 Results and Performance

### Data Summary

| Variable | Mean | Median | Std Dev | Range |
|---|---|---|---|---|
| Internet Access (%) | 85.95 | 90.17 | 16.09 | 12.15 - 97.57 |
| Poverty (%) | 11.33 | 10.47 | 6.74 | 4.00 - 32.97 |

<p align="center">
  <img width="720" height="570" alt="image"
       src="https://github.com/user-attachments/assets/ea500da9-f791-4c35-a150-e90a1c51d008" />
</p>

### Relationship Between Poverty and Internet Access

| Method | Statistic | p-value | Conclusion |
|---|---|---|---|
| Pearson Correlation | r = -0.8215 (R² = 0.6748) | p < 0.001 | Very strong, significant negative relationship |
| Linear Regression | F = 72.62, t(slope) = -8.52 | p < 0.001 | Significant model; poverty explains 67.48% of the variation in internet access |
| Chi-Square Test of Independence | χ² = 20.18, df = 4 | p < 0.001 | Poverty and internet access categories are significantly related |
| Cramer's V | V = 0.5222 | - | Strength of categorical association: very strong |
| Fisher's Exact Test (2x2) | Odds Ratio = 0 | p = 0.036 | No province has both high poverty and high internet access |

Resulting regression equation:

```
Internet_Total = 108.16 + (-1.96) x Kemiskinan_Persen
```

Each 1% increase in poverty rate is associated with an average decrease of 1.96% in internet access (95% CI: -2.43 to -1.49), valid only within the observed data range (4.00% - 32.97%). The intercept (108.16%) falls outside the 0-100% range because it is an extrapolation beyond the observed data, and therefore carries no substantive meaning.

<p align="center">
  <img width="680" height="400" alt="image"
       src="https://github.com/user-attachments/assets/99897e03-e1c7-4c1e-8c96-66984171e6b5" />
</p>

<p align="center">
  <img width="680" height="400" alt="image"
       src="https://github.com/user-attachments/assets/b68772db-fea0-4154-b5af-bb4277972460" />
</p>

### Regression Assumption Testing

| Assumption | Test | Result | Status |
|---|---|---|---|
| Residual normality | Shapiro-Wilk | W = 0.9494, p = 0.092 | Met |
| Homoscedasticity | Breusch-Pagan | LM = 20.43, p < 0.001 | Not met |
| No autocorrelation | Durbin-Watson | DW = 1.04 | Not met (positive autocorrelation) |

<p align="center">
  <img width="720" height="570" alt="image"
       src="https://github.com/user-attachments/assets/2538bc2a-e996-4844-8c0c-2db73259dd6e" />
</p>

Two of the three classical regression assumptions are not fully met. The interpretation of the coefficient (direction and significance) remains valid, but the reported standard errors and confidence intervals are potentially biased due to heteroscedasticity and autocorrelation. See the Limitations section.

### Categorization and Categorical Analysis

Both variables were categorized into Low/Medium/High based on quartiles (Q1 and Q3).

<p align="center">
  <img width="700" height="300" alt="image"
       src="https://github.com/user-attachments/assets/262cf076-6046-4277-9645-c69db167ed0b" />
</p>

<p align="center">
  <img width="400" height="350" alt="image"
       src="https://github.com/user-attachments/assets/4bcc40f4-375e-4d83-b007-3f1548e730af" />
</p>

The contingency table shows that 77.8% of cells have an expected frequency below 5, so the 3x3 chi-square test was supplemented with Fisher's Exact Test on a 2x2 table (categories merged into High vs. Low-Medium). The cell with the largest contribution to the chi-square value is the combination of **High Poverty x Low Internet Access** (7 provinces observed, 2.43 provinces expected, contributing 42.50% of the total chi-square).

<p align="center">
  <img width="720" height="570" alt="image"
       src="https://github.com/user-attachments/assets/894cf4e6-83a9-4f1f-ab7f-0d7ee7ab9ad8" />
</p>

<p align="center">
  <img width="700" height="300" alt="image"
       src="https://github.com/user-attachments/assets/aa7cc52e-8867-4aab-adfe-d7d8c3f29fbd" />
</p>

### Post-Hoc and Goodness-of-Fit

Post-hoc pairwise comparison (Bonferroni correction, corrected alpha = 0.0056) shows that the High Poverty category differs significantly from both Low and Medium, while the High Internet Access category differs significantly only from the Low category. The goodness-of-fit test shows that the category distributions for both variables individually do not differ significantly from a uniform distribution (p = 0.139 for Internet, p = 0.062 for Poverty), meaning the significant pattern stems purely from the relationship between the two variables, not from an imbalance in either variable's distribution.

### Provinces with Extreme Patterns

| Category | Province | Value |
|---|---|---|
| Highest internet access | Kep. Riau | 97.57% |
| Lowest internet access | Papua Pegunungan | 12.15% |
| Highest poverty | Papua Pegunungan | 32.97% |
| Lowest poverty | Bali | 4.00% |

Papua Pegunungan and Papua Tengah were detected as significant outliers (Z-score > 3) on both variables, and are the main drivers of the strength of the observed relationship.

### Final Conclusion

The poverty rate and internet access across provinces in Indonesia in 2024 are negatively, strongly, and statistically significantly related, consistently shown by both the numerical approach (Pearson r, regression) and the categorical approach (chi-square, Cramer's V, Fisher's Exact Test). Not a single province was found with a combination of both high poverty and high internet access, indicating a real digital divide between regions rather than mere random variation.

## 🚀 Getting Started

1. Clone the repository:
```bash
git clone https://github.com/kenzyfarzq/digital-divide-indonesia.git
cd digital-divide-indonesia
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

4. Download the two datasets from the links in the [Dataset](#-dataset) section, then place them in `data/raw/`.

5. **Important, reproducibility note**: the original notebook was developed in Google Colab and uses the file upload widget (`google.colab.files.upload()`) in section 1.2. To run it locally (Jupyter/VS Code), replace that cell with direct file reading from a local folder:

```python
df_internet_raw = pd.read_csv('data/raw/internet_access_2024.csv', skiprows=3, encoding='utf-8')
df_kemiskinan_raw = pd.read_csv('data/raw/poverty_rate_2024.csv', skiprows=4, encoding='utf-8')
```

6. Run the notebook:
```bash
jupyter notebook notebooks/digital_divide_statistical_analysis.ipynb
```
Execute the cells sequentially from top to bottom.

## ⚠️ Limitations

- **Two classical regression assumptions are not met** (homoscedasticity and non-autocorrelation). The coefficient and the direction of the relationship remain valid, but the resulting standard errors and p-values are slightly biased.
- **Small sample size**, so the results are susceptible to being influenced by a small number of extreme provinces (Papua Pegunungan and Papua Tengah were detected as outliers on both variables).
- **Single-year cross-sectional data (2024)**, so it cannot be used to make claims about trends or causal relationships, only association at a single point in time.
- **The notebook depends on Google Colab** for data input, so it cannot yet be run directly outside Colab without minor modifications (see the [Getting Started](#-getting-started) section).
