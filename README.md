# Sat_Image_Denoising
We have utilised the satellite image data set from doi: https://doi.org/10.48550/arXiv.1810.08468 and drone shots. 

First, we are considering the drone shots. Being temporal data, we are looping it to make separate frames. Then we took the first, third and fifth frame for testing and after testing we implemented the algorithm on all the frames.
Added Gaussian noise
Added random noise values generated are centered around 0, and approximately 68% of the noise values fall within ±25 of 0. A higher standard deviation results in more pronounced noise.


By applying median filter
-PSNR - 23.01 dB
 SSIM - 0.4925 (Very bad)

By Weiner filter
-PSNR-15.12 dB
 SSIM 0.3983 (very bad)

Hybrid method- SVD + Wavelet
So in SVD we are putting Rank ratio as a input 
which is controlling the feature-set to be considered of the image to which I need to reduce. SVD Rank Ratio is basically denoting the proportion of largest singular values to keep for preserving the image features and details.

 
Less significant feature tends to have more noise. So we are eliminating it.
Rank Ratio selection plays an integral part
SVD calculation
We are performing Wavelet Decomposition on the multispectral bands
- bayes, soft--> Wavelet family, mode -> haar ,bior2.2,coif2,db8,db4
- minimax, soft
- bayes, hard
- sure, soft are used 
We got the following result for the Satellite (Sentinel 2C) dataset.
-------------------------------------------------------------------
         |Rank Ratio| Wavelet | Thresholding| Mode | PSNR |  SSIM  |
------------------------------------------------------------------- 
Method 1 |  0.8     |  db4    | Bayes       | soft | 29.66| 0.9879 |
Method 2 |  0.8     |  db8    | Bayes       | soft | 29.66| 0.9878 |
Method 3 |  0.8     |  haar   | Bayes       | soft | 29.66| 0.9879 |
Method 4 |  0.8     |  bior2.2| Bayes       | soft | 29.66| 0.9879 |
Method 5 |  0.8     |  coif2  | Bayes       | soft | 29.66| 0.9879 |
--------------------------------------------------------------------
In the case of Satellite dataset we have experimented a bit with the different wavelengths but overall all of them gave similar result. Similarly we iterated for the drone shots getting the following values.
-------------------------------------------------------------------
         |Rank Ratio| Wavelet | Thresholding| Mode | PSNR |  SSIM  |
------------------------------------------------------------------- 
Method 1 |  0.7     |  db8    | Bayes       | soft | 29.66| 0.9878 |
Method 2 |  0.8     |  db8    | Sure        | soft | 29.56| 0.9672 |
Method 3 |  0.9     |  db8    | Bayes       | hard | 29.66| 0.9881 |
Method 4 |  0.8     |  db8    | Minimax     | soft | 29.60| 0.9738 |
--------------------------------------------------------------------
Where we can easily see how based on different thresholding method our result changes. We have specifically used Daubeschies as default as its a well performing wavelet family specially for image processing Bayes thresholding giving a better result compares to the other thresholding methods and a significantly good PSNR and a SSIM of 0.97-0.98, almost tending to 1, showing it's high resolution. It gives a far better result when compared to the median filtering.

This had been a passion project for me, and I can't thank @Debojyoti enough for his vital support to this project! 
Drone shots- taken from DroneStock!


Here one thing can be noticed, that finding the ideal SVD rank ratio is like finding the sweet spot which favours your dataset. If you notice for the drone shots 0.9 SVD rank ratio under Bayes hard thresholding gave the best result but that doesn't happen for Satellite data which suggest around 80% of the singular values had significant features under soft Bayes thresholding giving a SSIM of 0.9879. This gives a way better result if you compare to the other classical denoising techniques.
