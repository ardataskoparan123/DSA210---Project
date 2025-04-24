# Data Cleaning and Preprocessing

## Data Source

The dataset was manually collected from the [National Cancer Institute GDC Data Portal](https://portal.gdc.cancer.gov/). It contains:

- Clinical metadata describing smoking and alcohol habits of patients
- Gene expression levels for three key oncogenes: **KRAS**, **EGFR**, and **ALK**

The data was visually filtered and sorted using the portal interface, then downloaded in `.tsv` format for offline processing.

---

## Cleaning Strategy

Proper data cleaning was crucial to ensure reliable analysis. The following steps were taken to prepare the dataset:

### Replacement of Missing Indicators

The raw dataset used various textual placeholders to denote missing or unknown values such as `"Not Reported"`, `"Unknown"`, and `"Smoking history not documented"`. These values were systematically converted to actual `NaN` entries for consistent handling with pandas.


### Dropped Columns

Certain columns were removed from the dataset to eliminate noise and redundancy. The decision was based on three main factors:

- **High proportion of missing values**
- **Lack of variability (low information gain)**
- **Overlap with other, more informative variables**

| **Column**                      | **Reason for Removal**                                  |
|---------------------------------|----------------------------------------------------------|
| `Alcohol days per week`         | 100% of values missing                                   |
| `Pack years smoked`             | 99% of data had value `≤146` → uninformative distribution |
| `Tobacco smoking onset year`    | Redundant with `Years smoked`                            |
| `Tobacco smoking quit year`     | Sparse and lacked resolution                             |

These decisions helped ensure that only meaningful features remained for subsequent scoring and hypothesis testing.

---

## Feature Selection and Normalization

The dataset was reduced to retain only variables that directly contributed to either:

- **Quantitative gene expression values**: `KRAS`, `EGFR`, `ALK`
- **Behavioral risk indicators**: tobacco and alcohol use

All gene expression features were already **standardized**, meaning:

- Mean ≈ 0
- Standard Deviation ≈ 1

Thus, no additional normalization or scaling was applied before statistical testing.

---

## Risk Score Construction

To quantify patient lifestyle exposure, categorical smoking and alcohol indicators were converted into numeric scores. This enabled more effective statistical testing and visualization.

Before applying mappings, patients labeled as `"Current Reformed Smoker, Duration Not Specified"` were excluded due to ambiguity.

### Tobacco Risk Scoring

Three variables were used:
- **Smoking Status** (e.g., Lifelong Non-Smoker → 0, Current Smoker → 3)
- **Cigarettes Per Day** (increasing scale from ≤12 → 0 up to >48 → 4)
- **Years Smoked** (scored from ≤14 → 0 up to >56 → 4)

These were summed to create a composite `tobacco_risk` score ranging from **0 (no risk)** to **10 (high risk)**.

### Alcohol Risk Scoring

Two fields were used:
- **Alcohol History** (`Yes` = 1, `No` = 0)
- **Alcohol Intensity** (Lifelong Non-Drinker → 0, Occasional Drinker → 2)

These were summed to form the `alcohol_risk` score (range: 0–3).

The resulting numeric scores allowed for consistent group-based analyses of their relationship with gene expression outcomes and they were collected in the file "genes_risk_summary.tsv". Explatory data analysis and hypothesis testing were done using this file's dataframe.


# Explatory Data Analysis
---
## EGFR Expression by Tobacco Risk Score

This boxplot illustrates the distribution of **EGFR gene expression** across patients grouped by their computed **tobacco_risk** score.

![image](https://github.com/user-attachments/assets/28a7697d-9b9d-4ad9-931f-95a653e94df0)

### Observations

- Expression values are **centered around zero** across all groups, consistent with standardized data.
- The **spread (IQR)** appears slightly wider at lower risk scores (0–3), suggesting more variability in expression among non-smokers or low-risk individuals.
- Several **outliers** are visible in each group, especially at lower risk levels. These likely represent biologically extreme expression patterns.
- As tobacco risk increases beyond 6, expression levels appear to **narrow and flatten**, with fewer high-expression outliers.

### Interpretation

There is a **slight visual trend** suggesting a decrease in EGFR expression with increasing tobacco risk, though this difference is subtle.

## KRAS Expression by Tobacco Risk Score

This boxplot visualizes **KRAS gene expression** values distributed across patients with varying levels of **tobacco_risk**.

![image](https://github.com/user-attachments/assets/aedf8305-03dd-44e1-805c-188712ce5343)

### Observations

- Compared to EGFR, **KRAS expression is more widely distributed** across the risk spectrum.
- Low-risk groups (e.g., 0–3) show **greater variance** in expression, with multiple high-value outliers.
- As tobacco risk increases, the **median and interquartile range tend to decrease**, indicating potentially lower expression in higher-risk groups.
- Outliers persist throughout, including extreme values at low (0–4) and high (9) risk scores.

### Interpretation

The trend in this plot hints at a **negative correlation** between tobacco exposure and KRAS expression — i.e., increased tobacco use may be associated with suppressed KRAS activity. The **more noticeable spread** and variability also suggest that KRAS may be more sensitive to tobacco-related biological processes than EGFR.



