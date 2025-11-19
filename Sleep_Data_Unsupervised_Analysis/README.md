# Unsupervised Analysis: PCA & Clustering on Sleep Patterns

> **Project Summary:** A final exploratory analysis of the high-dimensional Sleep-EDF dataset using **PCA** (Principal Component Analysis) and **K-Means Clustering**. This project showcases the ability to reduce feature dimensionality, visualize complex data, and critically evaluate algorithm stability.

***

### 1. Dimensionality Reduction (PCA)

I performed PCA on the 16-feature training data to understand the structure and redundancy of the EEG signals. The goal was to transform the data to a new set of orthogonal axes that maximize variance.

* **90% Variance Threshold:** The analysis showed that only **5 components** are necessary to explain $\mathbf{90\%}$ of the total variance in the 16-dimensional dataset. This confirms the data has significant redundancy, justifying feature reduction.

![PCA Eigenspectrum](pca_eigenspectrum.png)

* **2D Projection:** The scatter plot of PC1 vs PC2 shows that the data is not easily separable, even though the points are colored by their true sleep stage (0-4). This visualizes the challenge faced by the supervised models in my previous project.

### 2. Clustering Analysis (K-Means Stability)

I performed 5-means clustering (as there are 5 known sleep stages) and compared two initialization strategies to analyze the algorithm's stability.

#### Key Finding: Initialization Matters
The experiment demonstrates the critical nature of initialization in K-Means. K-Means++ found a significantly better solution than the standard Random Initialization.

| Initialization Method | Final Inertia (Cost) | Interpretation |
| :--- | :--- | :--- |
| **Standard K-Means (Random)** | 166,129.21 | **Sub-optimal.** The algorithm got stuck in a local minimum, resulting in a higher cost. |
| **K-Means++ (Smart Init)** | **157,343.52** | **Optimal.** The smart seeding avoided local minima and achieved a lower, more reliable cost. |

#### Visual Evidence
The overlay plots show that K-Means++ (right plot) achieved a cleaner distribution of centroids that better reflects the natural groupings of the data compared to the random centers (left plot).

| K-Means (Random Init) | K-Means++ (Smart Init) |
| :---: | :---: |
|  |  |

***

### 4. Key Takeaways & Technologies

* **Full-Stack Analysis:** This project completes the portfolio by demonstrating proficiency in **Unsupervised Learning** methods (PCA and Clustering).
* **Statistical Insight:** Used the Eigenspectrum to justify data dimensionality reduction, showing an understanding of variance and information preservation.
* **Algorithm Optimization:** Proved the practical advantage of using **K-Means++** to ensure stability and reliable convergence in iterative models.

| Tech Stack | Analytical Skills |
| :--- | :--- |
| **Scikit-Learn** | **Dimensionality Reduction** (PCA). |
| **NumPy & Pandas** | **Clustering** (K-Means/K-Means++). |
| **Matplotlib** | **Critical Evaluation** (Inertia, Initialization). |
