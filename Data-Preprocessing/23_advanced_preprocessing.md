# Advanced Preprocessing

## Introduction

Advanced Preprocessing includes techniques used for special data problems such as:

- Imbalanced target classes
- Time-dependent data

In this section, we will cover:

### Imbalanced Data Handling
- SMOTE
- Random Undersampling
- Class Weights

### Time Series Preprocessing
- Resampling
- Lag Features
- Rolling Window Features

---

# 1. Imbalanced Data Handling

## What is Imbalanced Data?

A dataset is imbalanced when one target class contains significantly more observations than another class.

Example:

```text
Class 0 = 950 samples
Class 1 = 50 samples
```

Percentage:

```text
Class 0 = 95%
Class 1 = 5%
```

A model trained on highly imbalanced data may become biased toward the majority class.

For example, predicting every observation as Class 0 could produce high accuracy while completely failing to identify Class 1.

---

# SMOTE

## Definition

SMOTE stands for:

**Synthetic Minority Over-sampling Technique**

SMOTE increases the minority class by generating synthetic examples based on existing minority-class observations.

Example:

```text
Before SMOTE:

Class 0 = 100
Class 1 = 20

After SMOTE:

Class 0 = 100
Class 1 = 100
```

SMOTE does not simply duplicate every minority observation. It creates synthetic samples based on relationships between existing minority samples.

---

## Syntax

```python
from imblearn.over_sampling import SMOTE

smote = SMOTE(
    random_state=42
)

X_train_smote, y_train_smote = smote.fit_resample(
    X_train,
    y_train
)
```

---

## Important Rule

SMOTE should normally be applied to the **training data only**.

Correct workflow:

```text
Dataset
   ↓
Train/Test Split
   ↓
SMOTE on Training Data
   ↓
Train Model
   ↓
Evaluate on Original Test Data
```

Applying SMOTE before splitting can cause information leakage and unrealistic evaluation.

---

## Advantages

- Increases minority-class representation
- Does not simply discard majority-class data
- Can help models learn minority-class patterns
- Useful for many imbalanced classification problems

## Limitations

- Creates synthetic observations
- Synthetic points may not always represent meaningful real-world examples
- Can introduce noise
- Should not be applied blindly
- Must be used carefully with validation and test data

---

# Random Undersampling

## Definition

Random Undersampling reduces class imbalance by removing observations from the majority class.

Example:

```text
Before:

Class 0 = 1000
Class 1 = 100

After Undersampling:

Class 0 = 100
Class 1 = 100
```

---

## Syntax

```python
from imblearn.under_sampling import RandomUnderSampler

undersampler = RandomUnderSampler(
    random_state=42
)

X_train_under, y_train_under = undersampler.fit_resample(
    X_train,
    y_train
)
```

---

## Advantages

- Simple
- Reduces dataset size
- Can make training faster
- Can reduce majority-class dominance

## Limitations

- Removes real observations
- Useful information may be lost
- Can be risky for small datasets

---

# Class Weights

## Definition

Class Weights assign different importance to different target classes during model training.

Instead of changing the number of observations, the model gives a larger penalty to mistakes involving the more important or underrepresented class.

Example:

```text
Class 0 → Weight 1
Class 1 → Weight 5
```

The model will treat mistakes on Class 1 as more costly.

---

## Syntax

Many Scikit-learn classifiers support:

```python
class_weight="balanced"
```

Example:

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(
    class_weight="balanced",
    max_iter=5000
)
```

Scikit-learn then calculates weights based on class frequencies.

---

## Advantages

- Does not create synthetic observations
- Does not remove observations
- Easy to use with supported algorithms
- Useful when preserving the original dataset is important

## Limitations

- Not every algorithm supports class weights
- Appropriate weighting can affect model behavior significantly
- Class weights do not automatically guarantee better performance

---

# SMOTE vs Undersampling vs Class Weights

| Method | What It Does | Data Size |
|---|---|---|
| SMOTE | Creates synthetic minority samples | Increases |
| Random Undersampling | Removes majority samples | Decreases |
| Class Weights | Changes importance of classes | Remains the same |

### Quick Comparison

```text
SMOTE
→ Increase minority representation

