# Data Consistency and Standardization

## Introduction

Data Consistency and Standardization ensure that data follows a uniform format and numerical features are represented on suitable scales before Machine Learning.

This process mainly includes:

- Text Cleaning
- Numerical Scaling

---

# 1. Text Cleaning

Text Cleaning removes formatting inconsistencies from categorical and textual data.

Common problems include:

- Different letter cases
- Extra spaces
- Unnecessary special characters
- Unicode inconsistencies
- Spelling variations

---

## Convert to Lowercase

### Definition

Lowercase conversion transforms text values into the same letter case.

### Example

```text
HR
Hr
hr
```

After cleaning:

```text
hr
hr
hr
```

### Syntax

```python
df["column"] = df["column"].str.lower()
```

### Key Points

- Makes text values consistent
- Prevents duplicate categories caused by letter case
- Useful for categorical and textual features

---

## Remove Extra Spaces

### Definition

Extra-space removal eliminates unnecessary spaces from the beginning and end of text values.

### Example

```text
" Male"
"Male "
"  Male  "
```

After cleaning:

```text
"Male"
```

### Syntax

```python
df["column"] = df["column"].str.strip()
```

### Key Points

- Prevents visually identical categories from being treated differently
- Commonly required for manually entered data
- `strip()` removes leading and trailing spaces

---

## Remove Special Characters

### Definition

Special-character cleaning removes unnecessary symbols from text values.

### Example

```text
HR!
HR#
H.R.
```

These values may require standardization if the symbols do not carry useful meaning.

### Syntax

```python
df["column"] = df["column"].str.replace(
    r"[^a-zA-Z0-9\s]",
    "",
    regex=True
)
```

### Key Points

- Removes unwanted symbols
- Regular expressions can be used for cleaning
- Special characters should not be removed blindly
- Meaningful characters must be preserved

---

## Unicode Normalization

### Definition

Unicode Normalization converts different Unicode representations of text into a consistent form.

### Syntax

```python
import unicodedata

text = unicodedata.normalize(
    "NFKC",
    text
)
```

### Key Points

- Useful for multilingual datasets
- Helps standardize visually similar characters
- Useful when data comes from different external sources

---

## Spelling Correction

### Definition

Spelling Correction standardizes incorrectly or inconsistently written category values.

### Example

```text
Lahore
Lahor
Lahroe
```

If all values represent the same category, they can be standardized to:

```text
Lahore
```

### Example

```python
corrections = {
    "lahor": "lahore",
    "lahroe": "lahore"
}

df["city"] = df["city"].replace(corrections)
```

### Key Points

- Prevents unnecessary duplicate categories
- Domain knowledge should be used
- Automatic correction should be applied carefully
- Similar-looking words may have different meanings

---

# 2. Numerical Scaling

## Definition

Numerical Scaling transforms numerical features so that differences in their original measurement scales do not unnecessarily influence Machine Learning algorithms.

### Example

```text
Age               = 60
Platelets          = 250000
Serum Creatinine   = 1.2
```

These features have very different numerical scales.

Scaling can make them more suitable for algorithms that are sensitive to feature magnitude.

---

# Min-Max Scaling

## Definition

Min-Max Scaling usually transforms values into a range between `0` and `1`.

## Formula

```text
X_scaled = (X - X_min) / (X_max - X_min)
```

## Syntax

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

scaled_data = scaler.fit_transform(
    df[["column"]]
)
```

## Advantages

- Produces a fixed range
- Easy to understand
- Preserves relative ordering

## Limitations

- Sensitive to outliers
- Extreme values can compress other observations

---

# Standard Scaling

## Definition

Standard Scaling transforms values using their mean and standard deviation.

The transformed feature generally has:

```text
Mean ≈ 0
Standard Deviation ≈ 1
```

## Formula

```text
Z = (X - Mean) / Standard Deviation
```

## Syntax

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

scaled_data = scaler.fit_transform(
    df[["column"]]
)
```

## Key Points

- Commonly used in Machine Learning
- Does not restrict values to a fixed range
- Useful for algorithms sensitive to feature scales

---

# Robust Scaling

## Definition

Robust Scaling uses the median and Interquartile Range instead of the mean and standard deviation.

