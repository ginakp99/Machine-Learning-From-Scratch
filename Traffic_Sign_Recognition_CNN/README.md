# Traffic Sign Recognition for Autonomous Driving (CNN)

> **Project Summary:** A Computer Vision project designed to classify traffic signs for autonomous vehicle systems. Using the German Traffic Sign Recognition Benchmark (GTSRB), I built a Convolutional Neural Network (CNN) in PyTorch from scratch. By implementing a robust data augmentation pipeline (including motion blur simulation), the model achieved **91.96% accuracy** on unseen test data.

### 1. The Challenge
Traffic sign recognition is a safety-critical multi-class classification problem (43 distinct classes). Real-world deployment faces significant challenges:
* **Environmental Variability:** Changes in lighting (day/night/shadows).
* **Perspective Distortion:** Signs viewed from different angles and distances.
* **Image Quality:** Motion blur and low-resolution sensors.

### 2. Technical Approach

#### Network Architecture
I designed a custom CNN architecture optimized for spatial feature extraction on $28 \times 28$ pixel inputs:

* **Input:** RGB Images ($3 \times 28 \times 28$).
* **Feature Extraction:** Two Convolutional blocks (64 filters, $5 \times 5$ kernel).
* **Activation:** Used **ELU (Exponential Linear Unit)** instead of standard ReLU to allow negative activations, pushing mean unit activations closer to zero and speeding up convergence.
* **Pooling:** Max Pooling ($2 \times 2$) to reduce spatial dimensions and enforce translation invariance.
* **Classifier:** A dense fully connected layer mapping **1024 extracted features** ($64 \times 4 \times 4$) to 43 probability scores.

#### Data Augmentation Strategy
To ensure the model generalizes to real-world driving conditions, I implemented a dynamic augmentation pipeline using `torchvision`:
1.  **Random Affine:** Simulates rotation and position shifts (perspective changes).
2.  **Color Jitter:** Simulates variable lighting conditions (weather/time of day).
3.  **Gaussian Blur:** Simulates camera focus issues and **motion blur**, which is essential for high-speed driving scenarios.

### 3. Performance Results

The model was trained for 10 epochs using the **Adam** optimizer (`lr=0.001`) and **Cross-Entropy Loss**.

* **Training Stability:** The loss converged smoothly from **1.55** (Epoch 1) to **0.12** (Epoch 10), indicating stable learning without divergence.
* **Generalization:** The model achieved a final accuracy of **91.96%** on the independent test set (12,630 images).

![Training Loss Curve](training_curve.png)

### 4. Technologies Used
* **Deep Learning:** PyTorch, Torchvision (Autograd, CNNs).
* **Data Engineering:** Custom `DataLoader` and transformation pipelines.
* **Visualization:** Matplotlib for loss tracking.
