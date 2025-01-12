# Neural Regression Tree (NRT)

## Overview
The Neural Regression Tree (NRT) is a novel approach for Regression-via-Classification (RvC). This method converts a regression problem into a classification problem using hierarchical tree structures optimized for both classification and regression accuracy. NRT addresses the limitations of traditional RvC models by learning optimal discretization thresholds and adapting features locally at each node in the tree using neural classifiers.

---

## Key Concepts

### 1. Regression-via-Classification (RvC)
- Transforms a continuous regression variable into discrete bins.
- Classification rules identify the bin, and regression rules estimate the value within the bin.
- Objective: Minimize both classification and regression errors.

### 2. Neural Regression Tree (NRT)
- Combines regression trees and neural networks.
- Optimizes discretization thresholds and features jointly.
- Employs a hierarchical binary tree structure with neural classifiers at each node.
- Adapts features to enhance discriminative power for each partition.

---

## Model Components

### Partition
A partition, \(\Pi(Y)\), divides the response variable \(y \in Y\) into \(N\) disjoint intervals:
\[ \Pi(Y) = \{C_1, C_2, \ldots, C_N\}, \text{where } \bigcup_{n=1}^{N} C_n = Y \]

### Classification Rule
Maps input features \(x\) to bins:
\[ h_{\theta}: x \rightarrow \{C_1, \ldots, C_N\} \]

### Regression Rule
Maps \(x\) and \(C_n\) to response intervals:
\[ r: (x, C_n) \rightarrow (t_{n-1}, t_n] \]

### Combined Rule
Predicts \(\hat{y}(x)\) using posterior probabilities:
\[ \hat{y}(x) = \sum_{n=1}^{N} h_{\theta}^n(x) r_{C_n}(x) \]

### Objective
Learn optimal thresholds and parameters:
\[ \{t^*_n\}, \{\theta^*_n\} \leftarrow \arg \min_{t, \theta} \mathbb{E}_{x}[E(y, \hat{y}(x))] \]

---

## Tree Construction

The tree is built hierarchically, optimizing each node individually in a greedy manner.

### Node Optimization
Each node \(n\) minimizes a combined loss:
\[ \theta^*_n, t^*_n = \arg \min_{\theta_n, t_n} \lambda E_{\theta_n, t_n} + (1-\lambda) T(t_n) \]

### Classification Loss
Measures error in classifying data at each node:
\[ E_{\theta_n, t_n} = \frac{1}{|D_n|} \sum_{(x, y) \in D_n} E(y(t_n), h_{\theta_n}(x)) \]

### Triviality Penalty
Prevents trivial solutions by balancing data across nodes using entropy:
\[ T(t_n) = -p(t_n) \log p(t_n) - (1-p(t_n)) \log(1-p(t_n)) \]
where \(p(t_n)\) is the proportion of data assigned to the left child.

---

## Threshold Optimization

### Scan Method
An exhaustive search across all possible threshold values \(t_n\), selecting the one that minimizes the loss.

### Gradient Method
Smooth approximation of the binary indicator function \(y(t_n)\) for differentiability:
\[ y(t_n) = 0.5(\tanh(\beta(y - t_n)) + 1) \]
where \(\beta\) controls the steepness of the transition.

### Joint Optimization
Combines the feature optimization and threshold search within a node. The loss function is optimized using coordinate descent, alternating between classifier parameters \(\theta_n\) and threshold \(t_n\).

---

## Experiments

### Tasks
1. **Age Estimation**
   - Dataset: Fisher English Corpus
   - Features: 400-dimensional i-vectors

2. **Height Estimation**
   - Dataset: NIST-SRE8 Corpus
   - Features: 600-dimensional i-vectors

### Baselines
- Support Vector Regression (SVR)
- Classification and Regression Trees (CART)
- Multilayer Perceptron (MLP)
- Regression Tree with Support Vector Machines (SVM-RT)

### Metrics
- Mean Absolute Error (MAE)
- Root Mean Squared Error (RMSE)

### Results
NRT outperforms baselines in both tasks, demonstrating superior performance in terms of MAE and RMSE, particularly for younger age groups where vocal characteristics are more discriminative.

---

## Limitations
- Not universally optimal across all regression tasks.
- Optimizes classification error, not regression error directly.
- Higher variance in specific tasks compared to SVR.

---


