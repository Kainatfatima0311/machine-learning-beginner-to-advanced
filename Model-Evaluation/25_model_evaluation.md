# Model Evaluation

## Introduction

Model Evaluation is the process of measuring how well a trained Machine Learning model performs on unseen data.

Training a model is not enough. We also need to determine whether its predictions are reliable and useful.

Different types of Machine Learning problems require different evaluation metrics.

```text
Model Evaluation
│
├── Classification Metrics
│   ├── Confusion Matrix
│   ├── Accuracy
│   ├── Precision
│   ├── Recall
│   ├── F1 Score
│   ├── ROC Curve
│   └── AUC
│
└── Regression Metrics
    ├── MAE
    ├── MSE
    ├── RMSE
    └── R² Score
```

---

# 1. Classification Metrics

Classification metrics are used when the target represents classes or categories.

Examples:

```text
Spam / Not Spam
Disease / No Disease
Pass / Fail
Cat / Dog / Bird
```

---

# Confusion Matrix

## Definition

A Confusion Matrix summarizes the predictions of a classification model by comparing actual classes with predicted classes.

For Binary Classification, it contains four possible outcomes:

- True Positive (TP)
- True Negative (TN)
- False Positive (FP)
- False Negative (FN)

---

## True Positive (TP)

The actual class is Positive and the model also predicts Positive.

Example:

```text
Actual: Disease
Predicted: Disease
```

The prediction is correct.

---

## True Negative (TN)

The actual class is Negative and the model also predicts Negative.

Example:

```text
Actual: No Disease
Predicted: No Disease
```

The prediction is correct.

---

## False Positive (FP)

The actual class is Negative but the model predicts Positive.

Example:

```text
Actual: No Disease
Predicted: Disease
```

This is also called a false alarm.

---

## False Negative (FN)

The actual class is Positive but the model predicts Negative.

Example:

```text
Actual: Disease
Predicted: No Disease
```

The model failed to detect a positive case.

---

## Confusion Matrix Summary

| Actual | Predicted | Result |
|---|---|---|
| Positive | Positive | True Positive |
| Negative | Negative | True Negative |
| Negative | Positive | False Positive |
| Positive | Negative | False Negative |

---

# Accuracy

## Definition

Accuracy measures the proportion of total predictions that are correct.

## Formula

```text
             TP + TN
Accuracy = ───────────────
           TP + TN + FP + FN
```

## Example

Suppose:

```text
Total Predictions = 100
Correct Predictions = 90
```

Then:

```text
Accuracy = 90 / 100

Accuracy = 90%
```

---

## Limitation of Accuracy

Accuracy can be misleading when target classes are highly imbalanced.

Example:

```text
95 = No Disease
5  = Disease
```

If a model predicts every observation as `No Disease`, it will still achieve:

```text
95% Accuracy
```

However, it will detect none of the actual Disease cases.

Therefore, Accuracy should not always be used alone.

---

# Precision

## Definition

Precision measures how many of the observations predicted as Positive were actually Positive.

It focuses on **predicted positive cases**.

## Formula

```text
              TP
Precision = ───────
            TP + FP
```

## Example

Suppose a model predicts:

```text
10 transactions = Fraud
```

Out of those:

```text
8 = Actually Fraud
2 = Actually Normal
```

Then:

```text
Precision = 8 / 10

Precision = 80%
```

---

## When is Precision Important?

Precision is especially important when False Positives are costly.

Examples may include:

- Spam filtering
- Situations where false alarms are expensive
- Systems where incorrectly labeling a negative case as positive has a high cost

---

# Recall

## Definition

Recall measures how many of the actual Positive observations were correctly detected by the model.

It focuses on **actual positive cases**.

Recall is also known as:

**Sensitivity** or **True Positive Rate (TPR)**.

## Formula

```text
           TP
Recall = ───────
         TP + FN
```

## Example

Suppose:

```text
Actual Positive Cases = 10
Correctly Detected = 9
```

Then:

```text
Recall = 9 / 10

Recall = 90%
```

---

## When is Recall Important?

Recall is important when missing a Positive case is costly.

Examples may include:

- Disease screening
- Fraud detection
- Safety-related detection systems

---

# Precision vs Recall

## Precision

Asks:

```text
Out of everything predicted as Positive,
how many were actually Positive?
```

Focus:

```text
Predicted Positives
```

## Recall

Asks:

```text
Out of all actual Positive cases,
how many did the model detect?
```

Focus:

```text
Actual Positives
```

### Quick Rule

```text
Precision
= Predicted Positive → How many were correct?

Recall
= Actual Positive → How many were found?
```

---

# F1 Score

## Definition

F1 Score combines Precision and Recall into a single metric.

It uses their harmonic mean.

## Formula

```text
               Precision × Recall
F1 = 2 × ─────────────────────────
               Precision + Recall
```

## Purpose

F1 Score is useful when both Precision and Recall are important.

For example:

```text
Precision = High
Recall = Low
```

A high Precision alone may hide weak Recall.

