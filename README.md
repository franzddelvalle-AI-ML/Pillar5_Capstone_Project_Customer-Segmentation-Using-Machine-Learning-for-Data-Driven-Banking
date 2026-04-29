# Customer Segmentation Using Machine Learning (Clustering)

## 📌 Project Overview

This project applies **unsupervised machine learning (clustering)** to segment bank customers into meaningful groups based on financial behavior and demographics.

Traditional banking strategies treat customers as a single group, leading to inefficient targeting. This project aims to enable **data-driven segmentation** for better marketing, retention, and product strategies.

---

## 🎯 Business Problem

Banks lack personalized targeting due to limited segmentation.

### Goals:

* Improve marketing efficiency
* Increase product adoption
* Enhance customer retention

---

## 📂 Dataset

* **Source:** BankChurners Dataset
* **Size:** 10,127 customers
* **Features:**

  * Demographics (Age, Gender, Income, Education)
  * Credit behavior (Credit limit, utilization)
  * Transactions (Amount, count)

---

## ⚙️ Methodology

### 1. Data Preprocessing

* Dropped irrelevant columns (e.g., CLIENTNUM, Attrition_Flag)
* Handled missing values (mean/mode imputation)
* One-hot encoding for categorical variables
* Feature scaling using `StandardScaler`

### 2. Dimensionality Reduction

* Applied **PCA (95% variance retained)**
* Reduced features from 32 → 24

### 3. Clustering Models

* **K-Means**
* **DBSCAN**
* **Hierarchical Clustering**

---

## 📊 Model Results

| Model        | Parameters              | Silhouette Score    |
| ------------ | ----------------------- | ------------------- |
| K-Means      | K = 4                   | 0.079               |
| Hierarchical | K = 4                   | 0.0566              |
| DBSCAN       | eps=2.0, min_samples=48 | Failed (100% noise) |

### Key Insight:

* K-Means performed slightly better but overall cluster separation is weak
* DBSCAN was ineffective due to lack of density-based structure

---

## 👥 Customer Segments (K-Means)

* **Cluster 0:** Moderate credit, lower transactions
* **Cluster 1:** High credit, high transactions, low utilization
* **Cluster 2:** Lower credit, higher utilization
* **Cluster 3:** Lowest credit, highest utilization

---

## 💡 Business Recommendations

* Personalized marketing per segment
* Retention strategies for low-activity users
* Product targeting (e.g., premium vs budget users)
* Risk-based credit strategies

---

## ⚖️ Ethical AI Considerations

Customer segmentation must be used responsibly:

* Avoid bias based on gender, income, or demographics
* Ensure transparency in segmentation logic
* Protect customer data privacy
* Monitor for unfair targeting or exclusion
## ⚖️ Fairness Audit Results

A fairness audit was conducted to assess whether the clustering model exhibits potential bias across demographic groups such as **Gender** and **Income Category**.

### 📊 Cluster Distribution by Gender

Analysis of the cluster distribution shows **clear imbalance across gender groups**.

* **Cluster 0 and Cluster 1** are dominated by **male customers**
* **Cluster 2 and Cluster 3** are dominated by **female customers**

As shown in the visualization  (page 1), Cluster 3 contains a significantly higher proportion of female customers compared to males.

### 🔍 Interpretation

This suggests that:

* The clustering model is capturing **behavioral differences correlated with gender**
* However, it may also reflect **implicit bias in feature patterns**

⚠️ **Risk:**

* If used in decision-making (e.g., marketing, credit targeting), this may lead to **unequal treatment across genders**

---

### 📊 Cluster Distribution by Income Category

From the income distribution chart  (page 2):

* **Higher-income groups ($80K–$120K and $120K+) are concentrated in specific clusters**
* **Lower-income groups (<$40K)** dominate other clusters

### 🔍 Interpretation

* Clusters are strongly influenced by **income segmentation**
* This is expected in financial behavior modeling
* However, it introduces potential **economic bias in segmentation**

⚠️ **Risk:**

* Certain customer segments may be unfairly prioritized or excluded based on income

---

### ⚖️ Fairness Summary

* Clusters are **not demographically neutral**
* Observable skew exists across:

  * Gender
  * Income

👉 The model reflects **real-world behavioral differences**, but also introduces **potential bias risks** if used operationally.

---

## 🔍 Model Interpretability (SHAP Analysis)

To understand how features influence clustering behavior, **SHAP (SHapley Additive exPlanations)** was applied to the K-Means model.

### 📊 SHAP Summary Plot (Cluster 0)

The SHAP summary plot  (page 4) shows the **impact of PCA components (PC features)** on cluster assignment.

### 🔍 Key Observations

* **PC_0, PC_16, and PC_1** are the most influential features
* Both **high and low feature values** contribute differently to cluster assignment
* The spread of SHAP values indicates **non-linear relationships captured by clustering**

---

### 🧠 Interpretation

* The model does not rely on a single feature, but rather a **combination of transformed features (PCA components)**
* PCA compresses original variables, so:

  * Each principal component represents a mix of:

    * Credit behavior
    * Transaction activity
    * Demographics

👉 This makes interpretation more complex but also more powerful

---

### ⚠️ Limitation of SHAP in Clustering

* SHAP is traditionally used for supervised models
* In K-Means:

  * It explains **distance to cluster centroids**, not predictions
* Therefore:

  * Interpretations are **approximate but still useful**

---

## 🚨 Key Takeaway

* The clustering model is **data-driven and effective for segmentation**
* However:

  * It shows **demographic imbalance**
  * And relies on **latent feature representations (PCA)**

👉 This reinforces the need for:

* Responsible use
* Fairness monitoring
* Business-aware deployment

---


## 🚀 How to Run the Project

```bash
pip install -r requirements.txt
jupyter notebook
```

Make sure `BankChurners.csv` is in the same directory.

---

## 📁 Project Structure

```
📦 Customer-Segmentation-ML
 ┣ 📂 data
 ┣ 📂 notebooks
 ┣ 📂 models
 ┣ 📂 reports
 ┣ README.md
 ┗ requirements.txt
```

---

## 📈 Future Improvements

* Add **Davies-Bouldin Index evaluation**
* Perform **bias and fairness analysis**
* Improve clustering using advanced methods (GMM, Spectral)
* Deploy as dashboard or API

## 📊 Data Dictionary

| Feature                  | Type        | Description                       | Example / Range                |
| ------------------------ | ----------- | --------------------------------- | ------------------------------ |
| Customer_Age             | Numerical   | Age of the customer               | 26 – 73                        |
| Gender                   | Categorical | Customer gender                   | Male, Female                   |
| Dependent_count          | Numerical   | Number of dependents              | 0 – 5                          |
| Education_Level          | Categorical | Highest education attained        | Graduate, High School, Unknown |
| Marital_Status           | Categorical | Marital status of customer        | Married, Single, Divorced      |
| Income_Category          | Categorical | Annual income bracket             | <40K, 40K–60K, 60K–80K, etc.   |
| Card_Category            | Categorical | Type of credit card               | Blue, Silver, Gold, Platinum   |
| Months_on_book           | Numerical   | Duration as a customer (months)   | 13 – 56                        |
| Total_Relationship_Count | Numerical   | Number of bank products held      | 1 – 6                          |
| Months_Inactive_12_mon   | Numerical   | Months inactive in last 12 months | 0 – 6                          |
| Contacts_Count_12        |             |                                   |                                |

---

## 👤 Author

Franz Joseph Ryle del Valle
AI/ML Post Graduate Diploma – Asian Institute of Management
