# Feature Selection

## Introduction

Feature Selection is the process of selecting the most useful features from a dataset while removing irrelevant, redundant, or noisy features.

The main goals of Feature Selection are:

- Improve model performance
- Reduce overfitting
- Decrease training time
- Reduce noise
- Improve model interpretability
- Simplify the Machine Learning pipeline

Feature Selection methods are commonly divided into:

- Filter Methods
- Wrapper Methods
- Embedded Methods

---

# 1. Filter Methods

Filter Methods select features using statistical techniques before training the final Machine Learning model.

They are generally fast and computationally efficient.

---

## Correlation Analysis

### Definition

Correlation Analysis measures the strength and direction of a linear relationship between numerical variables.

Correlation values usually range between:

```text
-1 and +1
```

### Interpretation

```text
+1 → Strong positive linear relationship
 0 → Little or no linear relationship
-1 → Strong negative linear relationship
```

### Syntax

```python
correlation_matrix = df.corr(
    numeric_only=True
)
```

To check correlation with a target:

```python
target_correlation = (
    correlation_matrix["target"]
    .sort_values(
        ascending=False
    )
)
```

### Best Used When

- Features are numerical
- The target is numerical or numerically represented
- Linear relationships are being analyzed

### Advantages

- Fast
- Easy to understand
- Helps identify highly related features
- Can detect redundant numerical features

### Limitations

- Measures mainly linear relationships
- High correlation does not imply causation
- A feature with low correlation may still contain nonlinear predictive information

---

## Chi-Square Test

### Definition

The Chi-Square Test measures whether two categorical variables are statistically associated.

It can be used to evaluate categorical features against a categorical target.

### Syntax

```python
from sklearn.feature_selection import chi2

chi_scores, p_values = chi2(
    X,
    y
)
```

### Interpretation

A smaller p-value generally provides stronger evidence that the feature and target are associated.

### Best Used When

- Features are categorical or non-negative encoded values
- The target is categorical
- Category relationships need to be tested

### Advantages

- Simple statistical method
- Useful for categorical feature selection
- Computationally efficient

### Limitations

- Requires non-negative feature values in Scikit-learn's implementation
- Does not measure the strength of every possible relationship
- Results depend on sample size

---

## ANOVA

### Definition

ANOVA evaluates whether numerical feature values differ significantly across target classes.

Scikit-learn commonly provides the ANOVA F-test through `f_classif`.

### Syntax

```python
from sklearn.feature_selection import f_classif

f_scores, p_values = f_classif(
    X,
    y
)
```

### Best Used When

- Features are numerical
- The target is categorical
- Differences between group means need to be evaluated

### Advantages

- Fast
- Useful for classification feature selection
- Easy to rank numerical features

### Limitations

- Focuses on group-level statistical differences
- Does not capture every nonlinear relationship
- Statistical assumptions should be considered

---

# 2. Wrapper Methods

Wrapper Methods use a Machine Learning model to evaluate different feature subsets.

They are usually more computationally expensive than Filter Methods.

---

## Recursive Feature Elimination (RFE)

### Definition

Recursive Feature Elimination repeatedly trains a model and removes the least important feature until the desired number of features remains.

### Workflow

```text
Start With All Features
        ↓
Train Model
        ↓
Rank Features
        ↓
Remove Least Important Feature
        ↓
Train Again
        ↓
Repeat
```

### Syntax

```python
from sklearn.feature_selection import RFE
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(
    max_iter=1000
)

selector = RFE(
    estimator=model,
    n_features_to_select=3
)

selector.fit(
    X,
    y
)
```

### Advantages

- Uses model performance and importance
- Can identify a compact subset of features
- Useful when the number of features is manageable

### Limitations

- Computationally expensive
- Results depend on the selected estimator
- Can become slow with large feature sets

---

## Forward Selection

### Definition

Forward Selection starts with no features and adds the most useful feature at each step.

### Workflow

```text
No Features
    ↓
Add Best Feature
    ↓
Evaluate
    ↓
Add Next Best Feature
    ↓
Repeat
```

### Syntax

```python
from sklearn.feature_selection import SequentialFeatureSelector
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(
    max_iter=1000
)

forward_selector = SequentialFeatureSelector(
    model,
    n_features_to_select=3,
    direction="forward"
)

forward_selector.fit(
    X,
    y
)
```

### Advantages

- Builds a compact subset gradually
- Useful when only a few features are needed
- Easier to control than testing every possible subset

### Limitations

- Computationally expensive
- Early feature choices may affect later selections
- Results depend on the estimator and evaluation strategy

---

## Backward Selection

### Definition

Backward Selection starts with all features and removes the least useful features one by one.

### Workflow

```text
All Features
    ↓
Remove Least Useful Feature
    ↓
Evaluate
    ↓
Remove Next Feature
    ↓
Repeat
```

### Syntax

```python
backward_selector = SequentialFeatureSelector(
    model,
    n_features_to_select=3,
    direction="backward"
)

backward_selector.fit(
    X,
    y
)
```

### Advantages

- Starts with the complete feature set
- Can identify redundant features
- Useful when most original features may contain information

### Limitations

- More expensive when the dataset contains many features
- Depends on the estimator
- Removing a feature early can affect future results

---

# 3. Embedded Methods

Embedded Methods perform feature selection during model training.

They combine characteristics of Filter and Wrapper approaches.

---

## Lasso

### Definition

Lasso uses L1 Regularization.

L1 Regularization can shrink some coefficients exactly to zero.

Features with zero coefficients can be treated as unselected by that fitted model.

### Syntax

For regression:

