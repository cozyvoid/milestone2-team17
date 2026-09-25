# SIADS 696 Milestone II — Team 17

## Healthcare Access Characteristics: Predicting Routine Checkup Rates and Identifying County Access–Vulnerability Profiles

**Team Members:** Sara Jahanian, Samantha Salazar, Gina Sheesley  
**Course:** SIADS 696 — Milestone II  
**Program:** University of Michigan, Master of Applied Data Science

---

## Project Overview

This project examines how healthcare-resource availability and community vulnerability relate to preventive healthcare utilization across U.S. counties.

The project has two primary components:

1. **Supervised learning:** predict county-level routine checkup prevalence using healthcare-resource and access-related characteristics.
2. **Unsupervised learning:** identify distinct county healthcare-access and social-vulnerability profiles using clustering methods.

A broader goal is to understand whether counties with similar preventive-care utilization may face different underlying barriers, which could have implications for healthcare planning and resource allocation.

---

## Research Questions

### Part A — Supervised Learning

**Primary question:**  
How well can county-level healthcare-resource availability and access characteristics predict the age-adjusted prevalence of adults receiving a routine checkup within the past year?

Planned predictor domains include:

- healthcare provider supply
- healthcare infrastructure
- Health Professional Shortage Area (HPSA) measures
- rurality and geographic-access context
- selected community barriers, subject to leakage and temporal-alignment review

### Part B — Unsupervised Learning

**Primary question:**  
What distinct healthcare-access and social-vulnerability profiles emerge among U.S. counties?

**Secondary question:**  
Do the independently identified county profiles differ in their distributions of routine-checkup utilization?

The PLACES routine-checkup outcome will **not** be used to construct clusters. It may be examined afterward as an external descriptive outcome.

---

## Proposed Data Sources

### CDC PLACES

**Planned use:** supervised-learning outcome

The proposed target is the county-level **age-adjusted prevalence of adults receiving a routine checkup within the past year**.

Planned release:
- 2024 CDC PLACES county release
- routine-checkup estimates based on 2022 BRFSS data
- 2020 Census geography

Source:  
https://www.cdc.gov/places/

### HRSA Area Health Resources Files (AHRF)

**Planned use:** primary supervised and unsupervised predictors

Candidate measures include:

- primary-care provider supply
- healthcare workforce measures
- hospitals and healthcare facilities
- healthcare infrastructure
- rurality and related county characteristics

Source:  
https://data.hrsa.gov/topics/health-workforce/nchwa/ahrf

### HRSA Health Professional Shortage Area (HPSA) Data

**Planned use:** healthcare-shortage predictors

Candidate measures include:

- primary-care HPSA indicators
- shortage scores
- estimated provider shortages
- county-level aggregates of geographic/population/facility designations

Source:  
https://data.hrsa.gov/topics/health-workforce/shortage-areas/dashboard

### CDC/ATSDR Social Vulnerability Index (SVI)

**Planned use:** primarily unsupervised learning

Planned release:
- 2022 national county-level SVI

Candidate dimensions include:

- socioeconomic status
- household characteristics
- racial and ethnic minority status
- housing type and transportation

Source:  
https://www.atsdr.cdc.gov/place-health/php/svi/

---

## Planned Methods

### Supervised Learning

We currently plan to compare three diverse model families:

- **Elastic Net Regression** — interpretable regularized linear baseline
- **Support Vector Regression (SVR)** — nonlinear kernel-based model
- **Gradient Boosting** — tree-based ensemble model

Planned evaluation metrics:

- Mean Absolute Error (MAE)
- Root Mean Squared Error (RMSE)
- R²
- 5-fold cross-validation with mean and standard deviation

Additional evaluation may include:

- geographic/grouped cross-validation as a robustness check
- permutation feature importance
- feature-group ablation
- sensitivity analysis
- grouped residual/error analysis
- failure analysis on specific counties

### Unsupervised Learning

Primary planned approaches:

- **K-means clustering**
- **Gaussian Mixture Models (GMM)**

Possible supporting techniques:

- feature standardization
- PCA for dimensionality reduction and visualization
- cluster stability analysis
- silhouette score
- Davies-Bouldin index
- BIC/AIC for GMM
- county-level choropleth maps
- cluster-profile heatmaps

Hierarchical clustering or DBSCAN may be considered as supplementary robustness/outlier analyses if time permits.

---

## Feature Selection Strategy

AHRF contains thousands of variables, so features will not be selected arbitrarily.

Candidate variables will first be organized into conceptually defined groups:

- provider supply
- healthcare infrastructure
- provider shortage
- geographic/access context
- selected community barriers

Variables will then be screened based on:

