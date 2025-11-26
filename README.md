# Foreign Accent Reduction in L2 English: A Meta-Analysis of Learner and Training Factors

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![R](https://img.shields.io/badge/R-4.0+-276DC3.svg)](https://www.r-project.org/)


---

## 📖 Research Overview

This meta-analysis synthesizes empirical studies on **Foreign Accent Reduction (FAR)** in second language (L2) English to systematically assess:

1. **Overall effectiveness** of pronunciation training interventions  
2. **Learner-related factors** (age, L1–L2 distance, proficiency, motivation, gender)  
3. **Training-related factors** (integrated vs. single-modality training, explicit vs. implicit feedback, visual cues, peer interaction, CAPT, duration)  
4. **Interaction effects** between learner characteristics and instructional approaches  

**Key Outcomes Examined:**
- **Accentedness Reduction** – Perceived foreign accent/nativeness (rater judgments)
- **Comprehensibility/Intelligibility** – Ease and accuracy of listener understanding
- **Pronunciation Accuracy** – Objective segmental/suprasegmental production measures

---

## 🔄 Workflow Overview

```text
moderator_raw.csv + Effect_size_raw_results.csv
            ↓
      Meta_ready_cleaned.ipynb (Python)
            ↓
    Meta_ready_cleaned.csv
            ↓
  Statistical_Analysis.ipynb (R)
            ↓
      12 output files + funnel_plot.png
```




---

## 🔬 Analysis Workflow

### **Stage 1: Data Preparation** 
**Script**: `Meta_ready_cleaned.ipynb` (Python)

**Input Files**:
- `moderator_raw.csv` → Study-level moderator variables  
  - Learner factors: Age, L1, L1–L2 distance, proficiency, motivation, gender, residence duration
  - Training factors: Mode (perception/production/integrated), feedback type (explicit/implicit), visual cues, peer interaction, CAPT, duration, setting (lab/classroom)
- `Effect_size_raw_results.csv` → Effect size estimates (Hedges' *g*, SE, 95% CI)

**Processing Steps**:
1. **Data Import & Standardization** → Unified encoding (UTF-8), whitespace trimming, categorical recoding
2. **Index Harmonization** → Matching `Study_ID` and `Effect_ID` across moderator and effect size files
3. **Quality Control** → Filtering missing values, validating effect size metrics, handling nested effects
4. **Effect Size Structuring** → Extracting Hedges' *g*, standard error, confidence intervals for random-effects modeling

**Output File**:
- `Meta_ready_cleaned.csv` → **Analysis-ready dataset** (merged moderator + effect size data)

**Execution**:
```bash
jupyter notebook Meta_ready_cleaned.ipynb
```

---

### **Stage 2: Statistical Analysis**
**Script**: `Statistical_Analysis.ipynb` (R)

**Input File**:
- `Meta_ready_cleaned.csv` (generated from Stage 1)

**Analysis Modules**:

#### **Module 1: Overall Random-Effects Model**
- **REML estimation** for pooled effect size (Hedges' *g*)
- **Heterogeneity assessment**: *Q* statistic, *I*² (percentage of variation due to heterogeneity), *τ*² (between-study variance)
- **Prediction intervals** (95% PI) for expected effect range in future studies

#### **Module 2: Moderator Analyses**
- **Univariate meta-regression**: Testing single predictors  
  - *Learner factors*: Age (child vs. adult), L1–L2 distance, proficiency, motivation, gender  
  - *Training factors*: Mode, feedback type, visual cues, peer interaction, CAPT, duration, setting
- **Multivariate meta-regression**: Joint models controlling for confounds and testing interactions

#### **Module 3: Robustness & Sensitivity Checks**
- **Leave-One-Out (LOO) Analysis**: Assessing stability of pooled effect when each study is excluded
- **Influence Diagnostics**: Cook's *D*, DFBETAS, leverage values to detect outliers
- **Robust Variance Estimation (RVE)**: Adjusting for effect size dependency (clustered/nested designs)

#### **Module 4: Publication Bias Assessment**
- **Egger's regression test**: Testing funnel plot asymmetry
- **Trim-and-fill method**: Imputing potentially missing studies and recalculating adjusted effect size
- **Fail-safe N**: Number of null studies needed to nullify the observed effect

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

**Execution**:
```bash
jupyter notebook Statistical_Analysis.ipynb
```

---

## ⚙️ Environment Setup

### **Python Dependencies** (Stage 1)
```bash
pip install numpy>=1.20.0 pandas>=1.3.0 jupyter
```

### **R Dependencies** (Stage 2)
```r
install.packages(c("metafor", "robumeta", "IRkernel"))
# metafor  ≥ 3.0.0  → Meta-analytic models & moderator analyses (Viechtbauer, 2010)
# robumeta ≥ 2.0    → Robust variance estimation for clustered effects (Hedges et al., 2010)
# IRkernel          → R kernel for Jupyter Notebook
```

**Note**: After installing R packages, register the R kernel for Jupyter:
```r
IRkernel::installspec(user = TRUE)
```




---

## 📊 Data Structure

### **Input Data Format**

#### `moderator_raw.csv`
| Column | Type | Description |
|--------|------|-------------|
| `index` | String | Unique effect size identifier (must match `Effect_size_raw_results.csv`) |
| `Study_ID` | String | Study-level clustering variable for nested/dependent effects |
| **Learner Moderators** | | |
| `Age_Category` | Categorical | Child / Adult |
| `L1_L2_Distance` | Numeric | Linguistic distance index (Chiswick & Miller, 2004) |
| `Proficiency_Level` | Categorical | Beginner / Intermediate / Advanced |
| `Motivation_Type` | Categorical | Integrative / Instrumental |
| `Gender` | Categorical | Male / Female / Mixed |
| **Training Moderators** | | |
| `Training_Mode` | Categorical | Perception / Production / Integrated |
| `Feedback_Type` | Categorical | Explicit / Implicit / Mixed |
| `Visual_Cue` | Binary | Yes / No |
| `Peer_Interaction` | Binary | Yes / No |
| `CAPT` | Binary | Yes (Computer-Assisted) / No |
| `Duration_Hours` | Numeric | Total training time in hours |
| `Setting` | Categorical | Laboratory / Classroom |

#### `Effect_size_raw_results.csv`
| Column | Type | Description |
|--------|------|-------------|
| `index` | String | Unique effect size identifier (must match moderator file) |
| `Effect_ID` | String | Effect-level identifier |
| `Hedges_g` | Numeric | Bias-corrected standardized mean difference |
| `SE` | Numeric | Standard error (or variance) |
| `CI_lower` | Numeric | 95% CI lower bound |
| `CI_upper` | Numeric | 95% CI upper bound |


