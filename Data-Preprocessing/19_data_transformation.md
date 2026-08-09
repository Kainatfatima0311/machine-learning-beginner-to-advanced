# Data Transformation

## Introduction

Data Transformation converts existing data into a representation that is more suitable for Machine Learning models.

It mainly includes:

- Encoding Techniques
- Numerical Transformation
- Feature Engineering

---

# 1. Encoding Techniques

Encoding converts categorical values into numerical representations that Machine Learning algorithms can process.

---

## One-Hot Encoding

### Definition

One-Hot Encoding creates a separate binary column for each category.

### Example

Original:

```text
City
Lahore
Karachi
Islamabad
```

Encoded:

```text
           Lahore  Karachi  Islamabad
Lahore       1       0          0
Karachi      0       1          0
Islamabad    0       0          1
```

### Syntax

```python
encoded_data = pd.get_dummies(
    df,
    columns=["city"]
)
```

### Best Used When

- Categories have no natural order
- The number of categories is manageable

### Advantages

- Does not introduce an artificial order
- Easy for ML models to interpret

### Limitations

- Can create many new columns
- May be inefficient for high-cardinality features

---

## Label Encoding

### Definition

Label Encoding assigns a numerical value to each category.

### Example

```text
Female → 0
Male   → 1
```

### Syntax

```python
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()

df["encoded"] = encoder.fit_transform(
    df["category"]
)
```

### Best Used When

- The target variable contains categories
- Categories have a meaningful order in suitable cases
- Binary categories need a simple numerical representation

### Limitation

For unordered categories, numerical labels may introduce an artificial relationship between categories.

---

## Target Encoding

### Definition

Target Encoding replaces each category with a value calculated from the target variable, commonly the target mean for that category.

### Concept

```text
Category
   ↓
Target values for that category
   ↓
Average target value
   ↓
Encoded category
```

### Example

```python
target_means = df.groupby(
    "category"
)["target"].mean()

df["category_encoded"] = (
    df["category"].map(target_means)
)
```

### Advantages

- Does not create many new columns
- Can capture relationships between categories and the target

### Limitations

- High risk of data leakage
- Can overfit
- Must be handled carefully using training data

---

## Frequency Encoding

### Definition

Frequency Encoding replaces a category with its occurrence count or frequency.

### Example

```text
Lahore     → 100
Karachi    → 70
Islamabad  → 30
```

### Syntax

```python
frequency = df["city"].value_counts()

df["city_frequency"] = (
    df["city"].map(frequency)
)
```

### Advantages

- Simple
- Does not create many additional columns
- Useful for high-cardinality categorical features

### Limitations

Different categories can have the same frequency and therefore receive the same encoded value.

---

# 2. Numerical Transformation

Numerical Transformation changes the scale or distribution of numerical features.

---

## Standardization

### Definition

Standardization transforms numerical values using their mean and standard deviation.

### Formula

```text
Z = (X - Mean) / Standard Deviation
```

### Syntax

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

scaled_data = scaler.fit_transform(
    df[["column"]]
)
```

### Key Point

The transformed feature generally has a mean close to `0` and standard deviation close to `1`.

---

## Min-Max Scaling

### Definition

Min-Max Scaling usually transforms values into a range between `0` and `1`.

### Formula

```text
X_scaled = (X - X_min) / (X_max - X_min)
```

### Syntax

```python
from sklearn.preprocessing import MinMaxScaler

scaler = MinMaxScaler()

scaled_data = scaler.fit_transform(
    df[["column"]]
)
```

### Limitation

Min-Max Scaling is sensitive to outliers.

---

## Robust Scaling

### Definition

Robust Scaling uses the median and Interquartile Range instead of the mean and standard deviation.

### Syntax

```python
from sklearn.preprocessing import RobustScaler

scaler = RobustScaler()

scaled_data = scaler.fit_transform(
    df[["column"]]
)
```

### Key Point

It is more resistant to extreme values than Min-Max and Standard Scaling.

---

## Log Transformation

### Definition

Log Transformation compresses large positive values and can reduce right-skewness.

### Syntax

```python
df["log_column"] = np.log1p(
    df["column"]
)
```

### Key Points

- Useful for right-skewed features
- Reduces the influence of large values
- `log1p()` calculates `log(1 + X)`
- Does not remove observations

---

## Square Root Transformation

### Definition

Square Root Transformation applies the square root function to numerical values.

### Syntax

```python
df["sqrt_column"] = np.sqrt(
    df["column"]
)
```

### Best Used When

- Values are non-negative
- The distribution is moderately right-skewed
- A milder transformation than logarithmic transformation is required

### Key Point

Square Root Transformation compresses large values but generally less aggressively than Log Transformation.

---

## Polynomial Transformation

### Definition

Polynomial Transformation creates higher-order versions of numerical features.

### Example

For feature `X`:

```text
X
X²
X³
```

### Syntax

```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures(
    degree=2
)

polynomial_data = poly.fit_transform(
    df[["feature"]]
)
```

### Best Used When

The relationship between features and the target may be nonlinear.

### Limitation

Higher polynomial degrees can create many features and increase the risk of overfitting.

---

# 3. Feature Engineering

## Definition

Feature Engineering creates new useful features from existing data.

The objective is to provide Machine Learning models with better representations of the available information.

---

## Polynomial Features

### Definition

Polynomial Features create powers and combinations of existing numerical features.

### Example

From:

```text
Age
```

we can create:

```text
Age²
```

With multiple features, Polynomial Features can also create interaction terms.

### Syntax

```python
from sklearn.preprocessing import PolynomialFeatures

