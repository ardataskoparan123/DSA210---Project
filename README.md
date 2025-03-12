# DSA210 Project Proposal - Arda Taşkoparan

# Effect of Tobacco Usage in Cancer Patients in the Expression of Cancer Related Genes

## Idea of Project
In this project, my aim is to analyze the relationship between tobacco usage and  expression of certain genes by using genomic data from lung cancer patients. The goal is to explore how smoking habits correlate with mutations in key cancer related genes KRAS, ALK and EGFR, and whether gene expression levels are influenced by tobacco use. 

---

## Description of Dataset

**Clinical Data:** Includes patient-specific tobacco usage history, smoking onset and quit, pack years, and cigarettes per day.

**Genomic Data:** Contain gene expression levels of KRAS, ALK and EGFR in lung cancer patients, categorized by smoking history.

**Demographic Data:** Includes gender, age data to account for potential confounding factors.


---

## Plan

**Data Collection:**
  - All three of clinical, genomic and demographic data will collected from GDC Data Portal of National Cancer Institute (https://portal.gdc.cancer.gov/). Filtration of the data will be done manually.
  - Data extraction will focus on patient-specific linkage between smoking status and gene expression.

**Data Preparation and Analysis**
  - Merge clinical, genomic, and demographic data into a single patient-level dataset.

  - Perform exploratory data analysis (EDA) to detect trends and missing values.

  - Compute the expression levels of genes.

  - Normalize numerical features such as amount of cigarettes patient comsumes per day to ensure comparability.

---

## Constructing Hypothesis

  - **Null Hypothesis(H0):** Tobacco usage has no significant effect on the expression levels of lung cancer related genes KRAS, EGFR and ALK.

  - **Alternative Hypothesis(H1):** Tobacco usage significantly influences the expression of cancer related genes KRAS, EGFR and ALK.


---

## Machine Learning Models

  - Linear regression for analyzing the relationship between tobacco usage and gene expression levels and to determine how smoking habits impact KRAS, ALK and EGFR expression.
  
  - Support vector machines for modelling potential non-linear relationships in the gene expression data and to detect hidden patterns between tobacco exposure and gene activity.
  
  - Clustering for to explore whether smokers and non-smokers naturally cluster based on their gene expression profiles.


---

    













