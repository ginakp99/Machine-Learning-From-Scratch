# K-Nearest Neighbors (K-NN) from Scratch

> This project is a from-scratch implementation of the K-NN algorithm in Python. The goal was not just to build the classifier, but to analyze its performance and stability, specifically investigating the impact of validation set size and data corruption.

### 1. Core Technical Implementation

To make the algorithm efficient, I did not use `for` loops. The core logic is fully vectorized using NumPy:

* **Vectorized Distance Matrix:** I calculated the entire `(m x n)` squared Euclidean distance matrix in one operation using the formula $D^2 = x^T x - 2x^T z + z^T z$ and NumPy broadcasting.
* **Vectorized Predictions:** I found the predictions for *all K values* (from 1 to 50) at once by using `np.argsort` to get the sorted neighbor indices, and then `np.cumsum` on the `{-1, 1}` labels to perform the "voting" efficiently.

---

### 2. Analysis 1: Impact of Validation Set Size (Task #1)

The goal was to test how the *size* of the validation set (`n`) affects the reliability of our error estimate.

#### Finding:
As `n` increases, the variance of the error estimate drops significantly. A small `n=10` gives chaotic, unreliable results, while `n=80` gives a stable, trustworthy estimate.

#### Evidence:
This is clearly visible when comparing the 5 validation folds for `n=10` vs. `n=80`. The `n=80` lines are tightly clustered, while the `n=10` lines are far apart.

| Validation Error (n=10) | Validation Error (n=80) |
| :---: | :---: |
| <img src="../validation_error_n_10.png" width="400"> | <img src="../validation_error_n_80.png" width="400"> |

This is confirmed by the final **Variance Plot**, which shows the `n=10` line (blue) has dramatically higher variance than the `n=80` line (red).

![Variance Plot](../error_variance_vs_n.png)

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
