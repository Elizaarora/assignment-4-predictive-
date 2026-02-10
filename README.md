# assignment-4-predictive-
PDF Estimation Using GAN
Overview

This project explores how an unknown probability density function (PDF) can be learned directly from data samples, without assuming any predefined analytical or parametric form. To achieve this, a Generative Adversarial Network (GAN) is employed to implicitly model the distribution of a transformed random variable.

Real-world Indian air quality data is used for this task. Specifically, NO₂ concentration values are selected, transformed using a nonlinear function, and then modeled using a GAN. The goal is to demonstrate that generative models can effectively approximate complex, non-Gaussian probability distributions purely from data.

Dataset Description

Dataset: Indian Air Quality Dataset

Source: https://www.kaggle.com/datasets/shrutibhargava94/india-air-quality-data

The NO₂ concentration feature is chosen because it is continuous, real-valued, and exhibits a non-Gaussian distribution, making it well suited for density estimation using generative models.

The dataset initially contains 16,233 missing values in the NO₂ column. These missing entries are removed during preprocessing to ensure clean and reliable training data.

Methodology
Step 1: Data Transformation

Each NO₂ concentration value 
𝑥
x is transformed into a new variable 
𝑧
z using the nonlinear transformation:

𝑧
=
𝑥
+
𝑎
𝑟
⋅
sin
⁡
(
𝑏
𝑟
⋅
𝑥
)
z=x+a
r
	​

⋅sin(b
r
	​

⋅x)

Where:

𝑟
r is the university roll number

𝑎
𝑟
=
0.5
×
(
𝑟
 
m
o
d
 
7
)
a
r
	​

=0.5×(rmod7) → Calculated value: 2.0

𝑏
𝑟
=
0.3
×
(
𝑟
 
m
o
d
 
5
+
1
)
b
r
	​

=0.3×(rmod5+1) → Calculated value: 0.3

This transformation introduces nonlinearity into the data, increasing its complexity and making the density estimation task more challenging and realistic.

Step 2: GAN-Based PDF Learning
GAN Architecture

The model consists of two neural networks trained in an adversarial framework:

Generator

Takes random noise sampled from a normal distribution

Outputs synthetic samples of the transformed variable 
𝑧
z

Implemented as a fully connected neural network

Uses ReLU activation functions

Produces a single scalar output

The generator’s objective is to produce samples that are indistinguishable from real data.

Discriminator

Takes a single scalar value as input

Outputs a probability indicating whether the input is real or generated

Implemented using a fully connected neural network

Uses a sigmoid activation function in the final layer

The discriminator’s role is to correctly classify real and generated samples.

Both networks are trained simultaneously: the discriminator improves its classification ability, while the generator learns to fool the discriminator. Over time, this adversarial process enables the generator to implicitly learn the underlying probability distribution of 
𝑧
z.

Training Details

Loss Function: Binary Cross-Entropy Loss

Optimizer: Adam

Training Strategy:

The discriminator is trained on both real samples and generator-produced samples.

The generator is trained to maximize the discriminator’s confidence that generated samples are real.

Training continues until both networks reach a stable equilibrium, indicating balanced adversarial learning.

PDF Estimation from Generated Samples

After training is complete:

A large number of samples are generated using the trained generator.

The probability density function is estimated using:

Histogram-based density estimation

This histogram represents the GAN’s learned approximation of the true PDF of the transformed variable 
𝑧
z.

Results

A histogram comparison between the real data distribution and the GAN-generated distribution shows strong alignment:

(Histogram comparing real PDF and GAN-generated PDF)

Observations
1. Mode Coverage

The generator successfully captures the main regions where real data points are concentrated. The generated samples span similar ranges and cover the dominant modes of the real distribution, indicating effective mode coverage.

2. Training Stability

(Generator and Discriminator loss plots)

Discriminator Loss

Remains approximately between 1.3 and 1.5

Does not collapse to zero (which would indicate an overly powerful discriminator)

Does not diverge to excessively large values

This suggests that the discriminator maintains a balanced learning pace.

Generator Loss

Fluctuates between approximately 0.6 and 1.0

Such oscillations are typical in adversarial training

No signs of collapse or unbounded growth are observed

Overall, both losses remain bounded and oscillate smoothly, indicating stable and well-balanced GAN training.

3. Quality of Generated Distribution

The quality of the learned distribution is evaluated by visually comparing the histograms of real and generated samples. Both distributions exhibit a similar overall shape and are concentrated within the same value ranges. While minor differences are observed in peak height and tail behavior, the generator successfully captures the core structure of the data.

This demonstrates that the GAN is capable of producing realistic samples and effectively learning the underlying probability distribution without relying on explicit parametric assumptions.