1. temporal relevance
2. county coverage
3. missingness
4. interpretability
5. redundancy/correlation
6. overlap with variables used in PLACES target construction

The final supervised feature set is expected to contain approximately 15–30 variables, subject to exploratory data analysis.

---

## Data Integration and Quality Checks

County FIPS codes will be used as the primary geographic join key.

Before modeling, the team will:

- standardize county FIPS formats
- document Census geography versions used by each source
- compare source years and reference periods
- report row counts before and after each join
- identify unmatched or changed FIPS codes
- inspect missing-data patterns
- check whether missing counties are geographically or systematically concentrated
- aggregate HPSA data to county-level measures where necessary
- avoid silently dropping unmatched counties

---

## Important Methodological Considerations

### PLACES Shared-Input Circularity

CDC PLACES estimates are model-based rather than direct measurements for every county. Because demographic and socioeconomic information is used in the PLACES estimation process, the supervised analysis will avoid using overlapping demographic predictors when they could create shared-input circularity.

The primary supervised feature set will therefore emphasize AHRF and HPSA healthcare-resource variables.

### Interpretation

This project is **predictive and descriptive, not causal**.

Healthcare resources do not locate randomly across counties, and county-level associations should not be interpreted as evidence that changing a single resource will necessarily cause a change in routine-checkup utilization.

Results will also be interpreted at the county level rather than as individual-level relationships.

---

## Repository Structure

```text
milestone2-team17/
├── README.md
├── data/
│   ├── raw/
│   ├── interim/
│   └── processed/
├── notebooks/
│   ├── 01_data_audit.ipynb
│   ├── 02_cleaning_and_merging.ipynb
│   ├── 03_eda.ipynb
│   ├── 04_supervised_modeling.ipynb
│   └── 05_unsupervised_modeling.ipynb
├── src/
│   ├── data_utils.py
│   ├── features.py
│   └── modeling.py
├── figures/
├── results/
└── report/
```

### Folder Descriptions

- `data/raw/` — original source files; should remain unchanged
- `data/interim/` — partially cleaned or merged data
- `data/processed/` — final analysis/modeling-ready datasets
- `notebooks/` — numbered notebooks following the analysis workflow
- `src/` — reusable Python functions
- `figures/` — generated figures and maps
- `results/` — model metrics, tuning results, and other outputs
- `report/` — proposal, report drafts, and final report files

---

## Planned Workflow

1. Set up repository and documentation
2. Download and archive raw datasets
3. Audit schemas, years, FIPS codes, and missingness
4. Harmonize and merge county-level data
5. Conduct exploratory data analysis
6. Finalize supervised feature set
7. Train and compare supervised models
8. Perform feature importance, ablation, sensitivity, and failure analysis
9. Prepare clustering feature representation
10. Fit and evaluate unsupervised models
11. Compare routine-checkup utilization across discovered county profiles
12. Produce final visualizations and report
13. Verify reproducibility and GitHub submission requirements

---

## Team Responsibilities

Current division of responsibilities is provisional and may change as the project develops.

- **Sam:** HPSA/SVI integration, policy and health-equity framing, EDA, unsupervised analysis, interpretation
- **Sara:** AHRF feature engineering, repository setup, supervised-model support, visualizations
- **Gina:** data cleaning/integration, supervised analysis, PLACES preparation, unsupervised support
- **All team members:** methodology decisions, validation, report writing, reproducibility checks, and final review

---

## Project Status

**Current stage:** Proposal submitted; project setup and data feasibility review beginning.

Next planned milestone:

> Build `01_data_audit.ipynb` to verify that PLACES, AHRF, HPSA, and SVI can be harmonized at the county level and to quantify coverage, missingness, and join success.

---

## Reproducibility Notes

- Raw source files should not be edited directly.
- Any transformation from raw to processed data should be reproducible through code.
- Dataset versions, reference years, and download dates should be documented.
- Final modeling datasets should be generated from scripts/notebooks rather than manually edited.
- Random seeds should be specified for reproducible modeling and clustering.
- Any reused preprocessing code from prior coursework will be explicitly identified.

---

## Related Work

Related-work references will be expanded in the final report. Current examples include:

- Bauer et al. (2022), *County-Level Social Vulnerability and Breast, Cervical, and Colorectal Cancer Screening Rates in the US, 2018*
- Al Rifai et al. (2022), *State-Level Social Vulnerability Index and Healthcare Access: The Behavioral Risk Factor Surveillance System Survey*
- Bowser et al. (2024), *American clusters: Using machine learning to understand health and health care disparities in the United States*

---

## Notes

Project methods, feature sets, and dataset choices may be refined following instructor feedback and initial feasibility testing.
