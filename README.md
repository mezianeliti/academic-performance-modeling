# Academic Performance Modeling

Analysis and modeling of academic performance to identify the key factors influencing student exam scores using linear regression in R.

---

## Objective

Identify which student characteristics, study habits, family background, and school environment variables are most strongly associated with exam performance, and build a parsimonious, interpretable linear regression model.

---

## Dataset

- **File:** `data/project.csv` — 5 000 students × 16 variables
- **Response variable:** `y` — exam score (0–100)

Key predictors: `attend_pct`, `study_hrs`, `sleep_hrs`, `age`, `parent_educ`, `sleep_qual`, `trav_time`, `school_type`, `web_access`, `sexe`, `extra_act`, `study_method`

---

## Project Structure

```
.
├── data/project.csv          # Raw dataset
├── R/run_analysis.R          # Full analysis script
├── Projet Rapport.Rmd        # R Markdown report
├── Projet-Rapport.html       # Rendered HTML report
└── README.md
```

---

## Analysis Pipeline

1. **Data preparation** — integrity checks, factor recoding, removal of redundant variables
2. **EDA** — descriptive stats, LOESS smooths, boxplots, correlation matrix, chi-square tests
3. **Model building** — simple regressions per predictor, then 4 multiple regression models compared
4. **Model selection** — Adjusted R², AIC, BIC, nested F-tests, 80/20 train-test split
5. **Diagnostics** — residual plots, QQ-plot, Breusch-Pagan test, Shapiro-Wilk test

**Selected model: `mod1`**
```
y ~ attend_pct + study_hrs + age + sleep_hrs +
    parent_educ + sleep_qual + trav_time + school_type + web_access
```

---

## Results

| Metric | Value |
|--------|-------|
| R² (in-sample) | ~0.71 |
| Test R² (out-of-sample) | 0.713 |
| RMSE (test set) | ~8 pts |
| Median Absolute Error | ~5.4 pts |

**Key findings:** attendance rate is the strongest driver (r ≈ 0.74); private school, higher parental education, and good sleep quality are positively associated with scores; long commute (> 60 min) reduces scores by ~3.6 pts; gender showed near-zero explanatory power.

---

## How to Run

```r
install.packages(c("tidyverse", "collapse", "ggpubr", "corrplot",
                   "performance", "ggfortify", "lmtest", "broom", "car"))
source("R/run_analysis.R")
# or render the full report:
rmarkdown::render("Projet Rapport.Rmd")
```

All results are reproducible with `set.seed(42)`.