F1 Score reflects the balance between both metrics.

---

## Interpretation

F1 Score generally ranges from:

```text
0 → Poor
1 → Best possible score
```

Higher values indicate a better balance between Precision and Recall.

---

# ROC Curve

## Full Form

**ROC = Receiver Operating Characteristic**

Therefore:

**ROC Curve = Receiver Operating Characteristic Curve**

## Definition

The ROC Curve shows how a Binary Classification model behaves across different classification thresholds.

It plots:

```text
True Positive Rate
        vs
False Positive Rate
```

---

## True Positive Rate

True Positive Rate is the same as Recall.

```text
                 TP
TPR = Recall = ───────
               TP + FN
```

---

## False Positive Rate

False Positive Rate measures the proportion of actual Negative observations that were incorrectly predicted as Positive.

```text
           FP
FPR = ─────────
       FP + TN
```

---

# Classification Threshold

A classifier may first produce a probability.

Example:

```text
Patient A → 0.90
Patient B → 0.65
Patient C → 0.20
```

A threshold can convert these probabilities into classes.

Example:

```text
Probability >= 0.50
→ Positive

Probability < 0.50
→ Negative
```

Changing the threshold changes the balance between True Positives and False Positives.

The ROC Curve evaluates this behavior across different thresholds.

---

# AUC

## Full Form

**AUC = Area Under the Curve**

In this context, it refers to the area under the ROC Curve.

## Purpose

AUC summarizes the model's ability to distinguish or rank Positive observations above Negative observations across thresholds.

Typical interpretation:

```text
AUC = 1.0
→ Perfect discrimination

AUC ≈ 0.5
→ Approximately random discrimination
```

Generally, a higher AUC indicates better class discrimination.

However, AUC should not automatically be treated as the only metric required for a classification problem.

---

# Classification Metrics Summary

| Metric | Main Question |
|---|---|
| Confusion Matrix | What types of predictions were correct or incorrect? |
| Accuracy | How many total predictions were correct? |
| Precision | How reliable are predicted Positive cases? |
| Recall | How many actual Positive cases were detected? |
| F1 Score | How balanced are Precision and Recall? |
| ROC Curve | How does performance change across thresholds? |
| AUC | How well does the model discriminate between classes across thresholds? |

---

# Classification Metrics in Scikit-Learn

```python
from sklearn.metrics import (
    accuracy_score,
    precision_score,
    recall_score,
    f1_score,
    confusion_matrix,
    roc_curve,
    roc_auc_score
)
```

Example:

```python
accuracy = accuracy_score(
    y_test,
    y_pred
)

precision = precision_score(
    y_test,
    y_pred
)

recall = recall_score(
    y_test,
    y_pred
)

f1 = f1_score(
    y_test,
    y_pred
)
```

---

# 2. Regression Metrics

Regression metrics are used when the model predicts continuous numerical values.

Examples:

```text
House Price
Salary
Temperature
Sales
Revenue
```

The goal is generally to measure how far predictions are from actual values.

---

# MAE

## Full Form

**MAE = Mean Absolute Error**

## Definition

MAE calculates the average absolute difference between actual and predicted values.

## Formula

```text
MAE = Average(|Actual - Predicted|)
```

## Example

Suppose the absolute prediction errors are:

```text
10
5
2
```

Then:

```text
MAE = (10 + 5 + 2) / 3

MAE = 5.67
```

---

## Interpretation

Lower MAE generally indicates smaller average prediction errors.

MAE is expressed in the same unit as the target.

Example:

```text
Target = Rupees
MAE = Rupees
```

---

# MSE

## Full Form

**MSE = Mean Squared Error**

## Definition

MSE calculates the average of squared prediction errors.

## Formula

```text
MSE = Average((Actual - Predicted)²)
```

## Example

Suppose errors are:

```text
2
3
10
```

Squared errors:

```text
4
9
100
```

MSE calculates the average of these squared values.

---

## Why Square the Errors?

Squaring gives larger errors a much larger penalty.

Therefore, MSE is more sensitive to large prediction errors than MAE.

## Interpretation

```text
Lower MSE
→ Generally better
```

However, its unit is squared.

Example:

```text
Target = Rupees

MSE = Rupees²
```

This can make direct interpretation less intuitive.

---

# RMSE

## Full Form

**RMSE = Root Mean Squared Error**

## Definition

RMSE is the square root of MSE.

## Formula

```text
RMSE = √MSE
```

## Process

```text
Prediction Errors
      ↓
Square Errors
      ↓
Calculate Mean
      ↓
MSE
      ↓
Square Root
      ↓
RMSE
```

---

## Why Use RMSE?

RMSE:

- Penalizes large errors more strongly than MAE
- Returns the result to the original target unit

Example:

```text
Target = Rupees

RMSE = Rupees
```

## Interpretation

```text
Lower RMSE
→ Generally better
```

---

# MAE vs MSE vs RMSE

