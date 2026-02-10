# assignment-4-predictive
# 📊 PDF Estimation Using GAN

## 📌 Overview

This project focuses on learning an **unknown probability density function (PDF)** directly from data samples, without assuming any analytical or parametric form. A **Generative Adversarial Network (GAN)** is used to implicitly model the probability distribution of a transformed random variable.

Real-world **Indian air quality data** is used for this task. The NO₂ concentration values are first transformed using a nonlinear function and then modeled using a GAN. The objective is to demonstrate that generative models can effectively learn complex, non-Gaussian distributions purely from data.

---

## 📁 Dataset Description

- **Dataset:** Indian Air Quality Dataset  
- **Source:** https://www.kaggle.com/datasets/shrutibhargava94/india-air-quality-data  

The **NO₂ concentration** feature is selected because it is continuous, real-valued, and exhibits a non-Gaussian distribution, making it suitable for density estimation using generative models.

The dataset contains **16,233 missing values** in the NO₂ column, which are removed during preprocessing before further analysis.

---

## ⚙️ Methodology

### 🔹 Step 1: Data Transformation

Each NO₂ concentration value `x` is transformed into a new variable `z` using the following nonlinear function:

z = x + a_r · sin(b_r · x)

Where:
- `r` is the university roll number  
- `a_r = 0.5 × (r mod 7)` → **Value used: 2.0**  
- `b_r = 0.3 × (r mod 5 + 1)` → **Value used: 0.3**

This transformation introduces nonlinearity into the data, increasing the complexity of the distribution and making the PDF estimation task more challenging.

---

### 🔹 Step 2: GAN-Based PDF Learning

#### 🧠 GAN Architecture

The GAN consists of two neural networks trained adversarially:

##### Generator
- Takes random noise sampled from a normal distribution
- Produces synthetic samples of the transformed variable `z`
- Implemented as a fully connected neural network
- Uses ReLU activation functions
- Outputs a single scalar value

The generator aims to produce samples that closely resemble the real data.

##### Discriminator
- Takes a single scalar value as input
- Outputs a probability indicating whether the input is real or generated
- Implemented as a fully connected neural network
- Uses a sigmoid activation function in the final layer

The discriminator’s goal is to correctly distinguish real samples from generated ones.

Both networks are trained simultaneously in an adversarial manner, allowing the generator to implicitly learn the underlying distribution of `z`.

---

## 🏋️ Training Details

- **Loss Function:** Binary Cross-Entropy Loss  
- **Optimizer:** Adam  

### Training Strategy
- The discriminator is trained on both real samples and generator-produced samples.
- The generator is trained to maximize the discriminator’s belief that generated samples are real.

Training continues until both networks reach a stable equilibrium.

---

## 📈 PDF Estimation from Generated Samples

After training:
1. A large number of samples are generated using the trained generator.
2. The probability density function is estimated using:
   - **Histogram-based density estimation**

This estimated PDF represents the GAN’s learned approximation of the true distribution of the transformed variable `z`.

---

## 🧪 Results

A histogram comparison between the **real data distribution** and the **GAN-generated distribution** shows a strong overlap in shape and range.

<p align="center">
  <img src="https://github.com/user-attachments/assets/a8a7ee76-29be-4351-91c4-b3e68b94f078" width="700">
</p>

---

## 🔍 Observations

### 1️⃣ Mode Coverage

The generator successfully captures the main regions where the real data is concentrated. The generated samples span the same major value ranges as the real samples, indicating good mode coverage.

---

### 2️⃣ Training Stability

<p align="center">
  <img src="https://github.com/user-attachments/assets/abe19122-4e69-4905-babf-a161adc53789" width="500">
</p>

#### Discriminator Loss
- Remains approximately between **1.3 and 1.5**
- Does not collapse to zero or diverge
- Indicates balanced discriminator learning

#### Generator Loss
- Fluctuates between approximately **0.6 and 1.0**
- Expected behavior in adversarial training
- No signs of instability or collapse

Overall, the loss curves remain bounded and oscillate smoothly, indicating stable GAN training.

---

### 3️⃣ Quality of Generated Distribution

The histogram comparison shows that the generated distribution closely matches the real data distribution in terms of shape and range. Minor differences in peak heights and tail behavior are observed, which are expected. Overall, the GAN effectively learns the underlying probability distribution without relying on explicit parametric assumptions.

---

## ✅ Conclusion

This project demonstrates that GANs can be effectively used for **probability density estimation** of complex, non-Gaussian data distributions. By learning directly from transformed real-world data, the GAN successfully captures the core structure of the underlying distribution, validating its suitability for implicit density modeling tasks.