Random Undersampling
→ Reduce majority representation

Class Weights
→ Keep data unchanged and change class importance
```

---

# Evaluating Imbalanced Data

Accuracy alone can be misleading for highly imbalanced classification problems.

Other useful classification metrics include:

- Precision
- Recall
- F1 Score
- Confusion Matrix
- ROC-AUC

These metrics will be covered in the Model Evaluation section.

---

# 2. Time Series Preprocessing

## What is Time Series Data?

Time Series data contains observations recorded in chronological order.

Examples:

- Daily sales
- Monthly revenue
- Hourly temperature
- Daily stock prices
- Weekly website traffic

Example:

```text
Date          Sales

2026-01-01    100
2026-01-02    120
2026-01-03    150
2026-01-04    130
```

In Time Series data, the order of observations is important.

---

# Resampling

## Definition

Resampling changes the time frequency of Time Series data.

Examples:

```text
Hourly → Daily

Daily → Weekly

Daily → Monthly
```

Data can be aggregated using operations such as:

- Sum
- Mean
- Minimum
- Maximum

---

## Example

Daily sales:

```text
Monday       100
Tuesday      120
Wednesday    150
Thursday     130
Friday       140
```

These observations can be resampled into a weekly total or average.

---

## Syntax

```python
df["date"] = pd.to_datetime(
    df["date"]
)

df = df.set_index(
    "date"
)

weekly_sales = df["sales"].resample(
    "W"
).sum()
```

---

## Common Resampling Frequencies

Examples include:

```text
D → Daily
W → Weekly
ME → Month End
YE → Year End
```

The exact available aliases can depend on the Pandas version.

---

## Advantages

- Helps analyze data at different time scales
- Can reduce noisy short-term fluctuations
- Useful for trend analysis
- Can create useful aggregated datasets

---

# Lag Features

## Definition

A Lag Feature uses a previous time period's value as a feature for a later observation.

Example:

```text
Day    Sales    Previous Day Sales

1      100      NaN
2      120      100
3      150      120
4      130      150
```

---

## Syntax

```python
df["sales_lag_1"] = (
    df["sales"]
    .shift(1)
)
```

For a 2-period lag:

```python
df["sales_lag_2"] = (
    df["sales"]
    .shift(2)
)
```

---

## Why Lag Features Are Useful

Past values can contain information useful for predicting future values.

Examples:

```text
Yesterday's Sales
        ↓
Today's Sales
```

or:

```text
Previous Month Revenue
        ↓
Current Month Revenue
```

---

## Missing Values in Lag Features

The first lagged observation usually becomes missing.

Example:

```text
Original:

100
120
150

Lag 1:

NaN
100
120
```

This happens because the first observation has no previous observation.

These missing values must be handled appropriately before model training.

---

# Rolling Window Features

## Definition

Rolling Window Features summarize a moving group of recent observations.

Common rolling calculations include:

- Rolling Mean
- Rolling Sum
- Rolling Minimum
- Rolling Maximum
- Rolling Standard Deviation

---

## Example

Suppose sales are:

```text
100
120
150
130
140
```

For a 3-day Rolling Mean:

```text
Day 3:
Mean of 100, 120, 150

Day 4:
Mean of 120, 150, 130

Day 5:
Mean of 150, 130, 140
```

The window moves forward through time.

---

## Syntax

```python
df["rolling_mean_3"] = (
    df["sales"]
    .rolling(window=3)
    .mean()
)
```

---

## Rolling Sum

```python
df["rolling_sum_3"] = (
    df["sales"]
    .rolling(window=3)
    .sum()
)
```

---

## Rolling Standard Deviation

```python
df["rolling_std_3"] = (
    df["sales"]
    .rolling(window=3)
    .std()
)
```

---

## Missing Values in Rolling Features

A rolling window may create missing values at the beginning.

For example, a 3-period rolling mean needs three observations before the first complete 3-period result can be calculated.

These missing values should be handled appropriately.

---

# Lag Features vs Rolling Features

## Lag Feature

Uses a specific previous observation.

```text
Yesterday's Sales
        ↓