| Metric | Main Idea | Unit |
|---|---|---|
| MAE | Average absolute error | Original target unit |
| MSE | Average squared error | Squared target unit |
| RMSE | Square root of MSE | Original target unit |

### Quick Difference

```text
MAE
→ Average prediction mistake

MSE
→ Large mistakes receive stronger penalties

RMSE
→ Large mistakes receive stronger penalties
  while result remains in original unit
```

---

# R² Score

## Full Form

**R² = R-Squared**

It is also known as the **Coefficient of Determination**.

## Definition

R² measures how much of the variation in the target is explained by the model relative to a simple mean-based baseline.

---

## Basic Interpretation

```text
R² = 1
→ Perfect predictions

R² = 0
→ Performance equivalent to predicting the target mean

R² < 0
→ Worse than the mean baseline
```

Example:

```text
R² = 0.85
```

A simple interpretation is that the model explains approximately 85% of the observed variation in the target.

---

## Important Note

R² is **not Classification Accuracy**.

```text
R² = 0.85

does NOT mean

85% predictions are correct
```

Regression predictions are continuous values, so they are evaluated differently from Classification predictions.

---

# Regression Metrics in Scikit-Learn

```python
from sklearn.metrics import (
    mean_absolute_error,
    mean_squared_error,
    r2_score
)
```

Example:

```python
mae = mean_absolute_error(
    y_test,
    y_pred
)

mse = mean_squared_error(
    y_test,
    y_pred
)

rmse = mse ** 0.5

r2 = r2_score(
    y_test,
    y_pred
)
```

---

# Regression Metrics Summary

| Metric | Main Question |
|---|---|
| MAE | What is the average absolute prediction error? |
| MSE | What is the average squared prediction error? |
| RMSE | What is the error with stronger penalty for large mistakes in the original target unit? |
| R² | How much target variation does the model explain relative to the mean baseline? |

---

# Choosing Evaluation Metrics

There is no single evaluation metric that is best for every Machine Learning problem.

The correct metric depends on:

- Problem type
- Target distribution
- Class imbalance
- Cost of False Positives
- Cost of False Negatives
- Importance of large regression errors
- Real-world project requirements

---

## Classification Example

If missing Positive cases is especially costly:

```text
Recall may be important
```

If False Positives are especially costly:

```text
Precision may be important
```

If both Precision and Recall matter:

```text
F1 Score may be useful
```

For imbalanced datasets:

```text
Do not rely only on Accuracy
```

---

## Regression Example

If average error should be easy to interpret:

```text
MAE
```

If large prediction errors should receive stronger penalties:

```text
MSE / RMSE
```

If model performance relative to target variation is needed:

```text
R²
```

Multiple metrics are often evaluated together.

---

# Best Practices

- Evaluate models on unseen data.
- Select metrics according to the problem.
- Do not rely on Accuracy alone for highly imbalanced Classification.
- Inspect the Confusion Matrix to understand error types.
- Consider Precision and Recall according to the cost of FP and FN.
- Use F1 when a balance between Precision and Recall is useful.
- Understand classification thresholds when using ROC and AUC.
- Interpret MAE and RMSE in the target's original unit.
- Use R² together with error-based Regression metrics.
- Compare model performance against an appropriate baseline.
- Consider real-world consequences, not only metric values.

---

# Common Mistakes

- Evaluating a model only on training data
- Assuming high Accuracy always means a good classifier
- Confusing Precision with Recall
- Ignoring False Positives and False Negatives
- Treating ROC-AUC as the only important Classification metric
- Comparing MAE and MSE as if they have the same units
- Assuming R² is the same as Accuracy
- Assuming R² must always fall between 0 and 1
- Choosing a metric without considering the real-world problem

---

# Key Points

- Model Evaluation measures performance on unseen data.
- Classification and Regression require different metrics.
- Confusion Matrix contains TP, TN, FP, and FN.
- Accuracy measures overall correctness.
- Precision evaluates predicted Positive cases.
- Recall evaluates actual Positive cases.
- F1 Score balances Precision and Recall.
- ROC stands for Receiver Operating Characteristic.
- ROC Curve evaluates classification behavior across thresholds.
- AUC stands for Area Under the Curve.
- MAE measures average absolute error.
- MSE gives larger errors stronger penalties.
- RMSE is the square root of MSE and uses the target's original unit.
- R² measures explained variation relative to a mean baseline.
- The best evaluation metric depends on the problem.

---

# Quick Recap

```text
CLASSIFICATION

Confusion Matrix
= TP, TN, FP, FN

Accuracy
= Overall kitne correct?

Precision
= Predicted positives mein kitne correct?

Recall
= Actual positives mein kitne detect hue?

F1 Score
= Precision + Recall balance

ROC
= Receiver Operating Characteristic
= Different thresholds par model behavior

AUC
= Area Under the ROC Curve


REGRESSION

MAE
= Average absolute mistake

MSE
= Large mistakes ko stronger penalty

RMSE
= MSE ka square root
= Original target unit

R²
= Target variation kitni explain hui?
```

---

# Next Part

Machine Learning Algorithms