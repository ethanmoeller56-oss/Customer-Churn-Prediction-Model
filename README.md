# Customer Churn Prediction Model

## Project Overview

Customer churn represents a major challenge for subscription-based businesses because losing existing customers can reduce revenue and increase customer-acquisition costs.

This project develops a machine learning model that identifies telecommunications customers at risk of churning. It also examines the customer characteristics most strongly associated with churn and translates the findings into actionable retention recommendations.

The project covers the complete data science workflow:

* Data cleaning and validation
* Exploratory data analysis
* Feature preprocessing
* Model development and comparison
* Hyperparameter tuning
* Classification-threshold analysis
* Feature-importance analysis
* Precision-recall evaluation
* Customer risk scoring
* Business recommendations

## Quick Results

* Built and evaluated four classification models.
* Selected a tuned Random Forest as the final model.
* Achieved a **ROC-AUC of 0.854**.
* Achieved **76.7% recall**, identifying approximately three out of every four customers who churned.
* Achieved an **Average Precision score of 0.672**.
* Identified customer tenure, contract type, total charges, internet service, and monthly charges as major churn predictors.
* Produced a scored customer dataset containing churn probabilities and risk levels.

## Business Problem

A telecommunications company wants to identify customers who are likely to cancel their service before they leave.

The project addresses three primary questions:

1. Which customers are most likely to churn?
2. What customer characteristics are associated with increased churn risk?
3. How can the company use these findings to prioritize customer-retention efforts?

Because the primary business objective is to identify as many potential churners as possible, model selection places greater emphasis on recall, F1 score, ROC-AUC, and Average Precision than on accuracy alone.

## Dataset

The project uses the **IBM Telco Customer Churn dataset**, containing information about 7,043 telecommunications customers.

The dataset includes:

* Customer demographics
* Account tenure
* Contract type
* Internet and phone services
* Online Security and Tech Support subscriptions
* Payment method
* Monthly and total charges
* Customer lifetime value
* Churn status

The target variable is `Churn Value`:

* `0` — Customer did not churn
* `1` — Customer churned

## Data Preparation

The following steps were performed before model development:

* Converted `Total Charges` to a numerical variable.
* Addressed missing total-charge values for newly enrolled customers.
* Removed customer identifiers and constant columns.
* Removed high-cardinality geographic fields.
* Removed outcome-derived variables such as `Churn Score` and `Churn Reason` to prevent data leakage.
* Removed `Churn Label` because it duplicates the target.
* Divided the data into stratified training and testing sets.
* Standardized numerical features.
* One-hot encoded categorical features.
* Used scikit-learn pipelines to ensure preprocessing was learned only from training data.

## Exploratory Data Analysis

Exploratory analysis revealed several important relationships between customer characteristics and churn.

### Contract Type

Customers with month-to-month contracts had a churn rate of approximately **42.7%**, compared with:

* **11.3%** for one-year contracts
* **2.8%** for two-year contracts

This was one of the clearest relationships identified in the analysis.

### Customer Tenure

Customers with shorter tenure were much more likely to churn. This suggests that the beginning of the customer relationship represents an especially important period for retention efforts.

### Internet Service

Fiber optic customers experienced higher churn than DSL customers. This could reflect differences in pricing, customer expectations, service quality, or support experiences.

### Support Services

Customers without Online Security or Tech Support churned more frequently than customers subscribed to these services.

### Payment Method

Customers using electronic checks experienced higher churn than customers using automatic payment methods.

These findings represent associations rather than proof of causation, but they identify valuable customer segments for further investigation and experimentation.

## Machine Learning Models

Four classification models were evaluated:

1. Logistic Regression
2. Decision Tree
3. Random Forest
4. Tuned Random Forest

The Logistic Regression model served as a strong and interpretable baseline. Decision Tree and Random Forest models were then evaluated to capture nonlinear patterns and interactions between customer characteristics.

The Random Forest was optimized with `RandomizedSearchCV` using five-fold cross-validation.

## Model Comparison

| Model                   |  Accuracy | Precision |    Recall |  F1 Score |   ROC-AUC | Average Precision |
| ----------------------- | --------: | --------: | --------: | --------: | --------: | ----------------: |
| Logistic Regression     | **0.802** | **0.643** |     0.572 |     0.605 |     0.849 |             0.646 |
| Decision Tree           |     0.727 |     0.485 |     0.468 |     0.476 |     0.644 |                 — |
| Random Forest           |     0.793 |     0.634 |     0.524 |     0.574 |     0.835 |             0.631 |
| **Tuned Random Forest** |     0.769 |     0.547 | **0.767** | **0.638** | **0.854** |         **0.672** |

Logistic Regression achieved the highest accuracy and precision. However, the tuned Random Forest achieved the strongest recall, F1 score, ROC-AUC, and Average Precision.

Since the primary objective is to identify customers at risk of leaving, the tuned Random Forest was selected as the final model.

## Final Model Performance

| Metric            | Tuned Random Forest |
| ----------------- | ------------------: |
| Accuracy          |               0.769 |
| Precision         |               0.547 |
| Recall            |               0.767 |
| F1 Score          |               0.638 |
| ROC-AUC           |               0.854 |
| Average Precision |               0.672 |

The final model identifies approximately **76.7% of customers who ultimately churn**.

