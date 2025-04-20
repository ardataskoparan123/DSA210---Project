# DSA210 Project Proposal - Arda Taşkoparan

# Effect of Tobacco and Alcohol Usage in Cancer Patients in the Expression of Cancer Related Genes

## Idea of Project

In this project, my aim is to analyze the relationship between tobacco and alcohol usage and the expression of certain genes by using genomic data from lung cancer patients. My goal is to explore how smoking and alcohol consumption habits correlate with mutations in key cancer-related genes KRAS, ALK, and EGFR and whether gene expression levels are influenced by these lifestyle factors.


---

## Description of Dataset

**Clinical Data:** Includes patient-specific tobacco and alcohol usage history, smoking onset and quit, pack years, cigarettes per day, alcohol intensity, etc.

**Genomic Data:** Contains gene expression levels of KRAS, ALK, and EGFR in lung cancer patients, categorized by smoking and alcohol history..

**Demographic Data:**  Includes gender, age, ethnicity, and other relevant factors to account for potential confounding variables.


---

## Plan

**Data Collection:**
  - All three of clinical, genomic and demographic data will collected from GDC Data Portal of National Cancer Institute (https://portal.gdc.cancer.gov/). Additional sites for data collection can be used later in the project. Filtration of the data will be done manually.
  - Data extraction will focus on patient-specific linkage between smoking status and gene expression.

**Data Preparation and Analysis**
  - Merge clinical, genomic, and demographic data into a single patient-level dataset.

  - Perform exploratory data analysis (EDA) to detect trends and missing values.

  - Compute the expression levels of genes.

  - Normalize numerical features such as amount of cigarettes patient comsumes per day to ensure comparability.

---

## Constructing Hypothesis

  - **Null Hypothesis(H0):** Tobacco and alcohol usage has no significant effect on the expression levels of lung cancer related genes KRAS, EGFR and ALK.

  - **Alternative Hypothesis(H1):** Tobacco and alcohol usage significantly influences the expression of cancer related genes KRAS, EGFR and ALK.


---

## Machine Learning Models

  - Linear regression for analyzing the relationship between tobacco usage and gene expression levels and to determine how smoking habits impact KRAS, ALK and EGFR expression.
  
  - Support vector machines for modelling potential non-linear relationships in the gene expression data and to detect hidden patterns between tobacco exposure and gene activity.
  
  - Clustering for to explore whether smokers and non-smokers naturally cluster based on their gene expression profiles.


---


## Additional Analysis: Predicting Lung Cancer Risk from Gene Expression

Beyond analyzing lung cancer patients, I will also study non-cancer individuals with similar demographic, clinical, and genomic profiles. The goal in here is to predict the probability of developing lung cancer based on gene expression patterns influenced by smoking and alcohol usage.

**Steps:**

  - Obtain data on non-cancer individuals with genomic, clinical, and demographic variables matching those of cancer patients.

  - Apply machine learning classification models (Logistic Regression, Random Forest, Neural Networks) to predict lung cancer risk.

  - Compute probabilities for individuals developing lung cancer based on their gene expression patterns.

  - Evaluate the model’s performance using metrics such as AUC-ROC, precision and recall.

I believe this additional analysis will help understand whether lifestyle factors combined with genomic data can provide early warnings for lung cancer risk.













