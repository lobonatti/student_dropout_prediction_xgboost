# student_dropout_prediction_xgboost
Predicting university student dropout and academic success with CART, Bagging, Random Forest and XGBoost in R.

# Predicting Student Dropout and Academic Success with Tree Ensembles

Code accompanying my thesis **"Tree Ensemble Methods, Gradient Boosting and
Extreme Gradient Boosting. Theory and Their
Application in Predicting Students’ Dropout
and Academic Success"**.
Full thesis: [`thesis.pdf`](thesis.pdf)

The project classifies university students as **Dropout**, **Enrolled** or **Graduate**, and identifies the factors behind university success and dropout, comparing CART, Bagging, Random Forest and Extreme Gradient Boosting (XGBoost) in R.

---

## Dataset

*Student's Dropout and Academic Success*: 4,424 students of a Portuguese university, 36 variables (18 numerical, 18 categorical) covering demographic, social, economic and academic information (admission grade, curricular units per semester, scholarship, debt, parents' occupation, macroeconomic indicators).

- Target: `Dropout` (1,421), `Enrolled` (794), `Graduate` (2,209), so the classes are imbalanced.
- Source: [https://archive.ics.uci.edu/dataset/697/predict+students+dropout+and+academic+success].
- The dataset is **not included** in this repository (see "How to run").

## What the project does

1. **Variable selection and preprocessing**: merge highly correlated 1st/2nd-semester variables (|corr| > 0.85) into yearly totals, drop non-informative variables (boxplot analysis), group rare levels of parents' occupation and qualification.
2. **Split**: 60% train, 20% validation, 20% test.
3. **Models**: CART (baseline), Bagging, Random Forest, XGBoost.
4. **XGBoost tuning**: stepwise cross-validation strategy (5-fold, minimizing multiclass log-loss), one or two parameters at a time: `nrounds`, `max_depth` / `min_child_weight`, `gamma`, `subsample` / `colsample_bytree`, `alpha` / `lambda`, `eta`, `max_delta_step`. Class imbalance is handled with class weights.
5. **Evaluation**: accuracy, sensitivity, specificity and balanced accuracy per class, with a focus on the sensitivity of the **Dropout** class (identifying at-risk students).
6. **Variable importance**: which variables drive the predictions.

In the thesis the analysis is extended to a **binary problem** (*Success* vs *Failed*) and to a **pre-enrollment analysis** that keeps only information available before the student starts university.

## Main results (test set, three classes)

| Model | Accuracy | Balanced accuracy |
|---|---|---|
| CART (baseline) | 0.735 | 0.744 |
| Bagging | 0.742 | 0.766 |
| Random Forest | 0.751 | 0.774 |
| **XGBoost** | **0.772** | **0.804** |

- XGBoost has the best overall performance and the highest Dropout sensitivity (0.77).
- In the binary setting (*Success* vs *Failed*), XGBoost reaches about 0.86 accuracy.
- The most important variable in every model is the **total number of curricular units approved** (first year); before enrollment, the most relevant variables are **course** and **admission grade**.

Detailed results, plots and discussion are in the thesis.

## Repository structure

```
├── README.md
├── thesis.pdf
├── 01_data_preparation.Rmd   # variable selection, preprocessing, train/validation/test split
├── 02_models.Rmd             # CART, Bagging, Random Forest, XGBoost (tuning and evaluation)
└── data/                     # dataset (not included) and prepared_data.rds (generated)
```

## How to run

1. Install R and RStudio, then the required packages:
   ```r
   install.packages(c("caTools", "dplyr", "corrplot", "nnet", "pheatmap",
                      "xgboost", "gbm", "caret", "cvms", "randomForest",
                      "tree", "ggplot2", "class"))
   ```
2. Download the dataset and save it as `data/data.csv` (semicolon-separated).
3. In `01_data_preparation.Rmd`, update the path in the `read.csv(...)` call.
4. Knit `01_data_preparation.Rmd`: it creates `data/prepared_data.rds`.
5. Knit `02_models.Rmd`.

> The XGBoost tuning runs cross-validation over many parameter combinations and can take a long time.
> Small differences from the thesis figures may appear depending on package versions and random seeds.

## Tools

R, RStudio, R Markdown, `xgboost`, `randomForest`, `tree`, `caret`, `ggplot2`, `dplyr`.

## Author

[Bonatti Lorenzo], [email: bonatti.lorenzo1@gmail.com]
