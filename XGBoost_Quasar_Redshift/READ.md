# Project: XGBoost Regression for Quasar Redshift Estimation

> **Goal:** To implement and optimize a predictive model for estimating the redshift ($z$) of quasars from photometric data. The project compares the performance of an optimized XGBoost model against a simple K-Nearest Neighbors (K-NN) baseline, investigating the impact of model choice on prediction accuracy.

### 1. Problem Statement & Dataset

Quasars are extremely distant, luminous objects whose **redshift ($z$)** is essential for measuring cosmological distances. While high-precision redshift requires expensive spectral analysis, this project explores a fast, scalable estimation method using **10 spectral attributes** (photometric data) as features.

* **Total Instances:** 20,000
* **Model Task:** Regression (predicting a continuous value $z \in \mathbb{R}$).
* **Metric:** Root Mean Squared Error (RMSE) and $R^2$ Score.

---

### 2. Methodology: Model Optimization

The analysis included fixed-parameter training for baseline evaluation, followed by a robust hyperparameter optimization process.

#### A. Initial Model Assessment (Fixed Parameters)

An initial XGBoost model was trained (`n_estimators=500`, `learning_rate=0.1`, `max_depth=4`). A 10% hold-out validation set was used to monitor the training error vs. the unseen validation error.

The plot below shows the training and validation error curves. The validation RMSE flattens out around the 100-200 iteration mark, indicating optimal convergence and appropriate `n_estimators`.

****

#### B. Hyperparameter Grid Search (Optimization)

A 3-fold cross-validated grid search was performed on the training set to find the optimal hyperparameters that minimize the RMSE.

**Optimal Parameters Found:**

| Parameter | Optimal Value |
| :--- | :--- |
| `max_depth` | 4 |
| `learning_rate` | 0.1 |
| `colsample_bytree` | 0.75 |
| `reg_lambda` | 5.0 |

---

### 3. Final Results & Analytical Insight

The final, optimized XGBoost model was refit and evaluated against the K-Nearest Neighbors Regressor ($k=5$) baseline on the independent test set.

| Model | Test RMSE (Redshift Units) | Test $\mathbf{R^2}$ Score |
| :--- | :--- | :--- |
| **K-NN Baseline (k=5)** | **0.4798** | **0.3722** |
| **Optimized XGBoost** | 0.4866 | 0.3544 |

**Analytical Conclusion: K-NN Outperforms XGBoost**

The most significant finding is that the simpler, non-parametric **K-NN Baseline (RMSE 0.4798) slightly outperformed the Optimized XGBoost model (RMSE 0.4866)**.

This suggests that for this specific dataset:

1.  **Locality is Key:** The predictive signal might be highly dependent on the very local neighborhood of the data points, which K-NN captures effectively by averaging the nearest 5 neighbors.
2.  **Distance vs. Splits:** The Euclidean distance metric used by K-NN better captured the underlying similarity between quasar spectral profiles than the axis-aligned decision splits used by the tree-based XGBoost.

---

### Used

* **Python:** Core language for scripting.
* **XGBoost:** Primary model implementation.
* **Scikit-learn:** Data splitting, **Grid Search (Optimization)**, K-NN baseline, and metric calculation.
* **Pandas / NumPy:** Data handling and numerical operations.
* **Matplotlib:** Plotting the validation curves.
