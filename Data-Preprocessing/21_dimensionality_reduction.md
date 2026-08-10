# Dimensionality Reduction

## Introduction

Dimensionality Reduction is the process of reducing the number of features or dimensions in a dataset while preserving as much useful information as possible.

It is especially useful when a dataset contains a large number of features.

### Main Goals

- Reduce the number of dimensions
- Reduce computational complexity
- Speed up model training
- Reduce noise and redundancy
- Help control overfitting
- Make high-dimensional data easier to visualize

---

# Feature Selection vs Dimensionality Reduction

## Feature Selection

Feature Selection chooses useful features from the original dataset.

Example:

```text
Original Features:
Age
Salary
Height
Weight

Selected Features:
Age
Salary
```

The selected features remain original features.

## Dimensionality Reduction

Dimensionality Reduction can transform multiple original features into a smaller set of new features.

Example:

```text
Age
Salary
Height
Weight

        ↓ PCA

PC1
PC2
```

### Key Difference

```text
Feature Selection
= Select original features

Dimensionality Reduction
= Create a lower-dimensional representation
```

---

# 1. Principal Component Analysis (PCA)

## Definition

Principal Component Analysis is an unsupervised dimensionality reduction technique that transforms original features into new features called Principal Components.

The first Principal Components are designed to preserve as much variation in the data as possible.

### Example

```text
Feature 1 ─┐
Feature 2 ─┼──→ PCA ──→ PC1
Feature 3 ─┘            PC2
```

`PC1` and `PC2` are new features created from combinations of the original features.

## Syntax

```python
from sklearn.decomposition import PCA

pca = PCA(
    n_components=2
)

X_pca = pca.fit_transform(X)
```

## Explained Variance

PCA provides information about how much variation each Principal Component explains.

```python
pca.explained_variance_ratio_
```

Example:

```text
PC1 → 60%
PC2 → 25%
```

Together:

```text
85% explained variance
```

## Advantages

- Reduces the number of dimensions
- Can remove redundant information
- Speeds up some ML workflows
- Useful for visualization
- Does not require a target variable

## Limitations

- Principal Components are harder to interpret than original features
- Some information may be lost
- PCA is sensitive to feature scale
- Numerical features should generally be scaled before PCA

---

# 2. Linear Discriminant Analysis (LDA)

## Definition

Linear Discriminant Analysis is a supervised dimensionality reduction technique.

It creates new dimensions that attempt to separate target classes effectively.

### Example

Suppose the target contains:

```text
Class A
Class B
```

LDA tries to create a representation where the two classes are easier to distinguish.

## Syntax

```python
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis

lda = LinearDiscriminantAnalysis(
    n_components=1
)

X_lda = lda.fit_transform(
    X,
    y
)
```

## Key Point

Unlike PCA, LDA requires a target variable.

```text
PCA → Uses X

LDA → Uses X + y
```

## Advantages

- Uses class information
- Can improve class separation
- Useful for classification problems
- Can reduce dimensionality

## Limitations

- Requires labeled data
- Mainly suitable for classification
- The number of possible components is limited by the number of classes

---

# PCA vs LDA

| PCA | LDA |
|---|---|
| Unsupervised | Supervised |
| Does not require target | Requires target |
| Preserves maximum variance | Maximizes class separation |
| General dimensionality reduction | Classification-oriented reduction |

---

# 3. t-SNE

## Definition

t-SNE stands for t-Distributed Stochastic Neighbor Embedding.

It is a nonlinear dimensionality reduction technique commonly used to visualize high-dimensional data in two or three dimensions.

### Concept

```text
High-Dimensional Data
        ↓
      t-SNE
        ↓
2D or 3D Representation
```

## Syntax

```python
from sklearn.manifold import TSNE

tsne = TSNE(
    n_components=2,
    random_state=42
)

X_tsne = tsne.fit_transform(X)
```

## Best Used For

- Data exploration
- Cluster visualization
- High-dimensional visualization

## Advantages

- Useful for visualizing complex data
- Can reveal local clusters or patterns
- Works with nonlinear structures

## Limitations

- Can be computationally expensive
- Results can depend on parameters and initialization
- Distances between separated clusters should not automatically be interpreted as meaningful
- Mainly used for visualization rather than as a default preprocessing method for prediction

---

# 4. UMAP

## Definition

UMAP stands for Uniform Manifold Approximation and Projection.

It is a nonlinear dimensionality reduction technique that can represent high-dimensional data in fewer dimensions.

### Concept

```text
High-Dimensional Data
        ↓
       UMAP
        ↓
Low-Dimensional Representation
```

## Basic Syntax

```python
import umap

reducer = umap.UMAP(
    n_components=2,
    random_state=42
)

X_umap = reducer.fit_transform(X)
```

## Best Used For

- High-dimensional visualization
- Exploring clusters and structure
- Creating lower-dimensional representations

## Advantages

- Often fast on larger datasets
- Can preserve useful local structure
- Useful for 2D and 3D visualization

## Limitations

- Requires an additional library
- Results depend on hyperparameters
- The low-dimensional representation should be interpreted carefully

---

# t-SNE vs UMAP

| t-SNE | UMAP |
|---|---|
| Mainly used for visualization | Visualization and representation |
| Strong focus on local neighborhoods | Can preserve useful local and broader structure |
| Can be slower on large datasets | Often faster |
| Parameter-sensitive | Also parameter-sensitive |

