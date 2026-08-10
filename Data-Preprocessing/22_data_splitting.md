# Data Splitting

## Introduction

Data Splitting is the process of dividing a dataset into separate parts so that a Machine Learning model can be trained and evaluated on different data.

The main goal is to check whether the model can perform well on data it has not seen during training.

### Main Goals

- Train the model on one portion of data
- Evaluate the model on unseen data
- Measure model generalization
- Reduce the risk of misleading evaluation
- Prevent data leakage
- Compare models and settings fairly

---

# Features and Target

Before splitting the dataset, data is commonly separated into:

```text
X = Features / Input
y = Target / Output
```

Example:

```text
X:
Age
Salary
Experience

y:
Job_Selected
```

After splitting:

```text
X_train
X_test
y_train
y_test
```

---

# 1. Train/Test Split

## Definition

Train/Test Split divides the dataset into two main parts:

- Training Set
- Test Set

Example:

```text
Complete Dataset
       ↓
 ┌─────┴─────┐
Train Set   Test Set
   80%         20%
```

The exact ratio depends on the dataset and problem.

---

## Training Set

The Training Set is used to train the Machine Learning model.

The model learns patterns and relationships from this data.

```text
X_train + y_train
        ↓
      Model
        ↓
     Learning
```

---

## Test Set

The Test Set contains data that is kept separate from model training.

It is used to evaluate the trained model on unseen examples.

```text
X_test
   ↓
Trained Model
   ↓
Predictions
   ↓
Compare with y_test
```

---

## Syntax

```python
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.20,
    random_state=42
)
```

### Parameters

#### `test_size`

Controls the proportion of data assigned to the test set.

```python
test_size=0.20
```

means approximately:

```text
80% Training
20% Testing
```

#### `random_state`

Controls the randomness of the split.

Using the same value makes the split reproducible.

```python
random_state=42
```

---

# 2. Validation Set

## Definition

A Validation Set is an additional portion of data used during model development.

The dataset can be divided into:

```text
Training Set
Validation Set
Test Set
```

Example:

```text
Complete Dataset
        ↓
Train | Validation | Test
 70%  |    15%     | 15%
```

---

## Purpose of Each Set

### Training Set

Used to train the model.

### Validation Set

Used to:

- Compare models
- Tune hyperparameters
- Make model-development decisions

### Test Set

Used at the end to estimate final performance on unseen data.

---

## Why Keep the Test Set Separate?

If the test set is repeatedly used while adjusting the model, model-development decisions can indirectly adapt to the test data.

Therefore:

```text
Train
  ↓
Learn

Validation
  ↓
Tune / Compare

Test
  ↓
Final Evaluation
```

---

# 3. Stratified Sampling

## Definition

Stratified Sampling attempts to preserve the proportion of target classes when splitting classification data.

Suppose the original target contains:

```text
Class 0 = 90%
Class 1 = 10%
```

A stratified split attempts to maintain similar proportions:

```text
Training Set:
Class 0 ≈ 90%
Class 1 ≈ 10%

Test Set:
Class 0 ≈ 90%
Class 1 ≈ 10%
```

---

## Syntax

```python
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.20,
    random_state=42,
    stratify=y
)
```

The important parameter is:

```python
stratify=y
```

---

## When to Use Stratification

Stratification is especially useful when:

- The problem is classification
- Target classes are imbalanced
- Class proportions should remain similar across splits

---

## Advantages

- Preserves target class distribution
- Helps create more representative splits
- Particularly useful for imbalanced classification datasets

---

# 4. Cross Validation

## Definition

Cross Validation evaluates a model across multiple data splits instead of relying on only one split.

One of the most common methods is:

```text
K-Fold Cross Validation
```

---

# K-Fold Cross Validation

The data is divided into `K` approximately equal parts called folds.

Example:

```text
5-Fold Cross Validation

Fold 1
Fold 2
Fold 3
Fold 4
Fold 5
```

The model is trained and evaluated multiple times.

### Round 1

```text
TEST | TRAIN | TRAIN | TRAIN | TRAIN
```

### Round 2

```text
TRAIN | TEST | TRAIN | TRAIN | TRAIN
```

### Round 3

```text
TRAIN | TRAIN | TEST | TRAIN | TRAIN
```

The process continues until every fold has been used for validation once.

---

## Example Scores

```text
Fold 1 → 85%
Fold 2 → 82%
Fold 3 → 88%
Fold 4 → 84%
Fold 5 → 86%
```

The scores can then be summarized using their average and variation.

---

## Syntax

```python
from sklearn.model_selection import cross_val_score

scores = cross_val_score(
    model,
    X,
    y,
    cv=5
)

print(scores)
print(scores.mean())
```

Here:

```python
cv=5
```

means 5-fold Cross Validation.

---

# Stratified K-Fold

For classification problems, folds can also preserve target class proportions.

```python
from sklearn.model_selection import StratifiedKFold

skf = StratifiedKFold(
    n_splits=5,
    shuffle=True,
    random_state=42
)
```

