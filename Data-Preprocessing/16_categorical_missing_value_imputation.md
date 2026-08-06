# Categorical Missing Value Imputation

## Introduction

Categorical imputation replaces missing category labels with suitable estimated or explicitly defined values.

Categorical variables represent groups rather than continuous measurements.

Examples:

- Male / Female
- Yes / No
- City names
- Department names

Mean and median are generally not meaningful for categorical variables.

The main categorical imputation methods are:

- Mode Imputation
- New Missing Category
- Most Frequent Value Imputation
- Machine Learning-Based Imputation

---

# Mode Imputation

## Definition

Mode Imputation replaces missing values with the most frequently occurring category.

## Syntax

```python
mode_value = df["column"].mode()[0]

df["column"] = df["column"].fillna(
    mode_value
)
```

## Best Used When

- The missing percentage is relatively low.
- The most common category is representative.
- Category imbalance is not excessive.
- A simple imputation method is sufficient.

## Advantages

- Simple and fast
- Preserves all rows
- Uses an existing valid category
- Easy to explain

## Limitations

- Increases the frequency of the dominant category
- Can increase class imbalance
- Does not use relationships with other features
- Reduces category variability

---

# New Missing Category

## Definition

This method replaces missing categorical values with a new category such as `"Missing"` or `"Unknown"`.

## Syntax

```python
df["column"] = df["column"].fillna(
    "Missing"
)
```

## Best Used When

- Missingness may contain useful information.
- Assigning an existing category would be misleading.
- All records need to be preserved.
- The model should distinguish missing records.

## Advantages

- Does not guess an existing category
- Preserves missingness information
- Simple to implement
- Keeps all rows

## Limitations

- Creates an artificial category
- Can increase feature cardinality
- The model may overuse missingness as a signal
- May not be appropriate when values are missing completely at random

---

# Most Frequent Value Imputation

## Definition

Most Frequent Value Imputation replaces missing values with the category that appears most often.

It is equivalent in concept to Mode Imputation but can be implemented using Scikit-learn.

## Syntax

```python
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(
    strategy="most_frequent"
)

df[["column"]] = imputer.fit_transform(
    df[["column"]]
)
```

## Advantages

- Easy to integrate into ML pipelines
- Can process multiple columns
- Supports separate `fit()` and `transform()` steps
- Preserves all records

## Limitations

- Can strengthen category imbalance
- Uses a fixed replacement value
- Ignores feature relationships
- May distort category proportions

---

# Machine Learning-Based Imputation

## Definition

Machine Learning-Based Imputation trains a classification model using complete records and predicts missing categories from other available features.

## Workflow

```text
Complete Category Labels
          ↓
Train Classification Model
          ↓
Rows With Missing Labels
          ↓
Predict Missing Categories
```

## Example Models

- Decision Tree Classifier
- Random Forest Classifier
- Logistic Regression
- K-Nearest Neighbors Classifier

## Best Used When

- The missing category has meaningful relationships with other features.
- Sufficient complete records are available.
- A reliable classification model can be trained.
- Simple methods may remove useful patterns.

## Advantages

- Uses multiple feature relationships
- Produces record-specific predictions
- Can be more informative than fixed-value imputation

## Limitations

- More complex
- Depends on model quality
- Can introduce prediction bias
- Requires suitable predictors
- May overfit
- Predicted categories remain estimates

---

# Mode vs Most Frequent Imputation

| Mode Imputation | Most Frequent Imputation |
|---|---|
| Commonly implemented with Pandas | Implemented with `SimpleImputer` |
| Suitable for quick analysis | Suitable for ML pipelines |
| Uses `mode()[0]` | Uses `strategy="most_frequent"` |
| Usually produces the same replacement category | Usually produces the same replacement category |

---

# Comparison

| Method | Best Used When | Main Risk |
|---|---|---|
| Mode | Few values are missing | Dominant category increases |
| New Missing Category | Missingness is informative | Artificial category is created |
| Most Frequent | A pipeline-friendly method is required | Category imbalance may increase |
| ML-Based | Strong feature relationships exist | Model bias or incorrect predictions |

---

# Data Leakage

In a Machine Learning project, imputers should be fitted only on training data.

Correct process:

```text
Train-Test Split
      ↓
Fit Imputer on Training Data
      ↓
Transform Training Data
      ↓
Transform Testing Data
```

Fitting an imputer on the complete dataset may cause data leakage.

---

# Best Practices

- Preserve the original dataset.
- Analyze category frequencies before imputation.
- Measure missing-value percentages.
- Check category imbalance.
- Use a new category only when missingness is meaningful.
- Fit imputers only on training data in real ML workflows.
- Validate ML-based imputation models.
- Recheck missing values after imputation.
- Compare category distributions before and after imputation.
- Document the selected method and reasoning.

---

# Common Mistakes

- Applying mean or median to categorical labels
- Using Mode without checking category imbalance
- Treating Mode and Most Frequent as completely different concepts
- Creating a Missing category without understanding missingness
- Training an imputation model with weak predictors
- Modifying the original dataset directly
- Fitting an imputer before splitting the data
- Failing to verify category distributions after imputation

---

# Key Points

- Categorical variables require category-based imputation.
- Mode and Most Frequent methods usually produce the same category.
- A new Missing category preserves missingness information.
- ML-based imputation predicts categories using other features.
- No method is universally best.
- The selected method should be justified and validated.

---

# Next Topic

Outlier Detection and Treatment