Its precision of 54.7% means that slightly more than half of the customers flagged as high risk actually churned. This tradeoff may be appropriate when retention outreach is relatively inexpensive compared with the cost of losing a customer.

## Classification Threshold Analysis

Classification thresholds ranging from 0.30 to 0.75 were evaluated to measure the tradeoff between precision and recall.

The analysis demonstrated that:

* Lower thresholds increased recall but generated more false-positive predictions.
* Higher thresholds improved precision but missed more customers who ultimately churned.
* Thresholds near 0.45–0.50 produced the highest F1 score.
* The default threshold of **0.50** provided a strong balance between precision and recall.

For that reason, the default threshold was retained for the final model.

A company could still adjust this threshold according to its retention budget and the cost of unnecessary outreach.

## Precision-Recall Analysis

The tuned Random Forest achieved the highest Average Precision score:

| Model                   | Average Precision |
| ----------------------- | ----------------: |
| **Tuned Random Forest** |         **0.672** |
| Logistic Regression     |             0.646 |
| Random Forest           |             0.631 |

This indicates that the tuned Random Forest maintained the strongest overall balance between precision and recall across classification thresholds.

## Feature Importance

The most influential customer characteristics in the tuned Random Forest included:

1. Tenure Months
2. Contract Type
3. Total Charges
4. Internet Service
5. Dependents
6. Monthly Charges
7. Payment Method
8. Online Security
9. Tech Support

Random Forest feature importance shows how strongly a variable influenced predictions, but it does not show whether the variable increased or decreased churn risk. Therefore, feature importance was interpreted together with the exploratory analysis.

## Customer Risk Scoring

The final model was used to generate a customer-level scoring dataset containing:

* Predicted churn classification
* Churn probability
* Customer risk level

Customers were separated into three risk groups:

| Risk Level | Churn Probability |
| ---------- | ----------------: |
| Low        |            0%–30% |
| Medium     |     Above 30%–60% |
| High       |    Above 60%–100% |

This output can help a customer-retention team prioritize outreach toward customers with the highest predicted probability of leaving.

## Business Recommendations

### 1. Prioritize New Customers

Develop onboarding and retention programs focused on customers during their first year of service, when churn risk is highest.

Potential strategies include:

* Early satisfaction surveys
* Proactive service check-ins
* Onboarding assistance
* First-year loyalty incentives

### 2. Encourage Longer-Term Contracts

Offer incentives that encourage month-to-month customers to transition to one-year or two-year contracts.

Potential incentives include:

* Promotional pricing
* Loyalty discounts
* Free service upgrades
* Bundled support services

### 3. Investigate the Fiber Optic Experience

Conduct additional analysis of fiber optic customers to determine whether their higher churn is associated with:

* Pricing
* Service reliability
* Technical-support experiences
* Customer expectations
* Geographic service differences

### 4. Promote Support Services

Evaluate whether bundling Online Security and Tech Support into selected plans improves customer satisfaction and retention.

Because the current analysis is observational, an A/B test should be conducted before concluding that these services directly reduce churn.

### 5. Review Electronic-Check Customers

Investigate why customers using electronic checks experience higher churn. Automatic payment incentives or a simplified billing experience may improve retention.

### 6. Use Risk-Based Retention Campaigns

Use the final model to rank customers by churn probability and focus retention resources on customers with the greatest predicted risk.

Retention offers could be adjusted according to:

* Churn probability
* Customer lifetime value
* Monthly charges
* Contract type
* Cost of the proposed incentive

## Technologies Used

* Python
* Pandas
* NumPy
* Matplotlib
* Seaborn
* Scikit-learn
* Jupyter Notebook
* Microsoft Excel

## How to Run the Project

1. Clone the repository:

```bash
git clone https://github.com/ethanmoeller56-oss/Customer-Churn-Prediction-Model.git
```

2. Enter the project directory:

```bash
cd Customer-Churn-Prediction-Model
```

3. Install the required packages:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl jupyter
```

4. Start Jupyter Notebook:

```bash
jupyter notebook
```

5. Open the customer churn notebook and run the cells in order.

## Repository Contents

* `Telcom_customer_churn.ipynb` — Complete analysis and machine learning workflow
* `Telco_customer_churn.xlsx` — Original customer churn dataset
* `customer_churn_predictions.xls` — Customer-level predictions and churn probabilities
* `README.md` — Project overview, results, and instructions
* `LICENSE` — Repository license

## Limitations

This analysis has several limitations:

* The dataset represents a single historical snapshot.
* Customer behavior may change over time.
* Feature relationships should not automatically be interpreted as causal.
* Customer interactions, service outages, complaints, and usage behavior were not available.
* The financial value of true positives, false positives, and false negatives was not provided.
* Model performance should be monitored on new customer data before operational use.

## Future Improvements

Future work could include:

* Evaluating XGBoost, LightGBM, or CatBoost.
* Selecting the classification threshold using explicit retention costs and benefits.
* Using out-of-fold predictions to create an unbiased historical scoring file.
* Adding customer-support interactions, service outages, and usage behavior.
* Applying permutation importance or SHAP for more robust model interpretation.
* Building an interactive Streamlit churn-risk dashboard.
* Deploying the model through a Flask or FastAPI service.
* Monitoring model performance and data drift.
* Periodically retraining the model as new customer data becomes available.

## Author

**Ethan Moeller**

Data Science | Python | SQL | Machine Learning | Data Visualization

