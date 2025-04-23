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

## 🧮 Risk Score Construction

To quantify patient lifestyle exposure, categorical smoking and alcohol indicators were converted into numeric scores. This enabled more effective statistical testing and visualization.

Before applying mappings, patients labeled as `"Current Reformed Smoker, Duration Not Specified"` were excluded due to ambiguity.

### 🔁 Tobacco Risk Scoring

Three variables were used:
- **Smoking Status** (e.g., Lifelong Non-Smoker → 0, Current Smoker → 3)
- **Cigarettes Per Day** (increasing scale from ≤12 → 0 up to >48 → 4)
- **Years Smoked** (scored from ≤14 → 0 up to >56 → 4)

These were summed to create a composite `tobacco_risk` score ranging from **0 (no risk)** to **10 (high risk)**.

### 🍷 Alcohol Risk Scoring

Two fields were used:
- **Alcohol History** (`Yes` = 1, `No` = 0)
- **Alcohol Intensity** (Lifelong Non-Drinker → 0, Occasional Drinker → 2)

These were summed to form the `alcohol_risk` score (range: 0–3).

The resulting numeric scores allowed for consistent group-based analyses of their relationship with gene expression outcomes.


