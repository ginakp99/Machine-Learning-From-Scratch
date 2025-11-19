# Variable Star Classification: Kernel SVM with Hyperparameter Tuning

> **Project Summary:** A supervised learning project utilizing **Support Vector Machines (SVMs)** with a **Gaussian (RBF) Kernel** to classify variable stars based on their light curve features. This project focuses on the crucial process of model selection and the interpretation of the regularization parameter (C) in controlling model complexity.

### 1. The Challenge
The goal is to perform a binary classification to distinguish between two types of variable stars ("Beta Persei" vs. "Beta Lyrae") using a 61-dimensional feature vector derived from their astronomical light curves.

The key challenge is **hyperparameter optimization**; the SVM's performance relies heavily on tuning the regularization strength ($C$) and the kernel bandwidth ($\gamma$).

### 2. Methodology: Grid Search & Model Selection (Task 2.2)

To find the optimal hyperparameters, I used **5-fold Cross-Validation (CV)** combined with **Grid Search** on the training data.

* **Data Prep:** Input features were standardized (zero mean, unit variance) using the training data only, a necessary step for distance-based kernels.
* **Grid Search:** Tested 25 combinations of $C \in [0.01, 100]$ and $\gamma \in [0.001, 10]$ on a logarithmic scale.
* **Selection:** The pair of $(C, \gamma)$ yielding the highest average CV accuracy was selected to train the final model.

| Deliverable | Result |
| :--- | :--- |
| **Optimal C** | 1.0 |
| **Optimal Gamma ($\gamma$)** | 0.01 |
| **Final Test Loss (0-1 Error)** | **0.2073 (79.27% Accuracy)** |

### 3. Advanced Analysis: Inspecting the Margin (Task 2.3)

This analysis proved how the theoretical **regularization parameter $C$** directly controls the complexity and generalization of the model in the high-dimensional feature space.

| C Value (Penalty Strength) | Theoretical Effect | Experimental Result (Bounded SVs) |
| :--- | :--- | :--- |
| **1.0000 (Optimal C)** | Balanced margin. | 55 |
| **0.0100 (Drastically Lower)** | Forces margin to widen (simpler model). | 134 **(Increased by 144%)** |
| **0.0010 (Minimal C)** | Forces maximum possible margin. | 136 |

#### Key Insight:
* The number of **Bounded Support Vectors** (points violating the margin) significantly **increases** as $C$ is drastically decreased, while the number of **Free Support Vectors** (points on the margin) decreases.
* This verifies that reducing $C$ forces the decision boundary to become simpler and smoother (a wider margin), capping the influence of most points to the smaller $C$ value.

### 4. Key Takeaways 

* **Model Depth:** Demonstrated proficiency with **Kernelized SVMs** (Gaussian/RBF) and the mathematical implications of working in a potentially infinite-dimensional feature space.
* **Methodological Rigor:** Successfully performed a complete hyperparameter tuning cycle using **Grid Search** and **5-Fold Cross-Validation**.
* **Interpretation:** Proved a theoretical relationship ($C$ vs. Support Vectors) experimentally, showcasing critical thinking and debugging skills.

**Tech Stack:** `scikit-learn` (SVC, GridSearchCV), `NumPy`, `Pandas`, `StandardScaler`.
