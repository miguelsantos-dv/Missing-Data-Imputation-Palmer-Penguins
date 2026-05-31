#  Missing Data Imputation — Palmer Penguins

A data preprocessing project focused on handling missing values in the **Palmer Penguins** dataset, combining **KNN Imputation** for numerical features and **conditional mode imputation** for categorical features, with before/after validation in Python and Power BI.

---

##  Overview

The dataset had **30% of its cells removed at random** (seeded for reproducibility). The goal was to recover those missing values using statistically justified imputation strategies — without distorting the original distributions, correlations, or class proportions.

---

##  Objectives

- Perform exploratory analysis to understand the structure and missing data patterns
- Choose and justify imputation methods for both numerical and categorical columns
- Validate that distributions, statistics, and correlations were preserved after imputation
- Export clean and raw CSVs for visual comparison in **Power BI dashboards**

---

##  Imputation Strategy

### Numerical Columns
`bill_length_mm` · `bill_depth_mm` · `flipper_length_mm` · `body_mass_g`

| Option Considered | Verdict |
|---|---|
| Drop rows with NaN | ❌ Would lose 100+ rows from a 344-row dataset |
| Global mean/median | ❌ Distributions are multimodal — global mean falls between species clusters |
| **KNN Imputer (k=5)** | ✅ Exploits inter-variable correlations; neighbors are likely from the same species |

The numerical columns are significantly correlated (especially `flipper_length_mm` and `body_mass_g`). KNN leverages these correlations to estimate missing values from the 5 nearest neighbors, which tend to belong to the same species — producing species-appropriate estimates.

Features were **normalized to [0, 1]** before KNN and then rescaled back to original units.

---

### Categorical Columns
`species` · `island` · `sex`

Imputation was done in dependency order:

1. **`species`** → global mode (`Adelie`, the most frequent class) — no prior group to condition on
2. **`island`** → mode per species — each species has a dominant island (e.g., Gentoo → Biscoe, Chinstrap → Dream); global mode would misplace penguins geographically
3. **`sex`** → mode per species — male/female proportions vary by species; consistent with the approach used for `island`

---

##  Power BI Dashboards

Two dashboards were built to validate the imputation visually:

| Dashboard | Description |
|---|---|
| **Before** | Raw dataset with missing values — shows missing counts per column and original distributions |
| **After** | Imputed dataset — all missing counts drop to 0; distributions remain consistent |

The dashboards confirm that histograms and bar charts retained their original shapes after imputation, with no artificial peaks or distortions introduced.

---

##  Validation

After imputation, the following checks were performed:

- **Descriptive statistics** (mean, std, min, max) — remained close to pre-imputation values
- **Correlation matrix** — inter-variable relationships were preserved
- **Categorical proportions** — species, island, and sex distributions stayed consistent with the original data

---

##  Tech Stack

- **Python 3**
- `pandas` / `numpy` — data manipulation
- `seaborn` / `matplotlib` — EDA and validation plots
- `scikit-learn` — `KNNImputer`, `MinMaxScaler`
- **Power BI** — before/after dashboard comparison

---

##  Project Structure

```
MD_Trabalho2_Imputacao.ipynb   # Main notebook: EDA, imputation, validation
penguins_inicial.csv           # Raw dataset with missing values (Power BI input)
penguins_imputado.csv          # Fully imputed dataset (Power BI input)
dashboards.pdf                 # Power BI dashboards (before & after)
README.md                      # Project documentation
```

---

##  How to Run

1. Clone this repository
2. Install dependencies:
   ```bash
   pip install pandas numpy seaborn matplotlib scikit-learn
   ```
3. Open the notebook:
   ```bash
   jupyter notebook MD_Trabalho2_Imputacao.ipynb
   ```
4. Run all cells in order — the seeded random removal is deterministic, so results are fully reproducible

---

## 📚 Context

This project was developed as an academic assignment for a **Data Mining / Machine Learning** course. It demonstrates a real-world data preprocessing workflow: diagnosing missing data, selecting appropriate imputation methods based on data structure, and validating results before feeding the clean dataset into downstream models.
