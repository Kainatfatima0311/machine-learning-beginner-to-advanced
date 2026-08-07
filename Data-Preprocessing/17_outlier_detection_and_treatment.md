# Outlier Detection and Treatment

## Introduction

An outlier is an observation that differs significantly from most other values in a dataset.

Outliers may represent:

- Data-entry errors
- Measurement errors
- Sensor failures
- Different measurement units
- Rare but valid observations
- Natural variation

A statistically unusual value should not be removed automatically. Domain knowledge is required to determine whether an outlier is incorrect or meaningful.

---

# Impact of Outliers

Outliers can:

- Distort the mean
- Increase standard deviation
- Affect feature scaling
- Influence regression models
- Disturb distance-based algorithms
- Reduce model performance
- Create misleading visualizations

---

# Outlier Detection Methods

## Z-Score Method

### Definition

A Z-Score measures how many standard deviations a value is from the feature mean.

### Formula

```text
Z = (X - Mean) / Standard Deviation
```

A common rule is:

```text
|Z| > 3
```

### Syntax

```python
from scipy.stats import zscore

z_scores = zscore(df["column"])

outliers = df[abs(z_scores) > 3]
```

### Best Used When

- Data is approximately normally distributed.
- The distribution is reasonably symmetric.
- Extreme skewness is not present.

### Limitations

- Sensitive to outliers
- Less reliable for skewed data
- Depends on the mean and standard deviation

---

## IQR Method

### Definition

The Interquartile Range measures the spread of the middle 50% of the data.

### Formula

```text
IQR = Q3 - Q1

Lower Bound = Q1 - 1.5 × IQR
Upper Bound = Q3 + 1.5 × IQR
```

### Syntax

```python
q1 = df["column"].quantile(0.25)
q3 = df["column"].quantile(0.75)

iqr = q3 - q1

lower_bound = q1 - (1.5 * iqr)
upper_bound = q3 + (1.5 * iqr)

outliers = df[
    (df["column"] < lower_bound)
    | (df["column"] > upper_bound)
]
```

### Best Used When

- Data is skewed.
- Outliers are already present.
- A distribution-free method is preferred.

---

## Box Plot

### Definition

A Box Plot visually displays:

- Median
- First quartile
- Third quartile
- Interquartile range
- Whiskers
- Potential outliers

### Syntax

```python
sns.boxplot(x=df["column"])
```

Points outside the whiskers are potential statistical outliers.

---

## Scatter Plot

### Definition

A Scatter Plot helps identify unusual observations within the relationship between two numerical variables.

### Syntax

```python
plt.scatter(
    df["feature_1"],
    df["feature_2"]
)
```

Scatter Plots are useful for detecting isolated points, clusters, and unusual relationships.

---

## Standard Deviation Method

### Definition

This method creates bounds using the feature mean and standard deviation.

### Formula

```text
Lower Bound = Mean - 3 × Standard Deviation
Upper Bound = Mean + 3 × Standard Deviation
```

### Syntax

```python
mean_value = df["column"].mean()
std_value = df["column"].std()

lower_bound = mean_value - (3 * std_value)
upper_bound = mean_value + (3 * std_value)
```

---

# Outlier Treatment Methods

## Complete Outlier Removal

### Definition

Complete Outlier Removal deletes rows containing values outside the selected boundaries.

### Syntax

```python
cleaned_df = df[
    (df["column"] >= lower_bound)
    & (df["column"] <= upper_bound)
]
```

### Risks

- Data loss
- Removal of rare but valuable cases
- Possible sampling bias
- Reduced representation of extreme groups

---

## Winsorization

### Definition

Winsorization replaces extreme values with selected lower and upper percentile values.

### Syntax

```python
from scipy.stats.mstats import winsorize

winsorized_values = winsorize(
    df["column"],
    limits=[0.05, 0.05]
)
```

### Advantages

- Preserves all rows
- Reduces extreme influence
- Maintains dataset size

### Limitations

- Changes original values
- Requires a justified percentile threshold
- Can hide important extreme observations

---

## Capping

### Definition

Capping limits values to calculated lower and upper boundaries.

### Syntax

```python
df["column"] = df["column"].clip(
    lower=lower_bound,
    upper=upper_bound
)
```

Values below the lower bound are replaced with the lower bound, and values above the upper bound are replaced with the upper bound.

---

## Log Transformation

### Definition

Log Transformation compresses large positive values and reduces right skewness.

### Syntax

```python
import numpy as np

df["log_column"] = np.log1p(
    df["column"]
)
```

### Best Used When

- Values are non-negative.
- The distribution is right-skewed.
- Large values dominate the feature.

### Limitations

- Changes the feature scale
- Does not remove outliers
- Requires careful interpretation

---

## Domain-Specific Handling

### Definition

Domain-Specific Handling uses expert knowledge to determine whether an extreme value is an error, a rare valid observation, or a meaningful case.

Possible decisions include:

- Correcting an entry error
- Converting measurement units
- Removing an impossible value
- Preserving a valid extreme value
- Creating an outlier indicator
- Investigating the observation manually

---

# Method Comparison

| Method | Main Use | Main Limitation |
|---|---|---|
| Z-Score | Approximately normal data | Weak for skewed data |
| IQR | Skewed data | May flag valid extremes |
| Box Plot | Visual detection | Does not explain the cause |
| Scatter Plot | Relationship-based detection | Requires visual judgment |
| Standard Deviation | Symmetric distributions | Sensitive to extremes |
| Removal | Clear errors | Causes data loss |
| Winsorization | Reduce extreme influence | Alters original values |
| Capping | Enforce boundaries | Can compress meaningful values |
| Log Transformation | Reduce right skewness | Changes feature scale |
| Domain Knowledge | Validate real meaning | Requires expertise |

---

# Best Practices

- Preserve the original dataset.
- Detect outliers before handling them.
- Use more than one detection method.
- Analyze feature distributions.
- Check whether values are logically possible.
- Measure data loss before removing observations.
- Compare distributions before and after treatment.
- Avoid handling binary categorical variables as numerical outliers.
- Fit preprocessing decisions using training data in real ML workflows.
- Document every treatment decision.

---

# Common Mistakes

- Automatically deleting every Box Plot point
- Using Z-Score on highly skewed data
- Ignoring domain knowledge
- Modifying the original dataset directly
- Treating valid rare observations as errors
- Applying Log Transformation to negative values without adjustment
- Using the same handling technique for every feature
- Failing to compare the data before and after treatment

---

# Key Points

- Outlier detection and outlier treatment are separate decisions.
- Z-Score and Standard Deviation methods use the mean and standard deviation.
- IQR uses quartiles and is more robust for skewed data.
- Box Plots and Scatter Plots provide visual detection.
- Outliers can be removed, capped, winsorized, transformed, or retained.
- Domain knowledge should guide the final decision.

---

# Next Topic

Data Consistency and Standardization