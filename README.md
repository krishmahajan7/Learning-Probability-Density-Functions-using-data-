# Learning Probability Density Functions using GAN


## Project Overview

This project demonstrates how a **Generative Adversarial Network (GAN)** can learn an unknown probability density function (PDF) directly from data.

The **NO₂ concentration** from the India Air Quality dataset is used as the feature \( x \).  
A nonlinear transformation is applied to generate a new variable \( z \), and a GAN is trained to learn the distribution of \( z \) **without assuming any parametric distribution** (such as Gaussian or exponential).

The objective is to approximate the PDF of the transformed variable purely from samples.

---

## Dataset

- **Dataset:** India Air Quality Data (Kaggle)
- **Feature Used:** NO₂ concentration
- Missing values were removed before processing.

---

## Step 1: Data Transformation

Each NO₂ value \( x \) was transformed into:

\[
z = x + a_r \sin(b_r x)
\]

Where:

\[
a_r = 0.5 \times (r \bmod 7)
\]

\[
b_r = 0.3 \times ((r \bmod 5) + 1)
\]

\( r \) = University Roll Number

### Preprocessing for Stable Training

- Removed top 1% extreme outliers  
- Applied log transformation  
- Scaled data to range (-1, 1)

This ensures stable GAN training and prevents mode collapse.

---

### Step 2: GAN Architecture

### Generator

- Input: 1D Gaussian noise \( N(0,1) \)
- Fully connected layers: **64 → 128 → 64**
- Activation: LeakyReLU
- Output layer: 1 neuron with **Tanh** activation

The generator learns to convert random noise into realistic samples of \( z \).

---

### Discriminator

- Fully connected layers: **128 → 64**
- Activation: LeakyReLU
- Output layer: 1 neuron with **Sigmoid** activation

The discriminator classifies samples as **Real** or **Fake**.

---

###  Training Configuration

- Optimizer: Adam  
- Learning Rate: 0.0002  
- Loss Function: Binary Cross Entropy  
- Batch Size: 256  
- Noise Distribution: Standard Gaussian  

---

## Step 3: PDF Approximation

After training:

- 20,000+ synthetic samples were generated from the trained generator.
- PDF was estimated using:
  - Histogram Density Estimation
  - Kernel Density Estimation (KDE)

The generated distribution closely matches the real distribution in:

- Peak location  
- Spread  
- Overall shape  

---

## Observations

### Mode Coverage
The GAN successfully captures the dominant mode of the distribution and avoids mode collapse.

### Training Stability
After preprocessing (log transformation and outlier removal), training became stable and losses converged smoothly.

### Quality of Generated Distribution
The generated samples closely resemble the real distribution, demonstrating successful non-parametric PDF learning.

---

## Usage

### Step 1: Place Dataset

Place the dataset file inside the project directory.

### Step 2: Run the Script

    python main.py

Or run the Jupyter Notebook:

    jupyter notebook

### Step 3: Output

The program will:

- Transform the data
- Train the GAN
- Generate synthetic samples
- Plot:
  - KDE comparison
  - Histogram-based PDF
- Save plots as `.png` files

---

##Project Structure

    ├── Dataset.zip
    ├── learning_probability_density_functions_using_...
    ├── KDE_pdf.png
    ├── histogram_pdf.png
    ├── PDF Estimation using GAN.png
    ├── Submission.txt
    ├── README.md

---

##Conclusion

This project demonstrates that a GAN can successfully learn an unknown probability density function purely from data samples.

The generator implicitly models the PDF by producing realistic synthetic samples that match the real distribution.

✔ No parametric assumptions  
✔ Data-driven modeling  
✔ Successful PDF approximation  

---

##  Academic Note

This project was developed as part of an academic assignment on learning probability density functions using data only under the Predictive Analytics using Statistics (UCS654) course.

---

## Author

Krish Mahajan
