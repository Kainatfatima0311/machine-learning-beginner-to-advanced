# Model Selection Basics

## Introduction

Before selecting a Machine Learning algorithm, we first need to understand what type of problem we are solving.

The two main problem types covered in this section are:

- Classification
- Regression

Classification can further be divided into:

- Binary Classification
- Multiclass Classification

---

# 1. Classification

## Definition

Classification is a Supervised Machine Learning problem in which the target variable represents a category or class.

The model learns from labeled training data and predicts which class a new observation belongs to.

### Basic Flow

```text
Input Features
      ↓
Classification Model
      ↓
Predicted Class
```

### Examples

```text
Email
↓
Spam / Not Spam
```

```text
Student Information
↓
Pass / Fail
```

```text
Image
↓
Cat / Dog / Bird
```

---

## Classification Target

In Classification, the target represents categories.

Examples:

```text
Yes / No

Pass / Fail

Spam / Not Spam

Cat / Dog / Bird
```

Categories may also be represented using numbers.

Example:

```text
0 = No
1 = Yes
```

Even though `0` and `1` are numbers, they represent classes rather than continuous numerical quantities.

---

# 2. Binary Classification

## Definition

Binary Classification is a Classification problem containing exactly two possible target classes.

### Examples

```text
Yes / No
```

```text
0 / 1
```

```text
Pass / Fail
```

```text
Spam / Not Spam
```

```text
Disease / No Disease
```

---

## Practical Example

Suppose a dataset contains:

```text
Age
Income
Credit Score
Loan Approved
```

The input features are:

```text
Age
Income
Credit Score
```

The target is:

```text
Loan Approved

0 = No
1 = Yes
```

Because there are only two possible target classes, this is a Binary Classification problem.

---

## Common Binary Classification Problems

- Customer Churn Prediction
- Fraud Detection
- Spam Detection
- Loan Approval Prediction
- Pass/Fail Prediction
- Disease/No Disease Prediction

---

# 3. Multiclass Classification

## Definition

Multiclass Classification is a Classification problem where the target contains more than two possible classes.

### Example

```text
Apple
Banana
Orange
```

There are three possible classes.

Therefore, this is a Multiclass Classification problem.

---

## Practical Example

Suppose an image classification system predicts:

```text
Input Image
     ↓
Classification Model
     ↓
Cat / Dog / Bird
```

The model must choose one class from multiple possible categories.

---

## More Examples

### Student Grade Prediction

```text
A
B
C
D
F
```

### Product Category Prediction

```text
Electronics
Clothing
Furniture
Books
```

### Fruit Classification

```text
Apple
Banana
Orange
Mango
```

---

# Binary vs Multiclass Classification

| Binary Classification | Multiclass Classification |
|---|---|
| Exactly two classes | More than two classes |
| Yes / No | Cat / Dog / Bird |
| Pass / Fail | Grade A / B / C / D |
| Spam / Not Spam | Multiple email categories |

### Quick Rule

```text
2 Classes
→ Binary Classification

More Than 2 Classes
→ Multiclass Classification
```

---

# 4. Regression

## Definition

Regression is a Supervised Machine Learning problem in which the target is a continuous numerical value.

Instead of predicting a category, the model predicts a numerical quantity.

### Basic Flow

```text
Input Features
      ↓
Regression Model
      ↓
Numerical Prediction
```

---

## Practical Example

Suppose we want to predict house prices.

Input features:

```text
House Area
Bedrooms
Location Score
```

Target:

```text
House Price
```

Example prediction:

```text
Area = 1800 sq ft
Bedrooms = 3
Location Score = 8

        ↓

Predicted Price = 8,500,000
```

Because the output is a numerical value, this is a Regression problem.

---

## Common Regression Problems

- House Price Prediction
- Salary Prediction
- Temperature Prediction
- Sales Prediction
- Revenue Prediction
- Rent Prediction

---

# Classification vs Regression

## Classification

Classification predicts a category or class.

```text
Input
 ↓
Model
 ↓
Class
```

Examples:

```text
Spam / Not Spam

Yes / No

Cat / Dog / Bird
```