Lag 1
```

## Rolling Feature

Uses a summary of multiple recent observations.

```text
Last 7 Days
     ↓
Average
```

### Difference

```text
Lag
= One specific past value

Rolling
= Summary of multiple recent values
```

---

# Time Series Data Leakage

Time Series data requires special attention because future information must not leak into the past.

Incorrect:

```text
Randomly mix past and future observations
             ↓
Train/Test Split
```

This may allow future information to influence model training.

---

# Time-Based Split

A common approach is:

```text
Past
 ↓
Training Data

Future
 ↓
Testing Data
```

Example:

```text
January - October
        ↓
Training

November - December
        ↓
Testing
```

The model learns from the past and is evaluated on later observations.

---

# Creating Time Features Safely

When predicting a future value, features should only contain information that would actually have been available at prediction time.

For example:

```text
Past Sales → Valid

Future Sales → Leakage
```

This rule also applies to rolling features.

---

# Recommended Imbalanced Data Workflow

```text
Dataset
   ↓
Separate X and y
   ↓
Train/Test Split
   ↓
Apply SMOTE / Undersampling to Training Data Only
   ↓
Train Model
   ↓
Evaluate on Original Test Data
```

For class weights:

```text
Dataset
   ↓
Train/Test Split
   ↓
Train Model with Class Weights
   ↓
Evaluate on Test Data
```

---

# Recommended Time Series Workflow

```text
Time-Ordered Dataset
        ↓
Clean Data
        ↓
Create Past-Based Features
        ↓
Time-Based Train/Test Split
        ↓
Train on Past
        ↓
Evaluate on Future
```

---

# Best Practices

- Check target class distribution before handling imbalance.
- Do not rely only on accuracy for imbalanced classification.
- Apply SMOTE only to training data.
- Apply Random Undersampling only to training data.
- Consider class weights when supported by the model.
- Compare multiple imbalance-handling approaches.
- Preserve chronological order in Time Series data.
- Create lag features using past values only.
- Avoid future information in rolling features.
- Handle missing values created by lag and rolling operations.
- Choose rolling-window size based on the problem.
- Use time-based evaluation for Time Series prediction.

---

# Common Mistakes

- Applying SMOTE before Train/Test Split
- Applying SMOTE to test data
- Removing too much majority-class data
- Assuming balanced classes always produce a better model
- Using accuracy alone for severe class imbalance
- Randomly splitting Time Series data without considering time order
- Using future values as features
- Forgetting that lag features create missing values
- Forgetting that rolling windows can create missing values
- Using an inappropriate rolling-window size
- Treating synthetic SMOTE observations as real collected records

---

# Key Points

- Imbalanced data contains unequal target-class distributions.
- SMOTE creates synthetic minority-class samples.
- Random Undersampling removes some majority-class samples.
- Class Weights change the importance of target classes without changing dataset size.
- Time Series data has an important chronological order.
- Resampling changes time frequency.
- Lag Features use previous observations.
- Rolling Features summarize recent observations.
- Future information should never leak into past training data.
- Imbalance-handling methods should be evaluated rather than applied automatically.

---

# Quick Recap

```text
SMOTE
= Increase minority class using synthetic samples

Random Undersampling
= Reduce majority class

Class Weights
= Give different importance to classes

Resampling
= Change time frequency

Lag Feature
= Use a previous value

Rolling Window
= Summarize recent values

Time Series Split
= Train on past, test on future
```

---

# Part 3 Complete

With Advanced Preprocessing, the Data Preprocessing section is complete.

Topics covered include:

- Data Understanding
- Data Cleaning
- Missing Values
- Outlier Detection and Treatment
- Data Consistency and Standardization
- Data Transformation
- Feature Selection
- Dimensionality Reduction
- Data Splitting
- Advanced Preprocessing