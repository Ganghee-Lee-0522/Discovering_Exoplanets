# 🚀 Discovering Exoplanets from Noisy Light Curves Using Fourier Series [![📈POSTER](https://img.shields.io/badge/-📈%20POSTER-blue)](https://github.com/ML-Ewha-24-2/Discovering_Exoplanets/blob/main/poster.png)
<img width="684" alt="image" src="https://github.com/user-attachments/assets/92116972-1efa-4081-a6e9-56fb41c300bf" />

## 📌 Project Overview  
🔭 This project aims to detect exoplanets by analyzing stellar brightness variations.  
When an exoplanet transits in front of its host star, it blocks a fraction of the star’s light,  
creating periodic dimming patterns. However, real-world observations contain significant noise  
from telescope sensors, space environment, and other interference.  

✨ Our approach leverages **Fourier Transform** to filter out high-frequency noise and enhance  
periodic signals related to exoplanet transits. We then use **XGBoost** to classify  
whether a star has an exoplanet based on processed light curves.

---

## 📌 Problem Definition  
🪐 When an **exoplanet** passes in front of its host star, part of the star's light is blocked,  
leading to a detectable change in brightness.  
As exoplanets orbit their stars, they create **periodic brightness variations**,  
which can be analyzed to determine their presence.  

⚠️ However, real-world data contains **various sources of noise**,  
including telescope sensor noise, space environmental interference,  
and system errors, making it challenging to accurately detect these periodic changes.  

🎯 Our **ultimate goal** is to **analyze brightness variations despite noise interference**  
and **predict the presence of exoplanets**.

---

## 📊 Dataset  
📡 **Source**: NASA Kepler Space Telescope  
⏳ **Description**: Brightness measurements taken every 30 minutes  
📌 **Size**: 5,087 samples (37 positive cases, 5,050 negative cases)  
⚠️ **Challenge**: Highly imbalanced dataset  

📈 **We modeled the total flux as follows**:  

$F_{\text{total}} = F_{\text{avg}} + A F_s + F_n$  

where:  
- $F_{\text{total}}$ is the observed brightness  
- $F_{\text{avg}}$ is the average brightness  
- $A F_s$ represents planetary transit signals  
- $F_n$ is periodic noise

🛠️ We assumed the periodic distribution of noise to develop our model.

---

## 🔧 Data Preprocessing: Fourier Transform  
🔍 It is **difficult and inefficient** for a model to learn periodicity directly from the raw, noisy data.  
Therefore, we applied the **Fourier Transform** to convert the original **time-domain light curves**  
into the **frequency domain**, making periodicity more evident.  

📌 **Why was 1/9 Hz chosen?**  
- One of the **shortest exoplanetary orbital periods** is approximately **4.5 hours**  
- We defined **9 hours as the minimum orbital period**,  
  extracting only signals corresponding to **longer periods of planetary transit**  

💡 **After Fourier Transform Application**:  
✅ We used **Inverse Fourier Transform (IFT)** to remove high-frequency noise and reconstruct the signal.  
✅ The process eliminated short-term noise fluctuations while preserving the main planetary transit trends.  

---

## 🤖 Model Selection: XGBoost  
💡 We selected **XGBoost**, a **Boosting-based ensemble model**,  
which starts with weak learners and improves performance **iteratively by minimizing residuals**.  

📌 **XGBoost Training Process**:  
1️⃣ Initialize with a **base prediction (usually the mean)**  
2️⃣ Compute **residuals** (difference between actual and predicted values)  
3️⃣ Train new trees to **reduce residual errors**  
4️⃣ Repeat the process to obtain an **optimized model**  

🛠️ **Initially, we considered other time-series models**,  
but we ultimately selected **XGBoost** due to its **high learning efficiency with small datasets**  
and its **robust performance on numerical data**.

---

## 🔧 Data Augmentation Techniques  

### 📌 Data Sampling  
⚠️ Since **exoplanet-positive cases were extremely rare**,  
the model risked being **biased toward the majority class**.  
To address this, we used **SMOTE + Tomek Link** techniques to balance the dataset.  

