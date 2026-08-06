# Data Visualization

## Objective

Learn how to visualize data using different plots to understand distributions, relationships, patterns, and trends before building Machine Learning models.

---

## What is Data Visualization?

Data Visualization is the process of representing data using graphs and charts.

Instead of reading large tables, visualizations help us quickly understand the data.

Visualization is an important step of Exploratory Data Analysis (EDA).

---

## Why is Data Visualization Important?

- Understand data easily
- Find hidden patterns
- Detect outliers
- Identify trends
- Compare different variables
- Check feature distributions
- Understand relationships between variables
- Support better business decisions
- Improve Machine Learning preprocessing

---

## Types of Data Visualization

### Histogram

A Histogram shows the distribution of a numerical feature.

It groups data into intervals (bins) and displays how many values fall inside each interval.

### When to Use

- Check data distribution
- Detect skewness
- Detect outliers
- Understand feature spread

---

### Scatter Plot

A Scatter Plot shows the relationship between two numerical variables.

Each point represents one observation.

### When to Use

- Find relationships
- Detect correlation
- Identify clusters
- Detect outliers

---

### Pair Plot

A Pair Plot displays relationships between multiple numerical features at the same time.

It automatically creates scatter plots for feature pairs and histograms on the diagonal.

### When to Use

- Compare multiple features
- Detect correlations
- Understand feature distributions
- Perform quick exploratory analysis

---

## Visualization Libraries

### Matplotlib

- Most popular visualization library
- Highly customizable
- Foundation of many Python plotting libraries

Import:

```python
import matplotlib.pyplot as plt
```

---

### Seaborn

- Built on top of Matplotlib
- Better default styles
- Easier statistical visualizations

Import:

```python
import seaborn as sns
```

---

## Histogram

### Purpose

Shows how data is distributed.

### Syntax

```python
df.hist()
```

### Best For

- Numerical features
- Continuous variables

---

## Scatter Plot

### Purpose

Shows the relationship between two numerical features.

### Syntax

```python
plt.scatter(x, y)
```

### Best For

- Correlation analysis
- Trend analysis

---

## Pair Plot

### Purpose

Shows relationships among multiple numerical features.

### Syntax

```python
sns.pairplot(df)
```

### Best For

- Feature comparison
- Correlation analysis
- Initial EDA

---

## Common Mistakes

- Using Histogram for categorical variables
- Using Scatter Plot for text data
- Creating Pair Plots for very large datasets
- Forgetting axis labels
- Ignoring graph titles

---

## Best Practices

- Always label axes.
- Add meaningful titles.
- Use appropriate figure size.
- Select the correct visualization for the data type.
- Visualize data before preprocessing and model training.

---

## Summary

Data Visualization helps transform raw data into meaningful insights.

The three most important beginner visualizations are:

- Histogram → Distribution
- Scatter Plot → Relationship
- Pair Plot → Multiple feature relationships

These visualizations are commonly used before data preprocessing and Machine Learning model development.

---

