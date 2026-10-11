# Credit Card Approval Prediction

**Author:** Suryansh Mishra · [GitHub](https://github.com/Suryansh2111) · [LinkedIn](https://www.linkedin.com/in/suryansh-mishra-bb3879339/)

## Problem

Card issuers want to approve good applicants quickly while catching the ones who should be denied. This project identifies the variables that drive approval and builds a model to pre-screen applications, on extremely imbalanced data where only about 1 in 200 applicants is denied.

## Data

[Kaggle Application Data](https://www.kaggle.com/datasets/caesarmario/application-data): after cleaning, **25,128 applicants**, each with gender, car and property ownership, children, income and income type, education, family status, housing, job title, family size, age, years working, and total good and bad debt.

Target `Status`: 1 = approved (25,007), 0 = denied (**121, 0.48%**). A model that approves everyone is already 99.5% accurate, so models were judged on how many denials they caught.

## Approach

- Data quality checks: completeness, outliers, and how many distinct values each categorical column has.
- Binned numeric variables (income, age, years working, family size, good debt).
- Grouped high-cardinality categories into risk clusters by denial rate. For example, "Incomplete higher" and "Lower secondary" education form a high-risk cluster; "Academic degree" and "Secondary special" form a low-risk cluster.
- One-hot encoded the clusters; balanced the training data with SMOTE.
- Selected variables using correlation with the target, decision-tree splits and business sense.
- Compared 8 models across training, validation and test partitions: Logistic Regression, Linear Discriminant Analysis, Decision Tree, Boosted Tree, Bootstrap Forest, Naive Bayes, K-Nearest Neighbors and a Neural Network.

## Results

| Model | Denials caught (class 0) | Approvals correct (class 1) |
| --- | --- | --- |
| **Neural Network (selected)** | **87%** | **99%** |
| Naive Bayes | 97% | 55% |
| Boosted Tree | about 50% | 99%+ |
| K-Nearest Neighbors | 33% | 100% |
| Decision Tree | 16-22% | 99%+ |

The neural network was chosen because it caught most denials without rejecting good customers, and performed the same on training, validation and test data. Naive Bayes caught more denials but rejected almost half of good applicants.

**Top drivers** (neural network): total good debt (about 40% of importance), education type and job title (about 20% each), then total debt. In the Bootstrap Forest, total bad debt and total good debt ranked highest.

## Recommendations

- Auto-approve high-confidence applicants; send those below a set approval probability to a human underwriter.
- Avoid targeting applicants with both high bad debt and high good debt; target high-income applicants with academic degrees for offers.
- Collect more data on denied applicants and retrain regularly; log underwriter decisions as new labels.

## Limitations

- Only 121 denials, so recall on denials moves several points per misclassified applicant; treat results as directional.
- No credit score (FICO) in the data, which real lenders rely on.
- Gender, age and family status are restricted in US lending (ECOA); a production model would exclude or test them for disparate impact.

## Files

- `Credit_Card_Approval_Presentation.pptx`: full analysis, model comparison and recommendations
- `Application_Data_Transformed.xlsx`: cleaned data with binned and clustered variables

**Tools:** JMP Pro (model comparison), Python (preprocessing, one-hot encoding, SMOTE)
