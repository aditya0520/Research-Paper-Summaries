
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
A partition, \( \Pi(Y) \), divides the response variable \( y \in Y \) into \( N \) disjoint intervals:
![Partition Equation](https://latex.codecogs.com/png.latex?%5CPi%28Y%29%20%3D%20%5C%7BC_1%2C%20C_2%2C%20%5Cldots%2C%20C_N%5C%7D%2C%20%5Ctext%7Bwhere%7D%20%5Cbigcup_%7Bn%3D1%7D%5E%7BN%7D%20C_n%20%3D%20Y)

### Classification Rule
Maps input features \( x \) to bins:
![Classification Rule](https://latex.codecogs.com/png.latex?h_%7B%5Ctheta%7D%3A%20x%20%5Cto%20%5C%7BC_1%2C%20%5Cldots%2C%20C_N%5C%7D)

### Regression Rule
Maps \( x \) and \( C_n \) to response intervals:
![Regression Rule](https://latex.codecogs.com/png.latex?r%3A%20%28x%2C%20C_n%29%20%5Cto%20%28t_%7Bn-1%7D%2C%20t_n%5D)

### Combined Rule
Predicts \( \hat{y}(x) \) using posterior probabilities:
![Combined Rule](https://latex.codecogs.com/png.latex?%5Chat%7By%7D%28x%29%20%3D%20%5Csum_%7Bn%3D1%7D%5E%7BN%7D%20h_%7B%5Ctheta%7D%5En%28x%29%20r_%7BC_n%7D%28x%29)

### Objective
Learn optimal thresholds and parameters:
![Objective Equation](https://latex.codecogs.com/png.latex?%5C%7Bt%5E*_n%5C%7D%2C%20%5C%7B%5Ctheta%5E*_n%5C%7D%20%5Cgets%20%5Carg%20%5Cmin_%7Bt%2C%20%5Ctheta%7D%20%5Cmathbb%7BE%7D_%7Bx%7D%5BE%28y%2C%20%5Chat%7By%7D%28x%29%29%5D)

---

## Tree Construction

The tree is built hierarchically, optimizing each node individually in a greedy manner.

### Node Optimization
Each node \( n \) minimizes a combined loss:
![Node Optimization](https://latex.codecogs.com/png.latex?%5Ctheta%5E*_n%2C%20t%5E*_n%20%3D%20%5Carg%20%5Cmin_%7B%5Ctheta_n%2C%20t_n%7D%20%5Clambda%20E_%7B%5Ctheta_n%2C%20t_n%7D%20%2B%20%281-%5Clambda%29%20T%28t_n%29)

### Classification Loss
Measures error in classifying data at each node:
![Classification Loss](https://latex.codecogs.com/png.latex?E_%7B%5Ctheta_n%2C%20t_n%7D%20%3D%20%5Cfrac%7B1%7D%7B%7CD_n%7C%7D%20%5Csum_%7B%28x%2C%20y%29%20%5Cin%20D_n%7D%20E%28y%28t_n%29%2C%20h_%7B%5Ctheta_n%7D%28x%29%29)

### Triviality Penalty
Prevents trivial solutions by balancing data across nodes using entropy:
![Triviality Penalty](https://latex.codecogs.com/png.latex?T%28t_n%29%20%3D%20-p%28t_n%29%20%5Clog%20p%28t_n%29%20-%20%281-p%28t_n%29%29%20%5Clog%281-p%28t_n%29%29)

---

## Threshold Optimization

### Scan Method
An exhaustive search across all possible threshold values \( t_n \), selecting the one that minimizes the loss.

### Gradient Method
Smooth approximation of the binary indicator function \( y(t_n) \) for differentiability:
![Gradient Method](https://latex.codecogs.com/png.latex?y%28t_n%29%20%3D%200.5%28%5Ctanh%28%5Cbeta%28y%20-%20t_n%29%29%20%2B%201%29)

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
