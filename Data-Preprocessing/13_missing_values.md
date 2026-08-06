# Missing Values

## Definition

Missing values are data entries where no value is available for a particular row and column.

In Pandas, missing values are commonly represented as:

- `NaN`
- `None`
- `NaT`

Missing values must be identified before Machine Learning model training because they can affect data analysis and model performance.

---

## Common Causes of Missing Values

Missing values may occur because of:

- Incomplete forms
- Human data-entry errors
- Sensor failures
- Database issues
- Data integration problems
- Respondents skipping questions
- Information not being available

---

## Why Missing Values Are Important

Missing values can:

- Reduce data quality
- Produce incorrect statistical summaries
- Cause errors during model training
- Create biased predictions
- Reduce model performance
- Affect visualization and analysis

Missing values should first be detected and analyzed before selecting a handling method.

---

# Detecting Missing Values

## Using `isnull()`

The `isnull()` method checks every value in a DataFrame.

It returns:

- `True` when a value is missing
- `False` when a value is available

### Syntax

```python
df.isnull()
```

---

## Using `isna()`

The `isna()` method performs the same operation as `isnull()`.

### Syntax

```python
df.isna()
```

`isnull()` and `isna()` can be used interchangeably.

---

## Count Missing Values Per Column

The number of missing values in each column can be calculated using:

```python
df.isnull().sum()
```

The `sum()` method counts the `True` values because Python treats:

```text
True = 1
False = 0
```

---

## Total Missing Values

The total number of missing values in the complete dataset can be calculated using:

```python
df.isnull().sum().sum()
```

---

## Rows Containing Missing Values

Rows containing at least one missing value can be displayed using:

```python
df[df.isnull().any(axis=1)]
```

---

## Columns Containing Missing Values

Columns containing one or more missing values can be identified using:

```python
df.columns[df.isnull().any()]
```

---

# Missing Value Percentage

Missing-value percentage shows the proportion of unavailable values in each column.

## Formula

```text
Missing Percentage =
(Missing Values / Total Rows) × 100
```

## Pandas Syntax

```python
missing_percentage = (df.isnull().sum() / len(df)) * 100
```

Missing percentages help determine whether a column should later be:

- Imputed
- Selectively processed
- Removed

The final decision should also consider domain knowledge and business requirements.

---

# Missing Values Summary Table

A summary table can combine missing-value counts and percentages.

```python
missing_summary = pd.DataFrame({
    "Missing Values": df.isnull().sum(),
    "Missing Percentage": (df.isnull().sum() / len(df)) * 100
})
```

This table provides a clear overview of missing data in every column.

---

# Missing Value Visualization

Visualizations help identify missing-data patterns more quickly than raw numbers.

## Heatmap

A heatmap displays the locations of missing values across the dataset.

### Syntax

```python
sns.heatmap(df.isnull(), cbar=False)
```

Each highlighted area represents a missing value.

---

## Bar Plot

A bar plot can compare the number of missing values in different columns.

### Syntax

```python
df.isnull().sum().plot(kind="bar")
```

This visualization is useful when several columns contain missing values.

---

# Working With a Complete Dataset

The Heart Failure dataset used in this repository currently contains no missing values.

To practice missing-value detection without changing the original dataset:

1. Create a copy of the dataset.
2. Introduce missing values only into the copy.
3. Perform detection and visualization on the copied DataFrame.

```python
df_missing = df.copy()
```

This keeps the original raw dataset unchanged.

---

# Important Difference

## Detection

Detection identifies:

- Where missing values exist
- How many missing values exist
- What percentage of data is missing
- Whether missing values follow a pattern

## Handling

Handling decides what to do with missing values.

Examples include:

- Row deletion
- Column deletion
- Mean imputation
- Median imputation
- Mode imputation
- KNN imputation

Handling techniques are covered separately.

---

# Common Mistakes

- Modifying the original raw dataset directly
- Checking only the total missing count
- Ignoring missing-value percentages
- Removing rows without checking data loss
- Assuming every missing value should use the same treatment
- Treating zero as a missing value without domain evidence

---

# Best Practices

- Preserve the original dataset.
- Analyze missing counts and percentages.
- Check missing-data patterns visually.
- Understand the meaning of each feature.
- Select handling methods based on data type and domain knowledge.
- Verify the dataset again after handling missing values.

---

# Key Points

- Missing values represent unavailable information.
- `isnull()` and `isna()` detect missing values.
- `sum()` counts missing values.
- Missing percentages show the severity of missing data.
- Heatmaps and bar plots visualize missing-value patterns.
- Detection must happen before handling.
- The original raw dataset should remain unchanged.

---

# Next Topic

Missing Value Handling — Removal Methods