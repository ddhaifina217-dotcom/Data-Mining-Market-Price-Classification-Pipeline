# Vehicle Market Price Classification Pipeline

An end-to-end predictive analytics and machine learning workflow built using **Orange Data Mining**. This project reframes a vehicle valuation challenge from a regression problem into a structured multi-class classification task, enabling buyers and sellers to segment cars into distinct market value categories.

## 📌 Problem Statement
Car buyers and sellers often face difficulty in determining a fair market price due to variations in vehicle characteristics such as mileage, engine size, and manufacturer. This study applies data mining techniques using Orange to explore pricing patterns, segment vehicles into price categories, and predict price levels based on key attributes.

## 🛠️ Visual Workflow Workspace
![Orange Workflow Canvas](Data%20mining%20on%20car%20sales.svg)
*Figure 1: The complete end-to-end processing pipeline constructed using visual programming widgets in Orange.*

---

## 📋 Data Pipeline & Methodology

### 1. Data Ingestion & Initial Profiling
- **Dataset Scale**: ~50,000 rows and 7 columns (`car_sales_data`).
- **Initial Verification**: Handled via `CSV File Import` connected to `Data Info` and `Column Statistics` widgets to verify feature data types (categorical vs. numerical) and assess data spread. No missing values were natively present.

### 2. Data Cleaning & Preparation
- **Preventative Imputation**: Applied the `Impute` widget (standard best practice) to protect downstream model pipelines against future missing value entries or data format updates.
- **Target Variable Discretization**: Transformed the original continuous numerical `Price` attribute into a 3-tier categorical target array using the `Discretize` widget:
  - 🔵 **High Price**: $\geq 14,316.5$
  - 🔷 **Medium Price**: $4,286.5 \text{ to } 14,316.5$
  - 🔹 **Low Price**: $< 4,286.5$
- **Feature Selection**: Utilized the `Select Columns` widget to explicitly define the discretized `Price` column as our target variable, isolate predictive elements, and drop unhelpful downstream attributes.

### 3. Exploratory Data Analysis (EDA) Highlights
- **Informativeness-Scored Scatter Plot**: Evaluated feature pairs using Orange's built-in informativeness scoring system. A plot mapping **Mileage vs. Engine Size** revealed that cars with larger engine displacements and lower cumulative mileage heavily populate the high-price tier.
- **Statistical Variance (Box Plots)**: Analyzed mileage variance grouped by price tiers. An `ANOVA` test computed at the base of the widget returned a score of **47,064.825 ($p = 0.000$)**, proving that the variance in mileage distributions across the price categories is highly statistically significant and non-random.

---

## 📊 Predictive Modeling & Performance Analysis

To gain diverse analytical perspectives, three different classification approaches were passed through a **10-fold Cross-Validation** engine within the `Test & Score` widget to eliminate single-split bias:

1. **Logistic Regression (Linear Baseline)**: Evaluated structural class separability.
2. **k-Nearest Neighbors (kNN)**: Distance-based evaluation calculating sample pattern similarities.
3. **Random Forest (Ensemble)**: Tree-based non-linear evaluator optimizing complex feature interactions.

### Evaluation Metrics Summary
The models were validated across five main industry-standard evaluation dimensions: **AUC**, **Classification Accuracy (CA)**, **F1-Score**, **Precision/Recall**, and **Matthews Correlation Coefficient (MCC)**.


| Model | AUC | CA | F1-Score | Precision | Recall | MCC |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Logistic Regression** | **0.908** | **0.879** | **0.879** | **0.878** | **0.879** | **0.568** |
| **Random Forest** | 0.898 | 0.875 | 0.875 | 0.875 | 0.875 | 0.562 |
| **k-Nearest Neighbors** | 0.861 | 0.721 | 0.721 | 0.720 | 0.721 | 0.501 |

### Key Diagnostic Observations
- **Top Performer**: **Logistic Regression** achieved the highest overall Classification Accuracy (**87.9%**) and AUC (**0.908**), demonstrating that the price tiers are largely linearly separable based on the selected attributes.
- **Error Analysis via Confusion Matrix**: Reviewing the final matrix outputs showed exceptional predictive accuracy for the extreme polar ends (*Low* and *High* categories), with virtually zero cross-contamination errors between low and high categories. Minor transitional misclassifications occur strictly within the *Medium* category boundary thresholds due to expected feature overlap.

---

## 🚀 How to Run this Project Locally

1. **Install Runtime Application**: Download and install the latest stable version of [Orange Data Mining](https://orangedatamining.com).
2. **Download Repository**: Clone or download this project folder to your computer.
3. **Launch Workflow**: Open Orange, navigate to `File > Open`, and select the **`workflow.ows`** file inside this folder.
4. **Link Data Source**: Double-click the first `CSV File Import` node and update the directory pointer link to look at the dataset within your local folder path.
