# Machine Learning Analysis 

---

I have implemented the machine learning tasks using the same "210_project.ipynb" notebook files used in the EDA phase. Machine learning objectives were:
    
- To perform **regression** analysis predicting gene expression levels from tobacco and alcohol usage.
- To apply binary and multiclass **classification** models predicting expression groups or risk scores.


---

## Feature Transformation

I have used two transformed features created during preprocessing throughout the Machine Learning pipeline:

 - **tobacco_risk**: A composite score based on smoking status, cigarettes per day, and years smoked.
- **alcohol_risk**: A combined score from alcohol consumption history and drinking intensity.

These two numeric feautures allowed for a straightforward input into regression and classification models.

---

## Regression Tasks

---

### Objective
---

To predict the expression levels of **ALK**, **KRAS**, and **EGFR** using tobacco and alcohol risk scores.

### Models Used 
- Linear Regression
- Random Forest Regressor
---

### Regression Performance Metrics

The objective was to predict the gene expression levels of ALK, KRAS, and EGFR using the tobacco and alcohol risk scores. Two models were evaluated: **Linear Regression** and **Random Forest Regressor**.

#### ALK

**Linear Regression**
  - R² Score: 0.0009
  - Mean Absolute Error (MAE): 0.2380
  - Mean Squared Error (MSE): 0.1442
**Random Forest**
  - R² Score: 0.0992
  - MAE: 0.2176
  - MSE: 0.1301

Random Forest showed marginal improvement over Linear Regression.

#### KRAS

**Linear Regression**
  - R² Score: -0.0594
  - MAE: 0.3076
  - MSE: 0.1625
**Random Forest**
  - R² Score: -0.0990
  - MAE: 0.3081
  - MSE: 0.1680

Both models performed poorly, and the negative R² scores indicate that the models performed worse than simply predicting the mean.

#### EGFR

**Linear Regression**
  - R² Score: 0.0114
  - MAE: 0.3560
  - MSE: 0.2138
**Random Forest**
  - R² Score: 0.0500
  - MAE: 0.3485
  - MSE: 0.2055

Some improvement with Random Forest, but overall performance remained weak.

---

### Results
---
These results confirm that gene expression levels are not linearly predictable from tobacco and alcohol risk scores alone as both of the models showed low predictive perfonmance . Non-linear models like Random Forest performed slightly better but still yielded in low explanatory power. This aligns with earlier correlation analyses that showed only weak associations.

---

### Visualizations
---
Scatter plots showing **Actual vs Predicted** values were created for each gene and model:

![image](https://github.com/user-attachments/assets/1b6e1747-9698-456a-9a13-daf291597bcf)

![image](https://github.com/user-attachments/assets/9d76465c-cff0-41e9-a6c6-52f9955543a3)

![image](https://github.com/user-attachments/assets/0999c1c2-de44-437b-9f3e-ebd103070abe)

---

## Classification Tasks
---
To classify each sample's gene expression (ALK, KRAS, EGFR) as either **high** or **low**, using the median as the threshold.

---

### Models Used
---
- Logistic Regression
- Random Forest Classifier

































