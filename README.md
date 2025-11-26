# Meta-Analysis of L2 Pronunciation Training Effectiveness

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![R](https://img.shields.io/badge/R-4.0+-276DC3.svg)](https://www.r-project.org/)

>  | Quantifies the overall effectiveness of L2 English pronunciation training and isolates learner and instructional predictors through moderator analyses, robustness checks, and publication bias assessment.



---
## Workflow Overview

```text
moderator_raw.csv + Effect_size_raw_results.csv
            ↓
      Meta_ready_cleaned.ipynb
            ↓
    Meta_ready_cleaned.csv
            ↓
  Statistical_Analysis.ipynb (R)
            ↓
      Multiple output files
```




---

## 🔬 Analysis Workflow

### **Stage 1: Data Preparation** 
**Script**: `Meta_ready_cleaned.ipynb` (Python)

**Input Files**:
- `moderator_raw.csv` → Study-level moderator variables (*N* studies × *K* moderators)
- `Effect_size_raw_results.csv` → Effect size estimates (Hedges' *g*, SE, 95% CI)

**Processing Steps**:
1. **Data Import & Standardization** → Unified encoding, whitespace trimming
2. **Index Harmonization & Sample Inclusion** → Matching `Study_ID` and `Effect_ID`, filtering missing values
3. **Irregular Value Normalization** → Handling anomalies, missing data imputation
4. **Effect Size Structuring** → Extracting Hedges' *g*, SE, confidence intervals

**Output File**:
- `Meta_ready_cleaned.csv` → **Analysis-ready dataset** (merged & cleaned data)



---

### **Stage 2: Statistical Analysis**
**Script**: `Statistical_Analysis.ipynb` (R)

**Input File**:
- `Meta_ready_cleaned.csv` (generated from Stage 1)

**Analysis Modules**:

#### **Module 1: Overall Random-Effects Model**
- REML estimation for pooled effect size
- Heterogeneity quantification (*Q* statistic, *I*², *τ*²)
- Prediction intervals for generalization

#### **Module 2: Moderator Analyses**
- **Univariate Meta-Regression**: Single-predictor models
- **Multivariate Meta-Regression**: Joint models controlling for confounds

#### **Module 3: Robustness & Sensitivity Checks**
- **Leave-One-Out Analysis**: Stability testing
- **Influence Diagnostics**: Cook's *D*, DFBETAS, leverage values
- **Robust Variance Estimation (RVE)**: Adjustment for effect dependency

#### **Module 4: Publication Bias Assessment**
- Egger's regression test (funnel plot asymmetry)
- Trim-and-fill imputation for adjusted effect sizes
- Fail-safe *N* calculation

**Output Files** (12 total):
| Filename | Description |
|----------|-------------|
| `overall_meta_analysis_results.csv` | Pooled effect size & heterogeneity indices |
| `univariate_moderator_results.csv` | Single-predictor moderator test results |
| `significant_moderators.csv` | Summary of significant moderators (*p* < .05) |
| `multivariate_model_coefficients.csv` | Regression coefficients from multivariate model |
| `multivariate_model_fit.csv` | Model fit indices (*R*², AIC, BIC) |
| `leave_one_out_analysis.csv` | Sensitivity metrics from LOO analysis |
| `influence_diagnostics.csv` | Influence statistics (Cook's *D*, DFBETAS) |
| `rve_overall_effect.csv` | RVE-adjusted pooled effect size |
| `rve_moderator_results.csv` | RVE-adjusted moderator tests |
| `publication_bias_tests.csv` | Publication bias test statistics |
| `publication_bias_trim_and_fill.csv` | Imputed studies & adjusted effect sizes |
| `funnel_plot.png` | Visual assessment of funnel plot asymmetry |







---

## ⚙️ Environment Setup

### **Python Dependencies** (Stage 1)
```bash
pip install numpy>=1.20.0 pandas>=1.3.0
```

### **R Dependencies** (Stage 2)
```r
install.packages(c("metafor", "robumeta"))
# metafor  ≥ 3.0.0  → Meta-analytic models (Viechtbauer, 2010)
# robumeta ≥ 2.0    → Robust variance estimation (Hedges et al., 2010)
```




---

## 📊 Data Structure

### **Input Data Format**

#### `moderator_raw.csv`
| Column | Type | Description |
|--------|------|-------------|
| `index` | String | Unique effect size identifier |
| `Study_ID` | String | Study-level clustering variable |
| `[Moderator_1...K]` | Mixed | Categorical/continuous moderator variables |

#### `Effect_size_raw_results.csv`
| Column | Type | Description |
|--------|------|-------------|
| `index` | String | Unique effect size identifier (must match moderator file) |
| `Effect_ID` | String | Effect-level identifier |
| `Hedges_g` | Numeric | Bias-corrected standardized mean difference |
| `SE` | Numeric | Standard error (or variance) |
| `CI_lower` | Numeric | 95% CI lower bound |
| `CI_upper` | Numeric | 95% CI upper bound |


