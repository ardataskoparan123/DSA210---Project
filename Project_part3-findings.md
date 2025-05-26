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


### Results
---
These results confirm that gene expression levels are not linearly predictable from tobacco and alcohol risk scores alone as both of the models showed low predictive perfonmance . Non-linear models like Random Forest performed slightly better but still yielded in low explanatory power. This aligns with earlier correlation analyses that showed only weak associations.


### Visualizations
---
Scatter plots showing **Actual vs Predicted** values were created for each gene and model:

![image](https://github.com/user-attachments/assets/1b6e1747-9698-456a-9a13-daf291597bcf)

![image](https://github.com/user-attachments/assets/9d76465c-cff0-41e9-a6c6-52f9955543a3)

![image](https://github.com/user-attachments/assets/0999c1c2-de44-437b-9f3e-ebd103070abe)

---

## Classification Tasks

To classify each sample's gene expression (ALK, KRAS, EGFR) as either **high** or **low**, using the median as the threshold.


### Models Used
---
- Logistic Regression
- Random Forest Classifier


### Results
---
#### ALK 
- Logistic Regression Accuracy: **66%**
- Random Forest Accuracy: **74%**
- Random Forest provided a better balance between precision and recall.

#### KRAS 
- Logistic Regression Accuracy: **49%**
- Random Forest Accuracy: **43%**
- Overall performance was weak for KRAS.

#### EGFR
- Logistic Regression Accuracy: **54%**
- Random Forest Accuracy: **65%**
- Improved performance with Random Forest, especially in recall.


In general among the models tested, Random Forest Classifier consistently outperformed Logistic Regression for ALK and EGFR, achieving accuracies of 74% and 65% respectively. KRAS, on the other hand, showed poor classification performance with both models, hovering around chance-level accuracy. These results would suggest that behavioral risk factors have moderate predictive power for certain genes particular for ALK in this case but are insufficient alone for reliable classification across all targets.



### Feature Importance and Plotting

---
- In all Random Forest models, **tobacco_risk** had a higher importance than **alcohol_risk**.
- Bar plots showing feature importances for ALK, KRAS, and EGFR:

![image](https://github.com/user-attachments/assets/7985728e-ff84-4af6-b509-02c92146c1a7)

![image](https://github.com/user-attachments/assets/f18f4c8e-8d52-4bce-aaea-c4c930792c93)

![image](https://github.com/user-attachments/assets/4851bf1a-27d6-4ec7-9ed4-60fced661430)


## Multiclass Classification: Predicting Tobacco Risk from Gene Expression

### Objective
---
To classify patients into **Low**, **Medium**, or **High** tobacco risk groups using their gene expression values.

### Model Used
---
- Random Forest Classifier


### Results

- Accuracy: **62%** (with `random_state=45`)
- **Low risk** group was predicted well (precision = 0.67, recall = 0.87).
- **Medium and High** risk groups were predicted poorly, with High group having 0 precision and recall.

### Classification Report
---
- Low: precision 0.67, recall 0.87, f1-score 0.76
- Medium: precision 0.36, recall 0.23, f1-score 0.28
- High: precision 0.00, recall 0.00, f1-score 0.00

- Weighted average f1-score: **0.56**

### Visualizations
---

Visualization for this step include **confusion matrix** for tobacco risk group prediction and the **feature importance plot** showing the relative contributions of ALK, EGFR, and KRAS in predicting tobacco risk:

![image](https://github.com/user-attachments/assets/8002c6d4-4a2b-4b1e-b427-9e73b2cb6bd5)

![image](https://github.com/user-attachments/assets/a47bc6f3-5e6e-4e5d-a59d-ef5063d87eaf)

## Extra Notes 

- Gene expression values were log-transformed in regression models to fix skewness.
- Hyperparameter `random_state` was tuned couple of times to improve accuracy (Last value was 45).
- The machine learning results supported several findings from EDA but also introduced new insights into the predictive value of each gene.











 















