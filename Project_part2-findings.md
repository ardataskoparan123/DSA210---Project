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
- The **spread (IQR)** appears slightly wider at lower risk scores (0–4), suggesting more variability in expression among non-smokers or low-risk individuals.
- Several **outliers** are visible in each group, especially at lower risk levels. These likely represent biologically extreme expression patterns.
- As tobacco risk increases beyond 6, expression levels appear to **narrow and flatten**, with fewer high-expression outliers.

### Interpretation

There is a **slight visual trend** suggesting a decrease in EGFR expression with increasing tobacco risk, though this difference is subtle.

## KRAS Expression by Tobacco Risk Score

This boxplot visualizes **KRAS gene expression** values distributed across patients with varying levels of **tobacco_risk**.

![image](https://github.com/user-attachments/assets/aedf8305-03dd-44e1-805c-188712ce5343)

### Observations

- Compared to EGFR, **KRAS expression is more widely distributed** across the risk spectrum.
- Low-risk groups (e.g. 0–3) show **greater variance** in expression, with multiple high-value outliers.
- As tobacco risk increases, the **median and interquartile range tend to decrease**, indicating potentially lower expression in higher-risk groups.
- Outliers persist throughout, including extreme values at low (0–4) and high (9) risk scores.

### Interpretation

The trend in this plot hints at a **negative correlation** between tobacco exposure and KRAS expression — i.e., increased tobacco use may be associated with suppressed KRAS activity. The **more noticeable spread** and variability also suggest that KRAS may be more sensitive to tobacco-related biological processes than EGFR.

## ALK Expression by Tobacco Risk Score

This boxplot illustrates **ALK gene expression** across patient groups with different **tobacco_risk** scores.

![image](https://github.com/user-attachments/assets/edf498c9-cefd-4529-8095-5882faa87d3e)

### Observations

- Expression levels at **tobacco_risk = 0** show the **widest range** and contain many **extreme outliers**.
- As tobacco risk increases, the **spread of expression tightens**, and median values stay close to zero.
- Most boxplots show **very short interquartile ranges**, indicating that most patients—regardless of tobacco exposure—have ALK expression near 0.
- A few outliers exist across all risk levels, but their **frequency and intensity** are highest in the no-risk group (0.0).

### Interpretation

There is a visible trend of **higher expression variability** in low-risk individuals, particularly among those with no tobacco exposure. This might suggest a suppressive relationship between smoking and ALK expression. However, the median doesn’t vary drastically across risk levels.

The dramatic presence of **outliers at 0.0** may point to a subgroup of biologically distinct patients whose ALK expression is abnormally elevated and unaffected by tobacco risk.

---

## EGFR Expression by Alcohol Risk Score

This boxplot shows the distribution of **EGFR gene expression** levels for patients grouped by their **alcohol_risk** score.

![image](https://github.com/user-attachments/assets/76fa9914-69cd-4d0a-869a-de8e98000def)

### Observations

- The **majority of patients** fall into the `alcohol_risk = 0` group, resulting in a dense cluster with visible outliers.
- Higher alcohol risk groups (1,2) have **significantly fewer samples**, as reflected in the narrow boxes and absence of large outlier ranges.
- The **median expression** remains fairly stable across all alcohol risk levels.
- A few extreme outliers are present in both `alcohol_risk = 0` and `3`, suggesting some biological heterogeneity regardless of drinking status.

### Interpretation

There is **no strong visual trend** suggesting a consistent increase or decrease in EGFR expression with rising alcohol risk. However, the large imbalance in group sizes—especially the dominance of `alcohol_risk = 0`—makes it difficult to draw reliable conclusions.

This chart highlights:
- The **sparsity** of alcohol data
- The **importance of cautious interpretation** when sample sizes differ drastically between groups

## KRAS Expression by Alcohol Risk Score

This boxplot shows the distribution of **KRAS gene expression** across patient groups based on their calculated **alcohol_risk**.

![image](https://github.com/user-attachments/assets/919d72fc-695b-4dd3-8560-ec3e47a53292)

### Observations

- The **alcohol_risk = 0** group again has the **highest count and widest spread**, with numerous outliers reaching above 15.
- The **alcohol_risk = 1** group shows **almost no vertical spread**, with expression values nearly identical across all samples — suggesting either extreme uniformity or very limited data.
- Groups **2** and **3** display more spread, especially `3`, which shows modest variability and a few outliers.

### Interpretation

There is **no consistent monotonic trend** in KRAS expression like in the case of EGFR as alcohol risk increases. However:
- The **lack of variance in alcohol_risk = 1** stands out and may indicate a sampling issue or a biologically narrow profile.
- Groups with higher risk (2 and 3) resemble group 0 in structure but on a **smaller scale**, due to fewer observations.

## ALK Expression by Alcohol Risk Score

This boxplot depicts the distribution of **ALK gene expression** across patient groups defined by their **alcohol_risk** score.

![image](https://github.com/user-attachments/assets/facf303c-c1fd-4b1f-83ea-88b25de66b00)

### Observations

- The **alcohol_risk = 0** group is the largest and shows a **wide spread** of values, including many high outliers just like in the case of tobacco risk.
- The **alcohol_risk = 1** group again exhibits **almost no vertical spread**, mirroring the trend observed in KRAS and EGFR.
- Groups **2** has the **narrowest** box when compared to other genes and group **3** shows moderate dispersion and more outliers compared to other genes.

### Interpretation

The updated boxplot reinforces that **ALK expression** is highly variable in individuals with **no reported alcohol usage** (alcohol_risk = 0), consistent with previous genes like EGFR and KRAS.

- The **extreme compression of values** in group `1` likely reflects a **very limited sample size** again.
- The **narrow spread in group 2**, more distinct than in other genes, suggests ALK expression may be **tightly regulated** or uniformly low among occasional drinkers.
- The increase in **dispersion and outliers in group 3** implies that ALK expression may be **more biologically reactive** to heavier alcohol exposure.

However, given the **strong imbalance in group sizes**, especially the dominance of risk level 0, these patterns should be validated with appropriate statistical tests. The results hint that **ALK may be more sensitive to alcohol-related biological changes** compared to KRAS or EGFR, especially at the extremes of alcohol consumption.

### Additional Visualizations

To provide further visual variety and support exploratory insights, the accompanying Jupyter Notebook (`.ipynb` file) includes additional plots that were not all included in this markdown report:

- **Grouped boxplots** comparing all three genes across selected tobacco/alcohol risk levels
- **Pairplots** exploring relationships between gene expressions
- **Barplots** showing mean gene expression ± standard deviation across risk scores


---

# Hypothesis Testing
---

### Hypotheses

To statistically assess the relationship between lifestyle factors (tobacco and alcohol use) and the expression of lung cancer-related genes (**KRAS**, **EGFR**, and **ALK**), I formed following hypotheses:

- **Null Hypothesis (H₀):** Tobacco and alcohol usage have **no significant effect** on the expression levels of lung cancer-related genes KRAS, EGFR, and ALK.
- **Alternative Hypothesis (H₁):** Tobacco and alcohol usage **significantly influence** the expression levels of lung cancer-related genes KRAS, EGFR, and ALK.

## ANOVA Results

To determine whether gene expression levels vary significantly across **different tobacco and alcohol risk levels**, one-way ANOVA (Analysis of Variance) tests were conducted for each gene.

### EGFR Expression

- **Tobacco Risk:** F = 2.303, p = 0.0113 
- **Alcohol Risk:** F = 1.189, p = 0.3129 

**Interpretation:** EGFR expression varies significantly across tobacco risk groups (p < 0.05), suggesting that smoking may influence EGFR gene activity. However, there is no statistically significant difference across alcohol risk groups.


### KRAS Expression

- **Tobacco Risk:** F = 3.229, p = 0.0004 
- **Alcohol Risk:** F = 0.119, p = 0.9490 

**Interpretation:** KRAS expression shows a highly significant variation with tobacco exposure. The extremely low p-value (p < 0.001) indicates a strong potential relationship. No significant association was observed with alcohol risk.


### ALK Expression

- **Tobacco Risk:** F = 5.342, p = 0.0000 
- **Alcohol Risk:** F = 1.253, p = 0.2893 

**Interpretation:** ALK expression is **strongly influenced by tobacco risk** (p < 0.001), showing the most significant variation among the three genes. Similar to the others, no significant variation was found with alcohol risk levels.

---

## Correlation Analysis: Gene Expression vs Risk Scores

To assess the **monotonic relationship** between behavioral risk factors and gene expression levels, **Spearman correlation coefficients** were calculated. This method is suitable due to its ability to capture non-linear associations and its robustness to outliers.

### Correlation with Tobacco Risk

| Gene  | Spearman r  | p-value | Interpretation                 |
|-------|-------------|--------|---------------------------------|
| EGFR  | -0.043      | 0.1792 | Very weak, non-significant      |
| KRAS  |  0.085      | 0.0069 | Weak, statistically significant |
| ALK   |  0.086      | 0.0062 | Weak, statistically significant |

**Interpretation:**  
KRAS and ALK both show **weak but statistically significant** positive correlations with tobacco risk. This suggests that as tobacco risk increases, there's a slight tendency for expression levels of these genes to rise. However, the correlation is modest (r ≈ 0.08). EGFR shows a slight negative correlation, but it is **not statistically significant**.


### Correlation with Alcohol Risk

| Gene  | Spearman r | p-value | Interpretation                      |
|-------|------------|---------|-------------------------------------|
| EGFR  |  0.125     | 0.0001  | Weak, statistically significant     |
| KRAS  | -0.033     | 0.2926  | Very weak, non-significant          |
| ALK   |  0.328     | 0.0000  | Moderate, statistically significant |

**Interpretation:**  
ALK shows a **moderate and statistically significant** positive correlation with alcohol risk (r = 0.328), suggesting that higher alcohol exposure may be associated with higher ALK expression. EGFR also has a **weak but significant** positive correlation. KRAS, in contrast, shows a very weak and non-significant negative correlation.


# Summary and Conclusion
---

Based on exploratory visualizations and hypothesis testing, we evaluated whether tobacco and alcohol use significantly affect the expression of lung cancer-related genes: **KRAS**, **EGFR**, and **ALK**.

- For **tobacco risk**, we **reject the null hypothesis (H₀)**. ANOVA and correlation analyses show statistically significant associations with all three genes, especially ALK and KRAS.
- For **alcohol risk**, we **fail to reject the null hypothesis (H₀)** overall. While ALK shows a moderate correlation and EGFR a weak one, ANOVA results do not support strong group-wise differences.

Overall, tobacco exposure has a more consistent and measurable effect on gene expression in this dataset, with ALK emerging as the most responsive gene to both behavioral risk factors.



