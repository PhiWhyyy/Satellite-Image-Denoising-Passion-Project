# 🛰️ Sat_Image_Denoising

Hybrid SVD–Wavelet based denoising framework for **satellite (Sentinel-2C)** and **drone imagery**, evaluated using PSNR and SSIM.

---

## Datasets
- **Satellite images:** Sentinel-2C  
  Source: https://doi.org/10.48550/arXiv.1810.08468  
- **Drone shots:** DroneStock  

---

## Preprocessing

### Drone Data
- Treated as **temporal data**
- Video split into individual frames
- Frames **1, 3, and 5** used for testing
- Final algorithm applied to all frames

### Noise Injection
- **Gaussian noise** added
- Mean ≈ 0
- ~68% of noise values within ±25  
- Higher standard deviation → stronger noise

---

##  Baseline Denoising Methods

### Median Filter
- **PSNR:** 23.01 dB  
- **SSIM:** 0.4925  

### Wiener Filter
- **PSNR:** 15.12 dB  
- **SSIM:** 0.3983  

> Classical filters show poor structural preservation for both satellite and drone imagery.

---

## Proposed Method: Hybrid SVD + Wavelet

### 1. Singular Value Decomposition (SVD)
- Input parameter: **Rank Ratio**
- Controls proportion of singular values retained
- Higher singular values → structural information  
- Lower singular values → noise-dominant components  

### 2. Wavelet Denoising
- Applied on individual **multispectral bands**
- Wavelet families tested:
  - `haar`, `db4`, `db8`, `bior2.2`, `coif2`
- Thresholding methods:
  - Bayes (soft / hard)
  - Sure (soft)
  - Minimax (soft)

---

## 📊 Results

### Satellite Dataset (Sentinel-2C)

| Method | Rank Ratio | Wavelet | Threshold | Mode | PSNR (dB) | SSIM |
|------|------------|---------|-----------|------|-----------|------|
| 1 | 0.8 | db4 | Bayes | Soft | 29.66 | 0.9879 |
| 2 | 0.8 | db8 | Bayes | Soft | 29.66 | 0.9878 |
| 3 | 0.8 | haar | Bayes | Soft | 29.66 | 0.9879 |
| 4 | 0.8 | bior2.2 | Bayes | Soft | 29.66 | 0.9879 |
| 5 | 0.8 | coif2 | Bayes | Soft | 29.66 | 0.9879 |

✔ Similar performance across wavelet families  
✔ ~80% singular values capture significant features  
✔ Bayes soft thresholding gives optimal results

---

### Drone Dataset

| Method | Rank Ratio | Wavelet | Threshold | Mode | PSNR (dB) | SSIM |
|------|------------|---------|-----------|------|-----------|------|
| 1 | 0.7 | db8 | Bayes | Soft | 29.66 | 0.9878 |
| 2 | 0.8 | db8 | Sure | Soft | 29.56 | 0.9672 |
| 3 | 0.9 | db8 | Bayes | Hard | 29.66 | 0.9881 |
| 4 | 0.8 | db8 | Minimax | Soft | 29.60 | 0.9738 |

✔ Higher rank ratio favors drone imagery  
✔ Bayes hard thresholding performs best at Rank Ratio = 0.9  

---

##  Key Observations
- **Optimal SVD rank ratio is dataset-dependent**
  - Satellite imagery → ~0.8
  - Drone imagery → ~0.9
- Hybrid SVD + Wavelet significantly outperforms classical filters
- Achieves **SSIM ≈ 0.97–0.99**, indicating strong structural preservation

---

## Conclusion
The Hybrid **SVD + Wavelet** method provides:
- Superior noise suppression
- High structural similarity
- Interpretability and lower computational cost compared to deep learning approaches

Well-suited for **remote sensing and aerial imaging applications**.

---

##  Acknowledgements
Special thanks to **@Debojyoti** for invaluable support and guidance throughout the project.

---

## 📜 License
This project is released for academic and research use.