This is particularly useful when classes are imbalanced.

---

# Train/Test Split vs Cross Validation

| Train/Test Split | Cross Validation |
|---|---|
| Uses one split | Uses multiple splits |
| Faster | More computationally expensive |
| Simple | More robust evaluation |
| Result can depend more on one split | Evaluates across several folds |

---

# Train vs Validation vs Test

| Dataset | Purpose |
|---|---|
| Training Set | Learn model parameters |
| Validation Set | Tune and compare models |
| Test Set | Final evaluation |

A simple way to remember:

```text
Train      → Learn
Validation → Improve / Choose
Test       → Final Check
```

---

# Data Leakage

## Definition

Data Leakage happens when information that should not be available during training influences the model-training process.

This can produce unrealistically good evaluation results.

---

# Preprocessing Before Splitting

Incorrect workflow:

```text
Complete Dataset
      ↓
Fit Scaler / PCA / Feature Selection
      ↓
Train-Test Split
```

Information from the future test set may influence the learned preprocessing.

---

# Correct Workflow

```text
Raw Dataset
      ↓
Train-Test Split
      ↓
Fit Preprocessing on Training Data
      ↓
Transform Training Data
      ↓
Use Same Learned Transformation on Test Data
      ↓
Train Model
      ↓
Evaluate Model
```

---

## Scaling Example

Correct:

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

X_train_scaled = scaler.fit_transform(
    X_train
)

X_test_scaled = scaler.transform(
    X_test
)
```

Notice:

```text
Training Data → fit_transform()
Test Data     → transform()
```

The test set is not used to fit the scaler.

---

# Why `fit_transform()` on Train?

`fit_transform()` performs two operations:

```text
fit
↓
Learn required information from training data

transform
↓
Apply the learned transformation
```

Example with StandardScaler:

```text
Training Data
      ↓
Learn Mean + Standard Deviation
      ↓
Scale Training Data
```

---

# Why Only `transform()` on Test?

The test data should not teach the preprocessing method anything new.

Therefore:

```python
scaler.transform(X_test)
```

uses the mean and standard deviation already learned from the training set.

This helps keep the test data truly unseen.

---

# Random State

Many splitting operations involve randomness.

Example:

```python
random_state=42
```

Using a fixed random state makes results reproducible.

Running the code again with the same dataset and settings produces the same split.

The number `42` is not special for Machine Learning; another fixed integer can also be used.

---

# Choosing Split Ratios

Common examples include:

```text
80% Train / 20% Test

70% Train / 30% Test

70% Train / 15% Validation / 15% Test
```

There is no single ratio that is correct for every dataset.

The choice depends on:

- Dataset size
- Problem type
- Amount of training data required
- Evaluation requirements

---

# Data Splitting Workflow

A typical supervised Machine Learning workflow is:

```text
Dataset
   ↓
Separate X and y
   ↓
Train-Test Split
   ↓
Fit Preprocessing on Training Data
   ↓
Transform Train and Test
   ↓
Train Model on Training Data
   ↓
Generate Predictions
   ↓
Evaluate Using Test Data
```

When model tuning is required:

```text
Training Data
     ↓
Validation / Cross Validation
     ↓
Choose Model and Hyperparameters
     ↓
Final Test Evaluation
```

---

# Best Practices

- Separate features and target clearly.
- Keep test data separate from training.
- Use a reproducible `random_state` during experiments.
- Consider stratification for classification problems.
- Use validation data or Cross Validation for model selection.
- Keep the final test set for final evaluation.
- Fit preprocessing techniques only on training data.
- Apply the same learned transformations to validation and test data.
- Choose split ratios according to dataset size and problem requirements.
- Check class distributions after splitting classification datasets.

---

# Common Mistakes

- Training and testing on the same data
- Looking repeatedly at test performance while tuning the model
- Fitting preprocessing on the complete dataset before splitting
- Using `fit_transform()` separately on the test set
- Ignoring severe class imbalance during splitting
- Assuming 80/20 is mandatory for every dataset
- Forgetting to separate features and target
- Treating Cross Validation as a replacement for a final independent test set in every workflow
- Using inconsistent preprocessing between training and test data

---

# Key Points

- Data Splitting separates model learning from model evaluation.
- `X` represents input features.
- `y` represents the target.
- Training data is used to teach the model.
- Validation data helps tune and compare models.
- Test data is used for final evaluation.
- Stratified Sampling preserves target class proportions.
- Cross Validation evaluates performance across multiple folds.
- Preprocessing should be fitted using training data only.
- Test data should remain unseen during model development.
- A fixed `random_state` makes experiments reproducible.

---

# Quick Recap

```text
Train
= Model ki study material

Validation
= Mock test

Test
= Final exam

Stratify
= Har split mein classes ka similar ratio

Cross Validation
= Multiple mock tests

Data Leakage
= Final exam ki information pehle hi model tak pohanch jana
```

---

# Next Topic

Advanced Preprocessing