# Analysis of Coffee Consumption Behavior and Its Effect on Students' Study Focus

### Analyzing the coffee consumption patterns of `165` students using Python to measure its effect on study duration and focus

[![Streamlit App](https://img.shields.io/badge/Streamlit-Live%20Demo-FF4B4B?style=flat-square&logo=streamlit&logoColor=white)](https://kenzydarderdor.streamlit.app/)

## 📝 Table of Contents

1. [Overview](#-overview)
2. [Problem Statement](#-problem-statement)
3. [Dataset](#-dataset)
4. [Tech Stack](#-tech-stack)
5. [Demo and Screenshots](#-demo-and-screenshots)
6. [Results and Performance](#-results-and-performance)
7. [Limitations](#-limitations)
---

## 📌 Overview

This project is an exploratory study based on primary survey data, analyzing the coffee consumption patterns of university students and their relationship with study duration and focus. Data was collected independently through a Google Form questionnaire distributed to `165` students at Universitas Pembangunan Nasional "Veteran" Jawa Timur, then cleaned using custom text parsing functions before moving into visual exploration, correlation analysis, cross-tabulation, and linear regression. This project is intended as a reference for researchers, data practitioners, or anyone interested in data-driven studies of student consumption behavior. The primary outputs are two analysis notebooks, a set of exploratory visualizations, and a regression model that quantifies the strength of the relationship between the subjective effects of coffee and perceived study focus.

## ❓ Problem Statement

**Context:** Coffee is a daily staple for many students during their academic life, especially when working on assignments, but consumption patterns, spending, and the effects on learning ability are rarely measured systematically on campus. An internal survey of `165` students revealed highly varied responses, ranging from the number of cups per day to perceptions of improved focus, making it difficult to draw conclusions without structured data processing.

**Gap:** No quantitative picture existed linking coffee habits (frequency, acquisition method, location, timing) to their impact on student study duration and focus in this environment, nor was there any statistical testing of the common assumption that coffee improves focus.

**Solution:** This project cleans and structures the raw survey data through custom text parsing, then performs visual exploration, correlation analysis, cross-tabulation, and linear regression to statistically measure the extent to which coffee's effects influence perceived study focus. The result is a behavioral map of student coffee consumption along with the strength of its relationship to study productivity, serving as a useful reference for anyone interested in consumption behavior studies in a campus setting.

## 📊 Dataset

### Metadata

| Attribute | Detail |
|---|---|
| **Source** | Primary data, collected independently via Google Form survey |
| **Respondent Population** | UPN Veteran Jawa Timur students across faculties (`165` respondents) |
| **Size** | `165` rows × `18` columns (before feature engineering) |
| **Format** | CSV |
| **License** | Primary data, not published in raw form (see privacy note below) |
| **Time Coverage** | Around September 2025 (based on available timestamp sample) |
| **Sampling Method** | Convenience sampling (non-random) |

### Data Dictionary (Key Columns)

| Column | Data Type | Description | Example Value |
|---|:---:|---|---|
| `gelas_num` | float | Number of cups of coffee consumed per day, parsed from free-text responses | `1.0` |
| `pengeluaran_num` | float | Weekly coffee spending in Rupiah | `15000.0` |
| `durasi_num` | float | Study duration sustained after drinking coffee (hours) | `3.0` |
| `frekuensi_cafe_num` | int | Frequency of studying or hanging out at a café per week | `2` |
| `efek_kopi_num` | int | Ordinal encoding of the subjective effect of coffee; predictor in the regression model | `1` |
| `fokus_num` | float | **Dependent variable in the regression model** — perceived improvement in study focus (`0` to `100%`) | `50.0` |

## 🛠️ Tech Stack

| Layer | Technology | Role in the Project |
|:---:|:---:|---|
| Language | Python 3.x | Main language for the entire pipeline, from data cleaning to regression modeling |
| Environment | Google Colab (Jupyter Notebook) | Interactive execution and output composition |
| Data Processing | Pandas, NumPy, `re` (regex) | Parsing free-text responses into numerical values, IQR-based outlier detection and handling, categorical variable encoding |
| Visualization | Matplotlib, Seaborn | Boxplots, histogram + KDE, correlation heatmap, pairplot, and regression diagnostic plots |
| Statistical Analysis | SciPy (`stats`), Scikit-learn (`LinearRegression`, `r2_score`, `mean_squared_error`) | Linear regression, manual t-statistic and p-value computation, and residual diagnostic testing |
| Version Control | Git + GitHub | Source control and change documentation |

## 🎥 Demo and Screenshots

### Live Interactive Dashboard

👉 https://kenzydarderdor.streamlit.app/

> Explore the dashboard interactively to filter, visualize, and inspect the survey results before viewing the screenshots below.

### Consumption Pattern Summary

<p align="center">
  <img width="942" height="590" alt="image" 
     src="https://github.com/user-attachments/assets/6ac7901d-e5e5-4240-b248-102a8ce28319" />
</p>

Six summary charts covering coffee habits: frequency, spending, acquisition method, favorite location, drinking time, and café visit frequency.

### Regression Model Diagnostics

<p align="center">
  <img width="839" height="589" alt="image" 
    src="https://github.com/user-attachments/assets/f1f5e005-d296-427e-b01e-61ed41a3195b" />
</p>

Regression scatter plot, residual plot, Q-Q plot, and residual histogram for the model `Fokus = 34.47 + 11.71 × Efek Kopi`.

## 📈 Results and Performance

### Key Findings

1. **The majority of consumption patterns are moderate.** A total of `64.8%` of students (`107` out of `165`) drink only `1 cup` of coffee per day, with a median weekly spending of `Rp15,000`, indicating controlled and non-excessive habits.

2. **Coffee functions as a study aid, not a social activity.** `70.3%` of respondents drink coffee while working on assignments, while only `3.0%` view it as a social activity, confirming that its primary function is productivity, not socializing.

3. **Study duration is the numerical variable most strongly correlated with focus.** Among all numerical variable pairs, Study Duration and Focus have the highest correlation (`r = 0.42`), higher than the correlation between Number of Cups and Focus (`r = 0.29`), suggesting that study intensity plays a greater role in perceived focus than the amount of coffee consumed.
<p align="center">
   <img width="462" height="392" alt="image" 
     src="https://github.com/user-attachments/assets/92406c36-4fc2-4f90-8e52-7d8a9da7f722" />
</p>
   *Correlation matrix of five transformed numerical variables, showing Study Duration and Focus as the pair with the highest correlation.*

4. **Coffee's effect is statistically significant on focus, but its explanatory power is limited.** The regression model `Fokus = 34.47 + 11.71 × Efek Kopi` is significant (`p < 0.001`, Pearson correlation `r = 0.4892`), but it explains only `R² = 0.2393` (`23.9%`) of the variation in focus, meaning coffee's effect is real but not the sole dominant factor.

5. **Coffee acquisition method is a clear differentiator of spending.** Students who buy coffee at cafés or warungs spend an average of `Rp31,207` per week, around `60%` more than those who make it at home (`Rp19,476` per week).

6. **Drinking coffee is seen more as a routine habit than a necessity.** `52.1%` of students regard drinking coffee as merely a habit, compared to `35.8%` who see it as an essential need. Both groups have similar consumption patterns (mode of `1 cup` per day), so the difference lies in attitude, not behavior.
<p align="center">
   <img width="829" height="352" alt="image" 
     src="https://github.com/user-attachments/assets/c225f747-50de-4081-ad3f-5f29d6710df2" />
</p>
   *Distribution of what coffee means to students (left) and its relationship to weekly spending categories (right).*

## ⚠️ Limitations

- **Scope:** This analysis covers active students at UPN Veteran Jawa Timur who voluntarily completed the survey during the data collection period, and is not intended to represent the Indonesian student population in general.

- **Data Privacy:** The raw data contains respondent identity information (name, student ID, email, and WhatsApp number) and is therefore not included in this repository.

- **Generalization:** An `R² = 0.2393` indicates that coffee's effect explains only a small portion of the variation in perceived study focus, so the relationship found is correlational rather than causal, and has not been validated on other populations or time periods.
