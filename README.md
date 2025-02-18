# Lead Scoring Case Study

## **Overview**
X Education, an online course provider for industry professionals, attracts potential learners through digital marketing efforts. These visitors can browse courses, watch videos, or fill out a form to become leads. However, only **30%** of leads typically convert into customers.  

This project aims to **develop a logistic regression model** to identify **high-potential leads** ("Hot Leads") and assign them a **Lead Score** to help prioritize follow-ups and improve conversion rates. The target lead conversion rate is 80%.

---

## **Dataset Details**
- **Source:** `Leads.csv`
- **Records:** 9,240
- **Features:** 37 (30 categorical, 7 numerical)
- **Target Variable:** `Converted` (1 = Lead converted, 0 = Not converted)

## Goal:

- To identify the features that contributes to predict Lead Conversion.
- Identifying Hot Leads by generating Lead Score for all leads, so that leads having higher Lead Scores can be contacted with priority for achieving Higher Lead Conversion Rate.


## How to Run

1.  Install dependencies:

    ```bbash
    pip install -r requirements.txt
    ```

2.  Open the notebook and run each cell in order:

    
    ```bash
    jupyter notebook Lead_Scoring_Notebook.ipynb
    ```

## Approach

### **1\. Reading and Understanding the Data**

-   Initial data with 9240 records in `leads.csv` file has 37 columns, including 30 categorical and 7 numerical columns.

### **2\. Basic Data Cleanup**

-   Replaced `'Select'` values with `NaN` as they were default dropdown selections.

-   Dropped columns with only one unique value due to lack of variance.

-   Removed columns with more than **40% missing values**.

-   Grouped categorical variables with high cardinality into bins.

-   Imputed missing values using **business understanding** (e.g., `Not Disclosed` for `Specialization` and `Occupation`).

-   Renamed some column names for better readability during EDA and model building.

### **3\. Visualizing Data and EDA**

-   **Box Plots**: `TotalVisits`, `Total Time Spent on Website`, `Page Views Per Visit`.

-   **Pair Plot**: All numeric variables.

-   **Count Plots**: Various categorical variables against `Converted` label.

-   Insights documented in the **PPT and Jupyter Notebook**.

### **4\. Data Preparation**

-   **Outlier Treatment**: Identified and removed **2.8% of data** as outliers based on box plots and percentile values.

-   **Train-Test Split**: **70:30** ratio.

-   **Missing Value Imputation**: Used **median** for numerical and **mode** for categorical columns.

-   **Categorical Encoding**:

    -   Binary classes replaced with `0/1`.

    -   Dummy variables created for multi-class categorical columns (`drop_first=True`).

-   **Feature Scaling**: Applied **MinMaxScaler**.

-   **Variance Thresholding**: Removed features with variance `< 0.001`.

-   **Correlation Analysis**: Dropped highly correlated features.

### **5\. Feature Engineering & Model Building**

-   **RFE** (Recursive Feature Elimination) selected **top 16 features** for the first logistic regression model.

-   Built **7 models**, iteratively eliminating features with **high p-values (>0.05)** and **high VIF (>5)**.

-   Evaluated **accuracy and confusion matrix** at each step.

### **6\. Model Evaluation on Training Data (Cutoff = 0.5)**

-   **Model 7** was used to predict probabilities on training data.

-   Applied **0.5 probability threshold** to classify leads as `Converted` or `Not Converted`.

-   Computed evaluation metrics: **accuracy, precision, recall, F1-score**.

### **7\. Finding Optimal Probability Cutoff & Evaluating on Train Data**

-   Analyzed **specificity, sensitivity, and accuracy** across various probability thresholds.

-   **Optimal probability cutoff** determined as **0.32**.

### **8\. Prediction on Test Data & Generating Lead Score**

-   Applied **MinMax Scaling** (only transformation) on test data.

-   Used **Model 7** to compute probabilities and classified leads using **cutoff = 0.32**.

-   Generated **Lead Score (0-100)**: `Lead Score = Probability * 100`.

    -   Higher scores indicate **hot leads**, lower scores indicate **cold leads**.

### **9\. Model Evaluation on Test Data & Interpretation**

-   Computed final **evaluation metrics** on test data, including **accuracy, recall, precision, and F1-score**.

## Key Insights

-   **Top Predictors for Conversion:**

    -   **Total Time Spent on Website**: Strong positive impact.
    -   **Occupation (Working Professional, Unemployed)**: Significant conversion likelihood.
    -   **Lead Source** (Olark Chat, Landing Page Submission): Strong predictors.
-   **Optimal Cutoff**: **0.32** probability threshold for lead conversion.

## Conclusion

This project demonstrates how Lead Scoring can enhance conversion rates by systematically identifying and prioritizing high-potential leads.