---

# 5. Autoencoders

## Definition

An Autoencoder is a Neural Network that learns a compressed representation of input data.

It mainly contains:

- Encoder
- Latent Representation
- Decoder

### Architecture

```text
Original Input
      ↓
   Encoder
      ↓
Compressed Representation
      ↓
   Decoder
      ↓
Reconstructed Input
```

---

## Encoder

The Encoder converts the original high-dimensional input into a smaller representation.

```text
Many Features
     ↓
Few Latent Features
```

---

## Latent Representation

The compressed information learned by the Autoencoder is commonly called the latent representation or latent space.

It attempts to capture useful patterns from the original input.

---

## Decoder

The Decoder uses the compressed representation to reconstruct the original input.

```text
Compressed Representation
          ↓
        Decoder
          ↓
Approximate Original Data
```

The reconstructed data is not necessarily perfectly identical to the original input.

---

## Basic Conceptual Example

```python
from tensorflow.keras.models import Model
from tensorflow.keras.layers import Input, Dense

input_layer = Input(
    shape=(100,)
)

encoded = Dense(
    20,
    activation="relu"
)(input_layer)

decoded = Dense(
    100,
    activation="sigmoid"
)(encoded)

autoencoder = Model(
    input_layer,
    decoded
)
```

In this example:

```text
100 input dimensions
        ↓
20-dimensional representation
        ↓
100 reconstructed dimensions
```

## Advantages

- Can learn nonlinear representations
- Useful for complex high-dimensional data
- Commonly used with images and other complex datasets
- Can learn compressed representations automatically

## Limitations

- More complex than PCA
- Requires Neural Network training
- Usually requires more data and computation
- Hyperparameter tuning can be required
- Learned latent features may be difficult to interpret

---

# Compression Note

Dimensionality Reduction should not be confused with ordinary file compression.

## ZIP Compression

ZIP generally uses lossless compression.

```text
Original File
     ↓
Compressed ZIP
     ↓
Extract
     ↓
Original Information Recovered
```

## Autoencoder Compression

An Autoencoder learns a compact representation of patterns in data.

```text
Original Data
     ↓
Encoder
     ↓
Latent Representation
     ↓
Decoder
     ↓
Approximate Reconstruction
```

Therefore, an Autoencoder is not the same as creating a ZIP file.

---

# Technique Comparison

| Technique | Supervised? | Main Purpose |
|---|---:|---|
| PCA | No | General dimensionality reduction |
| LDA | Yes | Class separation |
| t-SNE | No | High-dimensional visualization |
| UMAP | No | Visualization and representation |
| Autoencoder | Usually No | Learned nonlinear representation |

---

# When to Use Which Technique

## PCA

Use when:

- Many numerical features exist
- Features contain redundant information
- A simpler representation is required
- General dimensionality reduction is needed

## LDA

Use when:

- A categorical target exists
- The problem is classification
- Class separation is important

## t-SNE

Use when:

- High-dimensional data needs to be visualized
- Local clusters and patterns need to be explored

## UMAP

Use when:

- High-dimensional data needs a low-dimensional representation
- Visualization is required
- t-SNE is too slow for the dataset

## Autoencoders

Use when:

- Data is complex and high-dimensional
- Nonlinear representations are useful
- Neural Network-based representation learning is appropriate

---

# Data Leakage

Dimensionality reduction techniques that learn from data should be fitted using training data only in a predictive Machine Learning workflow.

Incorrect:

```text
Complete Dataset
      ↓
Fit PCA
      ↓
Train-Test Split
```

Recommended:

```text
Train-Test Split
      ↓
Fit PCA on Training Data
      ↓
Transform Training Data
      ↓
Transform Test Data
```

This prevents information from the test set from influencing the learned transformation.

---

# Best Practices

- Understand why dimensionality reduction is needed before applying it.
- Scale numerical features before PCA when their scales differ.
- Select the number of PCA components using explained variance when appropriate.
- Use LDA when class labels are available and class separation is required.
- Use t-SNE mainly for visualization and exploration.
- Interpret t-SNE and UMAP plots carefully.
- Use Autoencoders when simpler techniques are insufficient for complex data.
- Fit learned transformations using training data only.
- Compare model performance before and after dimensionality reduction.
- Consider interpretability before replacing original features.

---

# Common Mistakes

- Confusing Feature Selection with Dimensionality Reduction
- Applying PCA without considering feature scaling
- Assuming Principal Components are original features
- Using LDA without a target
- Treating t-SNE plots as proof of real-world clusters
- Interpreting distances in t-SNE too literally
- Treating an Autoencoder like ordinary ZIP compression
- Performing dimensionality reduction on the complete dataset before splitting
- Reducing dimensions without checking how much useful information was lost

---

# Key Points

- Dimensionality Reduction represents data using fewer dimensions.
- Feature Selection keeps original features, while Dimensionality Reduction may create new ones.
- PCA preserves important variation without using the target.
- LDA uses class labels to improve class separation.
- t-SNE is mainly useful for high-dimensional visualization.
- UMAP provides low-dimensional representations of complex data.
- Autoencoders use Neural Networks to learn compressed representations.
- Dimensionality reduction can reduce complexity, noise, and computational cost.
- Some information may be lost during dimensionality reduction.
- Learned transformations should be fitted using training data only.

---

# Next Topic

Data Splitting