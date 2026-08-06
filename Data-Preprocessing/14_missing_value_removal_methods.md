# Missing Value Removal Methods

## Introduction

Missing-value removal is a data-cleaning approach in which incomplete rows or columns are deleted from a dataset.

Removal should be used carefully because it permanently reduces the amount of available data.

The main removal methods are:

- Row Deletion
- Column Deletion
- Selective Deletion

---

# Row Deletion

## Definition

Row Deletion removes complete records that contain missing values.

## Syntax

```python
cleaned_df = df.dropna()
```

By default, `dropna()` removes a row when at least one value is missing.

Equivalent syntax:

```python
cleaned_df = df.dropna(axis=0, how="any")
```

## Parameters

### `axis=0`

Removes rows.

### `how="any"`

Removes a row when any value is missing.

### `how="all"`

Removes a row only when all values are missing.

```python
cleaned_df = df.dropna(how="all")
```

## When to Use Row Deletion

- The number of incomplete rows is very small.
- The dataset is sufficiently large.
- Removed records are not critical.
- Missingness appears random.

## Risks

- Loss of useful information
- Reduced sample size
- Possible class imbalance
- Biased results

---

# Column Deletion

## Definition

Column Deletion removes an entire feature when it contains excessive missing data or provides little value.

## Remove a Specific Column

```python
cleaned_df = df.drop(columns=["column_name"])
```

## Remove Columns Using `dropna()`

```python
cleaned_df = df.dropna(axis=1)
```

This removes columns containing at least one missing value and is usually too aggressive for real datasets.

## Threshold-Based Column Deletion

```python
minimum_non_missing = len(df) * 0.5

cleaned_df = df.dropna(
    axis=1,
    thresh=minimum_non_missing
)
```

The `thresh` parameter defines the minimum number of non-missing values required to keep a column.

## When to Use Column Deletion

- Missing percentage is extremely high.
- The feature is not important.
- Reliable imputation is not possible.
- Domain experts confirm that removal is acceptable.

## Risks

- Loss of predictive information
- Removal of an important business feature
- Reduced model capability

---

# Selective Deletion

## Definition

Selective Deletion removes rows only when missing values occur in specific important columns.

## Syntax

```python
cleaned_df = df.dropna(
    subset=["age", "serum_creatinine"]
)
```

Rows are removed only when one or more listed columns contain missing values.

## When to Use Selective Deletion

- Some features are more important than others.
- The target variable is missing.
- Essential measurements are unavailable.
- Other missing features will be handled using imputation.

## Benefits

- More controlled than complete row deletion
- Preserves records with non-critical missing values
- Reduces unnecessary data loss

---

# Comparison

| Method | Removes | Best Used When |
|---|---|---|
| Row Deletion | Complete rows | Very few records are incomplete |
| Column Deletion | Complete columns | A feature has excessive missingness |
| Selective Deletion | Rows based on selected columns | Only critical missing fields should cause deletion |

---

# Data-Loss Measurement

Before and after removal, compare the dataset shape.

```python
rows_before = len(df)
rows_after = len(cleaned_df)

rows_removed = rows_before - rows_after
```

Calculate percentage loss:

```python
data_loss_percentage = (
    rows_removed / rows_before
) * 100
```

---

# Verification

After deletion, verify the result:

```python
cleaned_df.shape
```

```python
cleaned_df.isnull().sum()
```

```python
cleaned_df.isnull().sum().sum()
```

---

# Best Practices

- Preserve the original raw dataset.
- Work on independent DataFrame copies.
- Calculate data loss before accepting deletion.
- Use domain knowledge before removing important features.
- Prefer selective deletion over blind deletion when possible.
- Document every removed row or column.
- Verify missing values after each operation.

---

# Common Mistakes

- Deleting large amounts of data without analysis
- Modifying the original DataFrame directly
- Treating a percentage threshold as a universal rule
- Removing important columns without domain validation
- Misunderstanding the `thresh` parameter
- Failing to verify the result

---

# Key Points

- Removal permanently reduces the dataset.
- Row Deletion removes incomplete records.
- Column Deletion removes incomplete features.
- Selective Deletion removes rows based on specific columns.
- Data loss should always be measured.
- The original dataset should remain unchanged.

---

# Next Topic

Numerical Missing Value Imputation