- **SMOTE (Synthetic Minority Over-sampling Technique)**  
  - 📊 **Generates synthetic samples** for the minority class  
  - ✅ Achieved **~25% increase in dataset balance**  
- **Tomek Link**  
  - 🚀 **Removes unnecessary overlapping samples** at class boundaries to enhance decision-making  

---

## 📉 Feature Scaling & Dimensionality Reduction  

### 📌 Feature Scaling  
🛠️ Astronomical data contains a significant amount of **noise and outliers**.  
To minimize the impact of outliers, we applied **Robust Scaling**:  

✅ Trained the scaler on the training data  
✅ Applied the same transformation to test data  
✅ Normalized data distribution to improve model stability  

### 📌 Dimensionality Reduction (PCA)  
- **PCA (Principal Component Analysis) was used to reduce feature dimensionality**  
- The dataset contained numerous **flux-related features**  
- We extracted **10 principal components** and selected the **top 5** with the highest variance  
- 🔍 This preserved key information while filtering out **irrelevant noise**  

---

## 📈 Model Evaluation & Results  
📊 **Validation Method**: **Stratified K-Fold Cross Validation (5-Fold)**  
🎯 **Objective**: Prioritize **recall** to ensure that no exoplanet cases are missed  

| Model Variant         | Precision | Recall |
|----------------------|-----------|--------|
| Pure Data            | 50-70%    | 30-50% |
| Fourier Transform Applied   | 50-100%   | 40-90% |
| Full Pipeline (SMOTE + PCA + Scaling) | **86%** | -      |

---

## 🏁 Conclusion  
✅ **Fourier Transform significantly improved model performance**  
✅ **A correlation exists between low-frequency signals and exoplanet presence**  
✅ **Addressing dataset imbalance is critical for improving machine learning models**  

🚀 **Future Research Directions**:  
- 🧠 Implement **CNN/LSTM-based deep learning models**  
- 🔍 Apply **unsupervised learning for hidden exoplanet discovery**  
- 🎯 Explore **Bayesian optimization for hyperparameter tuning**  

---

## 🔗 Project Files
📂 [1. Discovering Exoplanets (Pure Data)](https://github.com/ML-Ewha-24-2/Discovering_Exoplanets/blob/main/1.Discovering_Exoplanets_pure.ipynb)  
📂 [2. Fourier Transform Applied](https://github.com/ML-Ewha-24-2/Discovering_Exoplanets/blob/main/2.Discovering_Exoplanets_Fourier_Transform.ipynb)  
📂 [3. Fourier Transform + Feature Engineering](https://github.com/ML-Ewha-24-2/Discovering_Exoplanets/blob/main/3.Discovering_Exoplanets_Fourier%20Transform_plus_%CE%B1.ipynb)  
📂 [4. Final Model Implementation](https://github.com/ML-Ewha-24-2/Discovering_Exoplanets/blob/main/4_Discovering_Exoplanets_final_model.ipynb)  

---

## 📌 References
🔗 [Kepler Space Telescope Data](https://exoplanetarchive.ipac.caltech.edu)  
🔗 [Fourier Transform in Signal Processing](https://en.wikipedia.org/wiki/Fourier_transform)  
🔗 [SMOTE: Synthetic Minority Over-sampling](https://imbalanced-learn.org/stable/over_sampling.html)  

---

## 👭 Members  

<table>
  <tr align="center">
    <td><img src="https://github.com/flowersayo.png" width="120"></td>
    <td><img src="https://github.com/Ganghee-Lee-0522.png" width="120"></td>
  </tr>
  <tr align="center">
    <td><a href="https://github.com/flowersayo">Seoyeon Kim</a></td>
    <td><a href="https://github.com/Ganghee-Lee-0522">Ganghee Lee</a></td>
  </tr>
</table>

---

## 💬 Contributions & Discussion  
💡 We welcome **feedback and contributions** to enhance this research! 🚀✨  
If you have any questions, feel free to reach out. 😊