---

## Regression

Regression predicts a continuous numerical value.

```text
Input
 ↓
Model
 ↓
Numerical Value
```

Examples:

```text
House Price = 8,500,000

Temperature = 32.5°C

Salary = 75,000
```

---

# Comparison

| Classification | Regression |
|---|---|
| Predicts classes | Predicts continuous numerical values |
| Output is categorical | Output is numerical |
| Spam / Not Spam | House Price |
| Pass / Fail | Salary |
| Cat / Dog / Bird | Temperature |

---

# How to Identify the Problem Type

Before selecting a model, inspect the target variable.

## Ask:

```text
What am I trying to predict?
```

### If the answer is a category:

```text
Which class?
```

Use:

```text
Classification
```

### If the answer is a continuous numerical quantity:

```text
How much?
```

Use:

```text
Regression
```

---

# Example 1 — Customer Churn

Question:

```text
Will the customer leave?
```

Target:

```text
Yes / No
```

Problem:

```text
Binary Classification
```

---

# Example 2 — Fruit Prediction

Question:

```text
Which fruit is this?
```

Target:

```text
Apple / Banana / Orange
```

Problem:

```text
Multiclass Classification
```

---

# Example 3 — House Price

Question:

```text
How much will the house cost?
```

Target:

```text
Numerical Price
```

Problem:

```text
Regression
```

---

# Example 4 — Temperature Prediction

Question:

```text
What will tomorrow's temperature be?
```

Target:

```text
32.5°C
```

Problem:

```text
Regression
```

---

# Model Selection

Once the problem type is identified, suitable Machine Learning algorithms can be considered.

## Classification Algorithms

Examples include:

- Logistic Regression
- Decision Tree Classifier
- Random Forest Classifier

## Regression Algorithms

Examples include:

- Linear Regression
- Ridge Regression
- Lasso Regression
- Decision Tree Regressor

The algorithms will be studied in detail in later sections.

---

# Important Note About Logistic Regression

Despite having the word **Regression** in its name, Logistic Regression is primarily used for Classification.

For example:

```text
Customer Data
     ↓
Logistic Regression
     ↓
Churn / No Churn
```

This algorithm will be studied in detail later.

---

# Model Selection Workflow

A simple workflow is:

```text
Define Problem
      ↓
Identify Target
      ↓
Check Target Type
      ↓
 ┌────┴────┐
Category   Continuous Number
   ↓              ↓
Classification  Regression
   ↓
If Classification:
Check Number of Classes
   ↓
2 Classes → Binary

More Than 2 → Multiclass
```

---

# Best Practices

- Clearly define the prediction target before selecting a model.
- Check whether the target represents categories or continuous values.
- Do not decide the problem type only from the target's data type.
- Remember that numerical labels such as `0` and `1` can represent categories.
- Distinguish Binary Classification from Multiclass Classification.
- Select algorithms according to the problem type.
- Consider the dataset and evaluation requirements before choosing a final model.
- Compare suitable models instead of assuming one algorithm will always perform best.

---

# Common Mistakes

- Using a Regression model for a Classification problem
- Using a Classification model for a continuous numerical target
- Assuming every numerical target represents Regression
- Forgetting that `0` and `1` may represent classes
- Confusing Binary and Multiclass Classification
- Selecting an algorithm before understanding the target
- Assuming Logistic Regression is a Regression algorithm because of its name

---

# Key Points

- Model Selection starts by understanding the Machine Learning problem.
- Classification predicts categories or classes.
- Binary Classification contains exactly two classes.
- Multiclass Classification contains more than two classes.
- Regression predicts continuous numerical values.
- The target variable helps determine the problem type.
- Numerical labels can still represent categorical classes.
- Problem type should be identified before selecting an algorithm.

---

# Quick Recap

```text
Classification
= Which class?

Binary Classification
= Exactly 2 classes

Multiclass Classification
= More than 2 classes

Regression
= How much?

Example:

Churn Yes/No
→ Binary Classification

Cat/Dog/Bird
→ Multiclass Classification

House Price
→ Regression
```

---

# Next Part

Model Evaluation