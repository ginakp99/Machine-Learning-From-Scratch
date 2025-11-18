# Sleep Stage Classification: A Comparative Analysis

> **Project Summary:** A machine learning project to automate sleep staging using EEG data from the Sleep-EDF-15 dataset. I implemented and compared three distinct classifiers (Logistic Regression, Random Forest, and K-NN) to determine which algorithm best generalizes to unseen medical data.
>
> **Key Result:** Contrary to common intuition, the simpler linear model (**Logistic Regression**) outperformed complex non-linear models (Random Forest), achieving the lowest test error of **9.95%**.

### 1. Data Understanding & Preprocessing

The dataset consists of feature vectors extracted from EEG signals, categorized into 5 sleep stages (0=Wake, 1=N1, 2=N2, 3=N3, 4=REM).

* **Dataset Size:** 33,723 training samples, 13,900 test samples.
* **Class Imbalance:** The data is highly imbalanced. Class 0 (Wake) comprises **52.09%** of the data, while Class 3 (Deep Sleep) represents only **4.69%**. This makes a simple accuracy metric misleading (a dummy classifier could get 52% accuracy), so achieving ~90% accuracy is significant.
* **Preprocessing:** Applied `StandardScaler` to normalize features (zero mean, unit variance) for distance-based (K-NN) and gradient-based (Logistic Regression) models.

### 2. Model Methodology

I trained and evaluated three progressively complex models to understand the decision boundaries of the data:

1.  **Logistic Regression (Baseline):** A linear classifier using Softmax (Multinomial) regression and L2 regularization ($C=1.0$) to prevent overfitting.
2.  **Random Forest (Non-Linear):** An ensemble of decision trees. I experimented with 50, 100, and 200 trees to test for overfitting.
3.  **K-Nearest Neighbors (Instance-Based):** I used **5-fold Cross-Validation** on the training set to scientifically determine the optimal number of neighbors ($k$).

### 3. Results & Analysis

| Model | Configuration | Training Error | Test Error | Accuracy |
| :--- | :--- | :--- | :--- | :--- |
| **Logistic Regression** | L2 Regularization | 14.95% | **9.95%** | **90.05%** |
| **K-Nearest Neighbors** | $k=20$ | 14.05% | 10.24% | 89.76% |
| **Random Forest** | 200 Trees | **0.00%** | 11.09% | 88.91% |

#### Key Findings:

* **The "Occam's Razor" Win:** Logistic Regression was the best performing model. This strongly suggests that the decision boundaries between sleep stages in this specific feature space are **linear**. The model generalized exceptionally well, with a lower test error than training error (likely due to a "cleaner" test set distribution).
* **Overfitting in Random Forests:** The Random Forest achieved a **0.00% training error**, meaning it perfectly memorized the training data. However, it had the highest test error (11.1%), proving it was "trying too hard" to fit noise rather than the underlying signal.
* **Optimal Neighbors ($k$):** The cross-validation process identified **$k=20$** as optimal. A larger $k$ helps smooth out decision boundaries, which is beneficial in noisy medical data.

### 4. Technologies & Skills Demonstrated

* **Libraries:** `scikit-learn`, `pandas`, `numpy`
* **Techniques:** Cross-Validation, Hyperparameter Tuning, Feature Standardization (Scaling), L2 Regularization.
* **Analysis:** diagnosing variance vs. bias (overfitting vs. underfitting) and interpreting model results in the context of the data distribution.
