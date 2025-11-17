# K-Nearest Neighbors (K-NN) from Scratch

> This project is a from-scratch implementation of the K-NN algorithm in Python. The goal was not just to build the classifier, but to analyze its performance and stability, specifically investigating the impact of validation set size and data corruption.

### 1. Core Technical Implementation

To make the algorithm efficient, I did not use `for` loops. The core logic is fully vectorized using NumPy:

* **Vectorized Distance Matrix:** I calculated the entire `(m x n)` squared Euclidean distance matrix in one operation using the formula $D^2 = x^T x - 2x^T z + z^T z$ and NumPy broadcasting.
* **Vectorized Predictions:** I found the predictions for *all K values* (from 1 to 50) at once by using `np.argsort` to get the sorted neighbor indices, and then `np.cumsum` on the `{-1, 1}` labels to perform the "voting" efficiently.

---

### 2. Analysis 1: Impact of Validation Set Size (Task #1)

This experiment was designed to answer two key questions.

#### Q1: What are the fluctuations of the validation error as a function of *n*?

**Answer:** The fluctuations (variance) of the validation error **decrease significantly** as the size of the validation set (`n`) **increases**.

* **Evidence:**
    * The plot for `n=10` shows 5 chaotic lines that are far apart. This indicates the error estimate is unstable and has **high variance**.
    * The plot for `n=80` shows 5 tightly clustered lines that follow the same trend. This indicates the error estimate is stable and has **low variance**.
    * The final "Variance Plot" confirms this, showing the `n=10` line has the highest variance, and the `n=80` line has the lowest.

* **Conclusion:** A small validation set gives an unreliable error estimate. A larger validation set is required to get a trustworthy result.

| Validation Error (n=10) | Validation Error (n=80) |
| :---: | :---: |
| <img src="validation_error_n_10.png" width="400"> | <img src="validation_error_n_80.png" width="400"> |

![Variance Plot](error_variance_vs_n.png)

#### Q2: What is the prediction accuracy of K-NN as a function of *K*?

**Answer:** The prediction accuracy as a function of `K` perfectly illustrates the **bias-variance trade-off**. (Note: The plots show *error rate*, so lower is better accuracy).

* **Small `K` (e.g., K=1-5): High Variance / Overfitting**
    The error is unstable and sensitive to noise. The model is too "spiky" and is fitting to the noise in the training data.

* **Large `K` (e.g., K > 25): High Bias / Underfitting**
    The error rate consistently *increases* as `K` gets larger. The model becomes too "simple" and "oversmooths" the decision boundary, losing the local patterns.

* **Optimal `K` (e.s., K ≈ 5-15): The "Sweet Spot"**
    The best accuracy (lowest error) is in the middle. This "sweet spot" balances the trade-off, creating a model that is complex enough to capture the data's pattern but not so complex that it overfits the noise.

---

### 3. Analysis 2: Impact of Data Corruption (Task #2)

*(This is the optional task. I highly recommend you do it and add it. It's what makes this a complete project.)*

I then tested how data corruption (noise) affects model accuracy and the optimal `K`.

#### Finding:
1.  As data corruption increases, the overall accuracy (minimum error) gets worse.
2.  As corruption increases, the "optimal K" (the best number of neighbors) also **increases**.

#### Evidence:
This is because the model needs to average over more neighbors to "smooth out" and "cancel" the random noise.

*(Here you would add your plots for `Uncorrupted` vs. `Heavy-Corruption`)*

---

### 4. Key Takeaways & Technologies

* **Statistical:** Showed the bias-variance trade-off (small `K` = high variance, large `K` = high bias) and proved that validation set size is critical for reliable model selection.
* **Technical:** Demonstrated efficient, vectorized programming in Python.

**Technologies Used:**
* Python
* NumPy
* Matplotlib
