# Foreign Accent Reduction in L2 English: A Meta-Analysis of Learner and Training Factors

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![R](https://img.shields.io/badge/R-4.0+-276DC3.svg)](https://www.r-project.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)


---

## 📖 Research Overview

This meta-analysis systematically synthesizes empirical studies on **Foreign Accent Reduction (FAR)** in second language (L2) English to quantify:

1. **Overall effectiveness** of pronunciation training interventions  
2. **Learner-related moderators** (age, L1, proficiency, education stage, English major status, learning context)  
3. **Training-related moderators** (context, focus type, target features, feedback type, duration, instructor type, peer interaction, visual cues)  

---

## 🔄 Complete Workflow

```text
Raw Data Files
├── Moderator_raw.csv (25 variables)
└── Effect_size_raw.csv (32 variables)
            ↓
   Meta_ready_cleaned.ipynb (Python)
            ↓
   Meta_ready_cleaned.csv (30 variables)
            ↓
   Statistical_Analysis.ipynb (R)
            ↓
   Meta_Analysis_Results/ (14 files)
   ├── Step1_Overall_Model/
   ├── Step2_Moderator_Analysis/
   ├── Step3_Robustness_Checks/
   └── Step4_Publication_Bias/
            ↓
   Data_Visualization.ipynb (Python)
            ↓
   Data_Visualization/ (8 files)
   ├── Step1_Effect_Distribution/
   ├── Step2_Moderator_Forest/
   ├── Step3_Multivariate_Panels/
   └── Step4_Diagnostics/
```

---

## 📁 Repository Structure

```
github/
├── README.md                                    # This file
├── requirements.txt                             # Python dependencies
├── requirements_r.txt                           # R package dependencies
│
├── ─────────────────────────────────────────── RAW DATA ───────────────────────────────────────────
├── Moderator_raw.csv                            # Study-level moderator variables (25 columns)
├── Effect_size_raw.csv                          # Raw effect size data (32 columns)
│
├── ─────────────────────────────────────────── PIPELINE ───────────────────────────────────────────
├── Meta_ready_cleaned.ipynb                     # Stage 1: Data preparation (Python)
├── Meta_ready_cleaned.csv                       # Analysis-ready dataset (30 columns)
├── Statistical_Analysis.ipynb                   # Stage 2: Meta-analysis (R)
├── Data_Visualization.ipynb                     # Stage 3: Publication figures (Python)
│
├── ──────────────────────────────────────── OUTPUTS: STATS ────────────────────────────────────────
├── Meta_Analysis_Results/
│   ├── Step1_Overall_Model/
│   │   └── overall_meta_analysis_results.csv    # Pooled effect & heterogeneity
│   ├── Step2_Moderator_Analysis/
│   │   ├── univariate_moderator_summary.csv     # Omnibus tests per moderator
│   │   ├── univariate_moderator_results.csv     # Level-by-level estimates
│   │   ├── multivariate_model_coefficients.csv  # Adjusted moderator effects
│   │   └── multivariate_model_fit.csv           # Model R² & fit indices
│   ├── Step3_Robustness_Checks/
│   │   ├── leave_one_out_analysis.csv           # Sensitivity analysis
│   │   ├── influence_diagnostics.csv            # Cook's D, DFBETAS, leverage
│   │   ├── rve_overall_effect.csv               # RVE-adjusted pooled effect
│   │   └── rve_moderator_results.csv            # RVE-adjusted moderator tests
│   └── Step4_Publication_Bias/
│       ├── publication_bias_tests.csv           # Egger, trim-and-fill, fsN
│       └── publication_bias_trim_and_fill.csv   # Imputed studies (if any)
│
├── ────────────────────────────────────── OUTPUTS: FIGURES ────────────────────────────────────────
└── Data_Visualization/
    ├── Step1_Effect_Distribution/
    │   ├── Figure1_Effect_Size_Distribution.png # Distribution + forest plot
    │   └── Figure1_Effect_Size_Distribution.pdf
    ├── Step2_Moderator_Forest/
    │   ├── Figure2_Univariate_Moderators.png    # Univariate moderator forest
    │   └── Figure2_Univariate_Moderators.pdf
    ├── Step3_Multivariate_Panels/
    │   ├── Figure3_Multivariate_Meta_Regression.png  # 4-panel dashboard
    │   └── Figure3_Multivariate_Meta_Regression.pdf
    └── Step4_Diagnostics/
        ├── Figure4_Publication_Bias_Robustness.png   # Funnel + LOO plots
        └── Figure4_Publication_Bias_Robustness.pdf
```




---

## 🔬 Analysis Pipeline

### **Stage 1: Data Preparation** 
**Script**: `Meta_ready_cleaned.ipynb` (Python)

**Input Files**:
- `Moderator_raw.csv` → Study-level moderator variables (25 columns)
- `Effect_size_raw.csv` → Raw effect size estimates (32 columns)
 
**Processing Steps**:
1. **Data Import & Standardization** → UTF-8 encoding, whitespace trimming, categorical recoding
2. **Index Harmonization** → Match `Study_ID` and `Effect_ID` across files
3. **Quality Control** → Filter missing values, validate effect size metrics
4. **Effect Size Extraction** → Hedges' *g*, SE, CI for random-effects models

**Output File**:
- `Meta_ready_cleaned.csv` → **Analysis-ready dataset** (30 columns)

**Execution**:
```bash
jupyter notebook Meta_ready_cleaned.ipynb
```

---

### **Stage 2: Statistical Analysis**
**Script**: `Statistical_Analysis.ipynb` (R)

**Input**: `Meta_ready_cleaned.csv`

**Analysis Modules**:

#### **STEP 0: Environment Setup**
- Load libraries: `metafor` (≥3.0.0), `robumeta` (≥2.0)
- Validate data structure and missing values

#### **STEP 1: Overall Random-Effects Model**
- **REML estimation** for pooled effect size (Hedges' *g*)
- **Heterogeneity**: *Q* statistic, *I*², *τ*²
- **Prediction intervals** (95% PI)
- **Output**: `overall_meta_analysis_results.csv`

#### **STEP 2: Moderator Analyses**
**Univariate Meta-Regression** (14 substantive moderators):
- *Learner factors*: Age group, L1, Proficiency, Education stage, English major, Learning context
- *Training factors*: Training context, Focus type, Target feature, Feedback type, Duration, Instructor type, Peer interaction, Visual cues

**Multivariate Meta-Regression**:
- Joint models controlling for confounds
- Interaction effects

**Outputs**: 
- `univariate_moderator_summary.csv` (omnibus tests)
- `univariate_moderator_results.csv` (level estimates)
- `multivariate_model_coefficients.csv`
- `multivariate_model_fit.csv`

#### **STEP 3: Robustness Checks**
- **Leave-One-Out (LOO)**: Effect size stability
- **Influence Diagnostics**: Cook's *D*, DFBETAS, leverage
- **Robust Variance Estimation (RVE)**: Clustered/nested designs

**Outputs**: 
- `leave_one_out_analysis.csv`
- `influence_diagnostics.csv`
- `rve_overall_effect.csv`
- `rve_moderator_results.csv`

#### **STEP 4: Publication Bias**
- **Egger's test**: Funnel plot asymmetry
- **Trim-and-fill**: Impute missing studies
- **Fail-safe N**: Robustness to null studies

**Outputs**: 
- `publication_bias_tests.csv`
- `publication_bias_trim_and_fill.csv` (conditional)

**Execution**:
```bash
jupyter notebook Statistical_Analysis.ipynb
```

---

### **Stage 3: Data Visualization**
**Script**: `Data_Visualization.ipynb` (Python)

**Input**: 
- `Meta_ready_cleaned.csv`
- All files from `Meta_Analysis_Results/`

**Figure Generation**:

#### **Figure 1: Effect Size Distribution & Forest Plot**
- **Panel A**: Kernel density + histogram with Cohen's benchmarks
- **Panel B**: Forest plot with precision-weighted markers, pooled diamond
- **Output**: `Step1_Effect_Distribution/Figure1_*.{png,pdf}` (600 DPI)

#### **Figure 2: Univariate Moderator Forest Plot**
- 14 moderators with confidence intervals
- Stratified by Learner vs. Training characteristics
- Multi-layer rendering (14-layer point estimates, gradient CIs)
- **Output**: `Step2_Moderator_Forest/Figure2_*.{png,pdf}` (600 DPI)

#### **Figure 3: Multivariate Meta-Regression Dashboard**
- **4-panel layout**: Education, English Major, L1, Treatment Duration
- Violin plots + forest plots with color-coded effect magnitudes
- **Output**: `Step3_Multivariate_Panels/Figure3_*.{png,pdf}` (600 DPI)

#### **Figure 4: Publication Bias & Robustness Diagnostics**
- **Panel A**: Funnel plot with 95% CI regions
- **Panel B**: Leave-one-out sensitivity analysis
- **Output**: `Step4_Diagnostics/Figure4_*.{png,pdf}` (600 DPI)

**Execution**:
```bash
jupyter notebook Data_Visualization.ipynb
```

---

## ⚙️ Environment Setup

### **Quick Start: Install All Dependencies**

**Python** (Stages 1 & 3):
```bash
pip install -r requirements.txt
```

**R** (Stage 2):
```r
# Run in R console
install.packages(c("metafor", "robumeta", "IRkernel"))
IRkernel::installspec(user = TRUE)
```

### **Python Dependencies** (Stages 1 & 3)
See `requirements.txt` for complete list.

**Core Libraries**:
- `numpy` ≥ 1.21.0 — Numerical operations
- `pandas` ≥ 1.3.0 — Data manipulation
- `matplotlib` ≥ 3.4.0 — Publication-grade visualization
- `scipy` ≥ 1.7.0 — Statistical functions (KDE)
- `jupyter` ≥ 1.0.0 — Notebook environment

**Manual Installation** (alternative):
```bash
pip install numpy>=1.21.0 pandas>=1.3.0 matplotlib>=3.4.0 scipy>=1.7.0 jupyter
```

### **R Dependencies** (Stage 2)
See `requirements_r.txt` for package details and references.

**Required Packages**:
- `metafor` ≥ 3.0.0 — Meta-analytic models (Viechtbauer, 2010)
- `robumeta` ≥ 2.0 — Robust variance estimation (Hedges et al., 2010)
- `IRkernel` — R kernel for Jupyter


---

## 📊 Data Structure

### **Input Data**

#### `Moderator_raw.csv` (25 columns)

| Category | Variables | Description |
|----------|-----------|-------------|
| **Identifiers** | `index`, `Study_ID`, `Effect_ID` | Unique identifiers for matching |
| **Study Design** | `Design_Type`, `N_Exp`, `Comparator_Type` | Experimental design features |
| **Learner Characteristics** | `Age_Group`, `Gender_Ratio_FM`, `L1`, `Proficiency_Level`, `Education_Stage`, `English_Major`, `Learning_Context` | Individual difference factors |
| **Training Characteristics** | `Training_Context`, `Focus_Type`, `Target_Feature`, `Feedback_Type`, `Training_TotalMinute`, `Training_TotalWeeks`, `Treatment_Duration`, `Instructor_Type`, `Peer_Interaction`, `Visual_Cue` | Intervention features |
| **Outcome Characteristics** | `Outcome_Domain`, `Rater_Type` | Measurement properties |

#### `Effect_size_raw.csv` (32 columns)

| Category | Variables | Description |
|----------|-----------|-------------|
| **Identifiers** | `index`, `Study_ID`, `Effect_ID`, `Author_Year`, `Year`, `Country`, `Publication_Type` | Study metadata |
| **Sample Info** | `N_Exp`, `N_Ctrl`, `Comparison_Type`, `Timepoint` | Sample characteristics |
| **Pretest Homogeneity** | `Pretest_Homogeneity_Confirmed`, `Pretest_Homogeneity_p_value`, `Pretest_Homogeneity_Interpretation` | Baseline equivalence |
| **Descriptive Stats** | `M_Exp_Pre`, `SD_Exp_Pre`, `M_Exp_Post`, `SD_Exp_Post`, `M_Ctrl_Pre`, `SD_Ctrl_Pre`, `M_Ctrl_Post`, `SD_Ctrl_Post` | Group means & SDs |
| **Test Statistics** | `t_value`, `df`, `p_value` | Inferential statistics |
| **Effect Sizes** | `Effect_Size`, `Hedges_g`, `SE`, `Variance`, `CI_Lower`, `CI_Upper`, `Weight` | Standardized effect metrics |

### **Output Data**

#### `Meta_ready_cleaned.csv` (30 columns)
**Merged dataset** combining 25 moderator variables + 5 effect size metrics (`Hedges_g`, `SE`, `Variance`, `CI_Lower`, `CI_Upper`)

#### Meta-Analysis Results (14 CSV files)
See **Repository Structure** section above for detailed output file descriptions.

---

## 📈 Key Results Summary

### **Overall Effect Size**
- **Pooled Hedges' *g***: *g* = 0.527 (95% CI [0.378, 0.676])
- **Heterogeneity**: *I*² = 54.6%, *τ*² = 0.088
- **Interpretation**: Medium-to-large positive effect with moderate heterogeneity

### **Significant Moderators** (*p* < .05)
**Learner Factors**:
- Education Stage
- English Major Status
- L1 Background

**Training Factors**:
- Treatment Duration
- Training Focus Type
- Feedback Type

*(Detailed results in `Meta_Analysis_Results/Step2_Moderator_Analysis/`)*

### **Publication Bias**
- **Egger's test**: *p* = .035 (funnel asymmetry detected)
- **Trim-and-fill**: 3 imputed studies, adjusted *g* = 0.489
- **Fail-safe N**: *N* = 287 (robust effect)


---

**Last Updated**: November 30, 2025