```python
from sklearn.linear_model import Lasso

lasso = Lasso(
    alpha=0.01
)

lasso.fit(
    X,
    y
)
```

For classification, L1-regularized Logistic Regression can also be used:

```python
from sklearn.linear_model import LogisticRegression

model = LogisticRegression(
    penalty="l1",
    solver="liblinear"
)
```

### Advantages

- Performs regularization and feature selection together
- Can create sparse models
- Reduces the influence of irrelevant features

### Limitations

- Results depend on the regularization strength
- Correlated features can make selection unstable
- Features should often be scaled before regularized linear models

---

## Ridge

### Definition

Ridge uses L2 Regularization.

It shrinks feature coefficients toward zero but usually does not make them exactly zero.

### Syntax

```python
from sklearn.linear_model import Ridge

ridge = Ridge(
    alpha=1.0
)

ridge.fit(
    X,
    y
)
```

### Key Point

Ridge is mainly a regularization technique rather than a strict feature-selection method.

Its coefficients can still help analyze feature influence.

### Advantages

- Reduces large coefficients
- Helps control overfitting
- Useful when features are correlated

### Limitations

- Usually keeps all features
- Does not naturally produce a sparse feature subset
- Coefficient magnitude can depend on feature scale

---

## Random Forest Feature Importance

### Definition

Random Forest can estimate how useful features are for prediction based on the fitted trees.

### Syntax

```python
from sklearn.ensemble import RandomForestClassifier

model = RandomForestClassifier(
    random_state=42
)

model.fit(
    X,
    y
)

importance = model.feature_importances_
```

### Example

```python
feature_importance = pd.Series(
    model.feature_importances_,
    index=X.columns
).sort_values(
    ascending=False
)
```

### Advantages

- Works with nonlinear relationships
- Can capture feature interactions
- Easy to rank features
- Does not require feature scaling

### Limitations

- Importance does not prove causality
- Importance values can be biased in some feature settings
- Correlated features may share or redistribute importance

---

# Feature Selection Method Comparison

| Method | Category | Main Idea | Speed |
|---|---|---|---|
| Correlation | Filter | Linear relationships | Fast |
| Chi-Square | Filter | Categorical association | Fast |
| ANOVA | Filter | Numerical differences between classes | Fast |
| RFE | Wrapper | Repeated model-based elimination | Slower |
| Forward Selection | Wrapper | Add features gradually | Slower |
| Backward Selection | Wrapper | Remove features gradually | Slower |
| Lasso | Embedded | L1 coefficients can become zero | Moderate |
| Ridge | Embedded / Regularization | Shrinks coefficients | Moderate |
| Random Forest Importance | Embedded / Model-Based | Tree-based importance | Moderate |

---

# Filter vs Wrapper vs Embedded Methods

## Filter Methods

```text
Statistical Test
      ↓
Feature Ranking
```

### Characteristics

- Fast
- Model-independent
- Suitable for initial screening

---

## Wrapper Methods

```text
Feature Subset
      ↓
Train Model
      ↓
Evaluate
      ↓
Choose Better Subset
```

### Characteristics

- Model-dependent
- More computationally expensive
- Can capture feature combinations

---

## Embedded Methods

```text
Train Model
      ↓
Feature Selection / Importance
```

### Characteristics

- Feature selection happens during training
- Often more efficient than Wrapper Methods
- Depends on the chosen algorithm

---

# Feature Selection vs Feature Extraction

Feature Selection keeps some original features.

Example:

```text
Original:
Age
Salary
Height
Weight

Selected:
Age
Salary
```

Feature Extraction creates new features from the original features.

Example:

```text
PCA Components
```

Dimensionality Reduction will be covered separately.

---

# Data Leakage

Feature Selection must be handled carefully in a Machine Learning workflow.

Incorrect:

```text
Use Entire Dataset
       ↓
Select Features
       ↓
Train-Test Split
```

This can expose information from the test data.

Recommended workflow:

```text
Train-Test Split
       ↓
Fit Feature Selection on Training Data
       ↓
Transform Training Data
       ↓
Apply Same Selection to Test Data
```

---

# Best Practices

- Understand the target before selecting features.
- Remove obvious irrelevant identifiers when justified.
- Analyze duplicated or strongly redundant information.
- Use Filter Methods for quick initial screening.
- Use Wrapper Methods when computational resources allow.
- Use Embedded Methods when the model naturally supports them.
- Do not select features using test-data information.
- Scale features when required by the selected model.
- Validate feature subsets using model performance.
- Use domain knowledge alongside statistical methods.
- Document why a feature was selected or removed.

---

# Common Mistakes

- Selecting features before defining the target
- Assuming correlation implies causation
- Removing all low-correlation features automatically
- Applying Chi-Square to unsuitable negative values
- Using Wrapper Methods on huge feature sets without considering cost
- Treating Ridge as if it automatically removes features
- Interpreting Random Forest importance as causal importance
- Performing Feature Selection on the complete dataset before splitting
- Removing medically or business-important features only because of a statistical score

---

# Key Points

- Feature Selection keeps the most useful original features.
- Filter Methods use statistical tests.
- Correlation measures linear relationships.
- Chi-Square is useful for categorical relationships.
- ANOVA can rank numerical features for categorical targets.
- RFE repeatedly removes weak features.
- Forward Selection adds useful features.
- Backward Selection removes weak features.
- Lasso can shrink coefficients to zero.
- Ridge shrinks coefficients but usually keeps all features.
- Random Forest provides model-based feature importance.
- Feature Selection should be fitted using training data only.
- Statistical results should be combined with domain knowledge.

---

# Next Topic

Dimensionality Reduction