## Syntax

```python
from sklearn.preprocessing import RobustScaler

scaler = RobustScaler()

scaled_data = scaler.fit_transform(
    df[["column"]]
)
```

## Advantages

- More resistant to outliers
- Uses robust statistics
- Suitable when extreme observations are present

## Key Point

Robust Scaling does not remove outliers. It only reduces their influence during scaling.

---

# Log Transformation

## Definition

Log Transformation compresses large values and can reduce right-skewness in numerical features.

## Syntax

```python
import numpy as np

df["log_column"] = np.log1p(
    df["column"]
)
```

`log1p()` calculates:

```text
log(1 + X)
```

## Best Used When

- Data is right-skewed
- Values are non-negative
- Large observations dominate the feature

## Key Points

- Reduces the influence of very large values
- Changes the distribution of the feature
- Does not remove observations

---

# Power Transformation

## Definition

Power Transformation changes a numerical feature to make its distribution more symmetric and stable.

## Syntax

```python
from sklearn.preprocessing import PowerTransformer

transformer = PowerTransformer()

transformed_data = transformer.fit_transform(
    df[["column"]]
)
```

## Common Methods

Scikit-learn's `PowerTransformer` supports:

- Yeo-Johnson
- Box-Cox

## Key Points

- Helps reduce skewness
- Can make distributions more symmetric
- Yeo-Johnson can work with zero and negative values
- Box-Cox requires strictly positive values

---

# Scaling Method Comparison

| Method | Main Purpose | Outlier Sensitivity |
|---|---|---|
| Min-Max Scaling | Transform values to a fixed range | High |
| Standard Scaling | Center around mean and standard deviation | Moderate |
| Robust Scaling | Scale using median and IQR | Low |
| Log Transformation | Reduce right-skewness | Helps with extreme positive values |
| Power Transformation | Make distribution more symmetric | Depends on data |

---

# When to Use Which Method

## Min-Max Scaling

Use when:

- A fixed range is useful
- Extreme outliers are not a major problem

## Standard Scaling

Use when:

- Features have different scales
- The ML algorithm is sensitive to feature magnitude

## Robust Scaling

Use when:

- Outliers are present
- Median and IQR are more representative than mean and standard deviation

## Log Transformation

Use when:

- Data is positively skewed
- Large positive values dominate the feature

## Power Transformation

Use when:

- The feature distribution is strongly skewed
- A more symmetric distribution is desired

---

# Data Leakage Warning

In a real Machine Learning workflow, scalers should be fitted only on the training data.

Correct workflow:

```text
Train-Test Split
       ↓
Fit Scaler on Training Data
       ↓
Transform Training Data
       ↓
Transform Test Data
```

Do not fit a scaler separately on the test data.

---

# Best Practices

- Preserve the original dataset before cleaning or scaling.
- Understand the meaning of each feature before modifying it.
- Standardize inconsistent text categories.
- Do not remove meaningful special characters blindly.
- Check feature distributions before selecting a scaling method.
- Do not scale categorical labels simply because they are stored as numbers.
- Consider Robust Scaling when outliers are present.
- Fit scalers only on training data in a real ML workflow.
- Compare data before and after transformation.
- Document every preprocessing decision.

---

# Common Mistakes

- Treating every numerical column as a continuous feature
- Scaling binary categorical variables unnecessarily
- Applying the same scaler to every dataset without analysis
- Removing meaningful text characters
- Automatically correcting spelling without validation
- Using Min-Max Scaling without considering outliers
- Fitting preprocessing tools on the complete dataset before evaluation
- Fitting separate scalers on training and testing data

---

# Key Points

- Data consistency creates uniform textual representations.
- Lowercase conversion prevents case-based category duplication.
- Extra spaces and unnecessary symbols can create inconsistent categories.
- Unicode Normalization is useful for text from different sources.
- Numerical scaling handles differences in feature magnitude.
- Min-Max Scaling usually maps values between 0 and 1.
- Standard Scaling uses the mean and standard deviation.
- Robust Scaling uses the median and IQR.
- Log and Power Transformations can reduce skewness.
- The preprocessing method should be selected according to the data and model.

---

# Next Topic

Data Transformation