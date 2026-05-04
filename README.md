# Academic Performance Modeling

Analysis and modeling of academic performance to identify the key factors influencing student exam scores using linear regression in R.

---

## Objective

This project investigates which student characteristics, study habits, family background, and school environment variables are most strongly associated with exam performance. The goal is to build a parsimonious linear regression model that is both statistically valid and practically interpretable.

---

## Dataset

- **File:** `data/project.csv`
- **Size:** 5 000 students × 16 variables
- **Response variable:** `y` — exam score (continuous, 0–100)

| Variable | Type | Description |
|---|---|---|
| `y` | continuous | Exam score (response) |
| `age` | continuous | Student age |
| `study_hrs` | continuous | Weekly study hours |
| `sleep_hrs` | continuous | Daily sleep duration |
| `attend_pct` | continuous | Class attendance rate (%) |
| `sexe` | categorical | Gender (Female / Male / Other) |
| `school_type` | categorical | School type (Public / Private) |
| `web_access` | binary | Internet access at home (Yes / No) |
| `extra_act` | binary | Extracurricular activities (Yes / No) |
| `study_method` | categorical | Learning strategy used |
| `parent_educ` | ordinal | Highest parental education level |
| `sleep_qual` | ordinal | Perceived sleep quality |
| `trav_time` | ordinal | Daily commute time to school |
| `agecat` | ordinal | Age category |
| `attend_pct_cat` | ordinal | Attendance category |

---

## Project Structure

```
.
├── data/
│   └── project.csv          # Raw dataset
├── R/
│   └── run_analysis.R       # Full analysis script (standalone)
├── Projet Rapport.Rmd       # R Markdown report (source)
├── Projet-Rapport.html      # Rendered HTML report
└── README.md
```

---

## Analysis Pipeline

### 1. Data Preparation
- Integrity checks: no duplicates, plausible value ranges, missing values
- Recoding of numeric codes into labelled factors (e.g. gender, school type)
- Ordinal variables set as ordered factors with explicit reference categories
- Removal of redundant derived variables

### 2. Exploratory Data Analysis (EDA)
- Descriptive statistics for quantitative variables
- Distribution of the response variable (histogram + boxplot)
- Relationships between `y` and each predictor: LOESS smooths for quantitative variables, boxplots/violin plots for categorical and ordinal variables
- Correlation matrix among quantitative predictors
- Chi-square tests and grouped summaries to detect associations between predictors and anticipate multicollinearity

**Key EDA findings:**
- `attend_pct` is the strongest single predictor (r ≈ 0.74)
- `study_hrs`, `parent_educ`, `sleep_qual`, and `trav_time` show clear monotone trends
- `sexe` shows near-zero association with exam scores
- No severe multicollinearity detected (all pairwise correlations < 0.7)
- The response variable is approximately symmetric — no transformation required

### 3. Model Building

Simple linear regressions are first estimated for each predictor individually (R² comparison), then four multiple regression models are compared:

| Model | Predictors |
|---|---|
| `mod0` | `attend_pct`, `study_hrs`, `age`, `sleep_hrs` |
| `mod1` | mod0 + `parent_educ`, `sleep_qual`, `trav_time`, `school_type`, `web_access` |
| `mod2` | mod1 + `extra_act`, `study_method` |
| `mod3` | mod1 + interaction `attend_pct × study_hrs` |

### 4. Model Selection

Models are compared using:
- **Adjusted R²**, **AIC**, **BIC** (penalise complexity)
- **Nested F-tests** to assess whether additional predictors or interactions significantly improve fit
- **80/20 train-test split** (seed = 42) for out-of-sample predictive evaluation

**Selected model: `mod1`** — best trade-off between explanatory power, parsimony, and interpretability.
- The interaction term in `mod3` was not significant (F = 2.60, p = 0.107)
- `extra_act` and `study_method` in `mod2` added minimal predictive value

### 5. Diagnostics
- **Residual plots** and **QQ-plot** to assess normality
- **Breusch-Pagan test** for homoscedasticity (p > 0.05 → homoscedasticity confirmed)
- **Shapiro-Wilk test** on residuals (large sample caveat noted; QQ-plot consistent with approximate normality)

---

## Results

### Final Model (`mod1`)

```
y ~ attend_pct + study_hrs + age + sleep_hrs +
    parent_educ + sleep_qual + trav_time +
    school_type + web_access
```

| Metric | Value |
|---|---|
| R² (in-sample) | ~0.71 |
| Adjusted R² | ~0.71 |
| Test R² (out-of-sample) | 0.713 |
| RMSE (test set) | ~8 points |
| Median Absolute Error | ~5.4 points |
| MSE reduction vs. baseline | 71.3% |

### Key Coefficient Findings
- **Attendance rate** has the largest positive effect — the strongest driver of exam performance
- **Study hours** have a clear positive but more moderate effect
- **Private school** students score meaningfully higher than public school students (all else equal)
- **Higher parental education** (up to PhD) is associated with significantly better scores
- **Good sleep quality** is positively associated with performance compared to poor sleep
- **Long commute (> 60 min)** is associated with ~3.6 point lower scores than short commute (< 15 min)
- **Gender** was excluded — near-zero explanatory power in simple regressions

---

## How to Run

### Requirements

R (≥ 4.1) with the following packages:

```r
install.packages(c(
  "tidyverse", "collapse", "ggpubr", "corrplot",
  "performance", "ggfortify", "tibble", "lmtest",
  "broom", "gridExtra", "car"
))
```

### Run the analysis

```r
source("R/run_analysis.R")
```

### Render the report

```r
rmarkdown::render("Projet Rapport.Rmd")
```

The rendered HTML report (`Projet-Rapport.html`) contains all outputs, figures, and interpretations.

---

## Reproducibility

All results are reproducible with `set.seed(42)` applied before the train/test split.
