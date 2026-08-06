# Numerical Missing Value Imputation

## Introduction

Numerical imputation replaces missing numerical values with estimated values instead of deleting incomplete records.

Imputation helps preserve the dataset size, but the selected method must match the feature distribution, data type, relationships, and domain context.

The main numerical imputation techniques are:

- Mean Imputation
- Median Imputation
- Mode Imputation
- Conditional Mean Imputation
- KNN Imputation
- Regression Imputation

---

# Mean Imputation

## Definition

Mean Imputation replaces missing values with the arithmetic average of the available values in the same column.

## Syntax

```python
mean_value = df["column"].mean()

df["column"] = df["column"].fillna(
    mean_value
)
```

## Best Used When

- The feature is numerical.
- The distribution is approximately symmetric.
- Extreme outliers are not present.
- The missing percentage is relatively low.

## Advantages

- Simple to understand
- Fast to calculate
- Preserves all rows

## Limitations

- Sensitive to outliers
- Reduces variance
- Can distort the original distribution
- Repeats the same value across missing records

---

# Median Imputation

## Definition

Median Imputation replaces missing values with the middle value of the sorted available data.

## Syntax

```python
median_value = df["column"].median()

df["column"] = df["column"].fillna(
    median_value
)
```

## Best Used When

- The feature is skewed.
- Outliers are present.
- A robust central value is required.

## Advantages

- Less sensitive to outliers
- Suitable for skewed numerical data
- Simple and efficient

## Limitations

- Reduces variability
- May not preserve feature relationships
- Uses the same replacement value for every missing record

---

# Mode Imputation

## Definition

Mode Imputation replaces missing numerical values with the most frequently occurring value.

## Syntax

```python
mode_value = df["column"].mode()[0]

df["column"] = df["column"].fillna(
    mode_value
)
```

## Best Used When

- Numerical values are discrete.
- A limited set of values repeats frequently.
- The most common value is meaningful.

## Advantages

- Simple
- Preserves valid discrete values
- Does not create decimal values for integer-like features

## Limitations

- Can over-represent the most frequent value
- May distort the distribution
- Multiple modes may exist

---

# Conditional Mean Imputation

## Definition

Conditional Mean Imputation calculates separate mean values within meaningful groups and uses the appropriate group mean to fill missing values.

## Syntax

```python
df["column"] = (
    df.groupby("group_column")["column"]
    .transform(
        lambda group: group.fillna(
            group.mean()
        )
    )
)
```

## Best Used When

- A meaningful grouping variable exists.
- Group averages differ.
- Domain knowledge supports the grouping relationship.

## Advantages

- More context-aware than global mean imputation
- Preserves some group-level differences
- Uses related feature information

## Limitations

- Requires a meaningful group
- Small groups may produce unreliable means
- Missing group labels create additional problems
- Incorrect grouping may introduce bias

---

# KNN Imputation

## Definition

KNN Imputation estimates missing values using the values of the most similar observations.

## Syntax

```python
from sklearn.impute import KNNImputer

imputer = KNNImputer(
    n_neighbors=5
)

imputed_array = imputer.fit_transform(
    numerical_data
)
```

## Best Used When

- Similar records exist in the dataset.
- Multiple numerical features contain useful relationships.
- The dataset is not excessively large.

## Advantages

- Uses multiple feature relationships
- Can produce more personalized estimates
- More flexible than fixed-value imputation

## Limitations

- Computationally expensive
- Sensitive to feature scales
- Sensitive to irrelevant features
- Requires a suitable number of neighbors

---

# Regression Imputation

## Definition

Regression Imputation trains a regression model on complete records and predicts the missing values using other available features.

## Workflow

```text
Complete Records
      ↓
Train Regression Model
      ↓
Incomplete Records
      ↓
Predict Missing Values
```

## Best Used When

- The missing numerical feature has meaningful relationships with other variables.
- Sufficient complete records are available.
- The regression model provides reliable predictions.

## Advantages

- Uses relationships among variables
- Can produce record-specific values
- More advanced than constant-value imputation

## Limitations

- Depends on model quality
- May overfit
- Can underestimate uncertainty
- Requires careful feature selection
- Predicted values may introduce artificial patterns

---

# Comparison

| Method | Suitable For | Main Limitation |
|---|---|---|
| Mean | Symmetric numerical data | Sensitive to outliers |
| Median | Skewed data with outliers | Reduces variability |
| Mode | Discrete numerical data | Over-represents common value |
| Conditional Mean | Meaningful grouped data | Depends on grouping quality |
| KNN | Similar observations | Scale-sensitive and slower |
| Regression | Predictable numerical features | Model-dependent |

---

# Method Selection Guide

- Use Mean for approximately symmetric data.
- Use Median for skewed data or data containing outliers.
- Use Mode for discrete numerical variables.
- Use Conditional Mean when meaningful groups exist.
- Use KNN when multiple related numerical features are available.
- Use Regression when the missing feature can be predicted reliably.

---

# Data Leakage

In a Machine Learning workflow, an imputer should be fitted only on the training data.

Correct process:

```text
Split Data
    ↓
Fit Imputer on Training Data
    ↓
Transform Training Data
    ↓
Transform Testing Data
```

Fitting an imputer on the entire dataset may cause data leakage.

---

# Best Practices

- Preserve the original raw dataset.
- Analyze feature distribution before imputation.
- Measure missing percentages.
- Compare multiple suitable methods.
- Verify data types after imputation.
- Recheck missing values.
- Evaluate the impact on model performance.
- Fit imputers only on training data in real ML pipelines.
- Document the selected method and reasoning.

---

# Common Mistakes

- Using Mean Imputation for highly skewed data
- Ignoring outliers before selecting Mean
- Using Mode for continuous measurements
- Choosing arbitrary groups for Conditional Mean
- Applying KNN without considering feature scale
- Training a regression imputer on incomplete target values
- Modifying the original dataset directly
- Fitting the imputer on the complete dataset before splitting

---

# Key Points

- Imputation preserves records but introduces estimated values.
- No single method is best for every numerical feature.
- Mean and Median are simple statistical approaches.
- Mode is useful for discrete numerical features.
- Conditional Mean uses group-level information.
- KNN uses neighboring observations.
- Regression uses predictive relationships.
- Method selection should be validated instead of assumed.

---