poly = PolynomialFeatures(
    degree=2,
    include_bias=False
)

new_features = poly.fit_transform(
    df[["feature1", "feature2"]]
)
```

### Limitation

The number of features can increase rapidly.

---

## Binning

### Definition

Binning converts continuous numerical values into groups or intervals.

### Example

```text
18–30 → Young
31–50 → Middle
51+   → Senior
```

### Syntax

```python
df["age_group"] = pd.cut(
    df["age"],
    bins=[0, 30, 50, float("inf")],
    labels=["Young", "Middle", "Senior"]
)
```

### Advantages

- Simplifies continuous variables
- Can create meaningful groups
- Can help capture nonlinear patterns

### Limitation

Some numerical information is lost when continuous values are converted into groups.

---

## Interaction Features

### Definition

Interaction Features combine two or more features to represent their joint effect.

### Example

```text
Age × Serum Creatinine
```

### Syntax

```python
df["age_creatinine_interaction"] = (
    df["age"]
    * df["serum_creatinine"]
)
```

### Key Point

An interaction can contain useful information even when individual features alone do not fully represent their combined effect.

---

## Date Feature Extraction

### Definition

Date Feature Extraction creates useful numerical or categorical features from date and time values.

### Example

From:

```text
2026-08-09
```

we can extract:

```text
Year  → 2026
Month → 8
Day   → 9
```

### Syntax

```python
df["date"] = pd.to_datetime(
    df["date"]
)

df["year"] = df["date"].dt.year
df["month"] = df["date"].dt.month
df["day"] = df["date"].dt.day
```

### Other Possible Features

- Day of week
- Quarter
- Weekend indicator
- Hour
- Time since an event

---

## Aggregation Features

### Definition

Aggregation Features summarize multiple observations or values into useful information.

### Examples

- Total purchases per customer
- Average transaction value
- Maximum transaction
- Number of orders

### Syntax

```python
customer_average = df.groupby(
    "customer_id"
)["amount"].mean()
```

### Key Point

Aggregation is especially useful when multiple records belong to the same entity.

---

# Encoding Method Comparison

| Method | Best For | Main Limitation |
|---|---|---|
| One-Hot Encoding | Unordered categories | Creates many columns |
| Label Encoding | Binary/ordered categories or targets | Can introduce artificial order |
| Target Encoding | High-cardinality features | Data leakage risk |
| Frequency Encoding | High-cardinality features | Categories may share frequencies |

---

# Numerical Transformation Comparison

| Method | Main Purpose |
|---|---|
| Standardization | Standardize feature scale |
| Min-Max Scaling | Transform to a fixed range |
| Robust Scaling | Scale data with less outlier sensitivity |
| Log Transformation | Reduce right-skewness |
| Square Root Transformation | Mildly reduce right-skewness |
| Polynomial Transformation | Represent nonlinear relationships |

---

# Data Leakage

Some transformations learn information from the dataset.

Examples include:

- StandardScaler
- MinMaxScaler
- RobustScaler
- Target Encoding
- Polynomial preprocessing in a modeling pipeline

In a real Machine Learning workflow, transformations that learn parameters should be fitted using training data only.

Correct workflow:

```text
Train-Test Split
       ↓
Fit Transformation on Training Data
       ↓
Transform Training Data
       ↓
Transform Test Data
```

Target Encoding requires additional care because it directly uses the target variable.

---

# Best Practices

- Preserve the original dataset.
- Understand the meaning of a feature before transforming it.
- Choose encoding according to the category type.
- Avoid unnecessary One-Hot Encoding for very high-cardinality features.
- Avoid introducing artificial order with Label Encoding.
- Prevent data leakage during Target Encoding.
- Analyze numerical distributions before applying transformations.
- Avoid unnecessary polynomial degrees.
- Create features that have logical or domain meaning.
- Validate whether engineered features improve model performance.
- Fit learned transformations only on training data.

---

# Common Mistakes

- Encoding categories without understanding their meaning
- Applying Label Encoding to unordered features without considering artificial ordering
- Performing Target Encoding on the complete dataset
- Scaling binary categorical labels unnecessarily
- Applying Log Transformation to unsuitable values
- Creating excessive polynomial features
- Creating arbitrary bins without justification
- Adding engineered features without checking whether they provide useful information
- Fitting transformations before train-test splitting

---

# Key Points

- Encoding converts categorical data into numerical representations.
- One-Hot Encoding creates separate binary columns.
- Label Encoding assigns numerical labels.
- Target Encoding uses information from the target variable.
- Frequency Encoding uses category occurrence frequency.
- Numerical transformations change feature scale or distribution.
- Square Root and Log Transformations can reduce right-skewness.
- Polynomial transformations can represent nonlinear relationships.
- Feature Engineering creates useful information from existing features.
- Binning, interactions, date extraction, and aggregation are common Feature Engineering techniques.
- Data leakage must be prevented during preprocessing.

---

# Next Topic

Feature Selection