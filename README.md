# Foreign Accent Reduction in L2 English: A Meta-Analysis of Learner and Training Factors

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![R](https://img.shields.io/badge/R-4.0+-276DC3.svg)](https://www.r-project.org/)


---

## 📖 Research Overview

This meta-analysis synthesizes empirical studies on **Foreign Accent Reduction (FAR)** in second language (L2) English to systematically assess:

1. **Overall effectiveness** of pronunciation training interventions  
2. **Learner-related factors** (age, L1, proficiency, education stage, learning context)  
3. **Training-related factors** (focus type, feedback type, visual cues, peer interaction, instructor type, duration)  

**Key Outcomes Examined:**
- **Outcome Domain**: Accentedness, Comprehensibility, and Pronunciation Accuracy measures

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
- `moderator_raw.csv` → Study-level moderator variables (25 variables)
- `Effect_size_raw_results.csv` → Raw study data and effect size estimates (32 variables)
 
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
  - *Learner factors*: Age group (child vs. adult), L1 background, proficiency level, gender ratio, education stage, English major status  
  - *Training factors*: Focus type, feedback type, visual cues, peer interaction, instructor type, training duration (minutes/weeks), training context
  - *Study design factors*: Design type, comparator type, rater type, outcome domain
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

#### `moderator_raw.csv` (25 columns)
| Column | Type | Description |
|--------|------|-------------|
| **Identifiers** | | |
| `index` | String | Unique effect size identifier (must match `Effect_size_raw_results.csv`) |
| `Study_ID` | String | Study-level clustering variable for nested/dependent effects |
| `Effect_ID` | String | Effect-level identifier within each study |
| **Study Design Moderators** | | |
| `Design_Type` | Categorical | Experimental design type |
| `N_Exp` | Numeric | Sample size in experimental/treatment group |
| `Comparator_Type` | Categorical | Type of control/comparison condition |
| **Learner Moderators** | | |
| `Age_Group` | Categorical | Age category of participants |
| `Gender_Ratio_FM` | Numeric | Proportion of female participants (0-1 scale, calculated from F/M counts) |
| `L1` | Categorical | Learners' first language background |
| `Proficiency_Level` | Categorical | English proficiency level |
| `Education_Stage` | Categorical | Educational level of participants |
| `English_Major` | Categorical | Whether participants are English majors |
| `Learning_Context` | Categorical | ESL (English as Second Language) / EFL (English as Foreign Language) |
| **Training Moderators** | | |
| `Training_Context` | Categorical | Setting where training took place |
| `Focus_Type` | Categorical | Type of pronunciation focus (segmental/suprasegmental/integrated) |
| `Target_Feature` | Categorical | Specific pronunciation feature(s) targeted |
| `Feedback_Type` | Categorical | Type of feedback provided |
| `Training_TotalMinute` | Numeric | Total training time in minutes |
| `Training_TotalWeeks` | Numeric | Training duration in weeks |
| `Treatment_Duration` | Categorical | Categorized training duration |
| `Instructor_Type` | Categorical | Type of instructor (human/CAPT/hybrid) |
| `Peer_Interaction` | Categorical | Whether peer interaction was involved |
| `Visual_Cue` | Categorical | Whether visual cues were used |
| **Outcome Moderators** | | |
| `Outcome_Domain` | Categorical | Type of pronunciation outcome measured |
| `Rater_Type` | Categorical | Type of raters for outcome assessment |

#### `Effect_size_raw_results.csv` (32 columns)
| Column | Type | Description |
|--------|------|-------------|
| **Identifiers & Study Metadata** | | |
| `index` | String | Unique effect size identifier (must match moderator file) |
| `Study_ID` | String | Study-level clustering variable |
| `Effect_ID` | String | Effect-level identifier |
| `Author_Year` | String | Study citation (Author & Year) |
| `Year` | Numeric | Publication year |
| `Country` | String | Country where study was conducted |
| `Publication_Type` | Categorical | Type of publication (Journal/Thesis/etc.) |
| `Timepoint` | Categorical | Measurement timepoint (Post/Follow-up) |
| **Sample Information** | | |
| `N_Exp` | Numeric | Sample size in experimental group |
| `N_Ctrl` | Numeric | Sample size in control group |
| `Comparison_Type` | Categorical | Type of comparison (Exp_vs_Ctrl, Pre_vs_Post, etc.) |
| `Pretest_Homogeneity_Confirmed` | Categorical | Whether pretest equivalence was confirmed |
| `Pretest_Homogeneity_p_value` | Numeric | P-value for pretest homogeneity test |
| `Pretest_Homogeneity_Interpretation` | String | Interpretation of homogeneity test result |
| **Raw Descriptive Statistics** | | |
| `M_Exp_Pre` | Numeric | Experimental group pretest mean |
| `SD_Exp_Pre` | Numeric | Experimental group pretest standard deviation |
| `M_Exp_Post` | Numeric | Experimental group posttest mean |
| `SD_Exp_Post` | Numeric | Experimental group posttest standard deviation |
| `M_Ctrl_Pre` | Numeric | Control group pretest mean |
| `SD_Ctrl_Pre` | Numeric | Control group pretest standard deviation |
| `M_Ctrl_Post` | Numeric | Control group posttest mean |
| `SD_Ctrl_Post` | Numeric | Control group posttest standard deviation |
| **Test Statistics** | | |
| `t_value` | Numeric | T-test statistic |
| `df` | Numeric | Degrees of freedom |
| `p_value` | Numeric | Statistical significance (p-value) |
| **Effect Size Metrics** | | |
| `Effect_Size` | Numeric | Raw effect size (Cohen's *d*) |
| `Hedges_g` | Numeric | Bias-corrected standardized mean difference (Hedges' *g*) |
| `SE` | Numeric | Standard error of effect size |
| `Variance` | Numeric | Variance of effect size |
| `CI_Lower` | Numeric | 95% confidence interval lower bound |
| `CI_Upper` | Numeric | 95% confidence interval upper bound |
| `Weight` | Numeric | Inverse-variance weight for meta-analysis |

### **Output Data Format**

#### `Meta_ready_cleaned.csv` (30 columns)
Final merged and cleaned dataset ready for meta-analysis, combining moderator variables with effect size metrics.

| Column Source | Columns |
|---------------|---------|
| **From moderator_raw.csv** | `index`, `Study_ID`, `Effect_ID`, `Design_Type`, `N_Exp`, `Age_Group`, `Gender_Ratio_FM`, `L1`, `Proficiency_Level`, `Education_Stage`, `English_Major`, `Learning_Context`, `Training_Context`, `Focus_Type`, `Target_Feature`, `Feedback_Type`, `Training_TotalMinute`, `Training_TotalWeeks`, `Treatment_Duration`, `Instructor_Type`, `Peer_Interaction`, `Visual_Cue`, `Comparator_Type`, `Outcome_Domain`, `Rater_Type` |
| **From Effect_size_raw_results.csv** | `Hedges_g`, `SE`, `Variance`, `CI_Lower`, `CI_Upper` |
| `CI_Lower` | Numeric | 95% CI lower bound |
| `CI_Upper` | Numeric | 95% CI upper bound |


