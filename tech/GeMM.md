# Introduction

GeMM stands for General Matrix Multiplication. It is part of the BLAS library. It can utilize the parallelism in CPUs and GPUs.       

Every clock cycle, output of matrix updates its element of C, using one row of A and one column of B to multiply and the accumulation. Such as for 4x4 matrix, there are 16 multiplications and 16 accumulation at one time.

Useful sources: [Why GEMM is at the heart of deep learning](https://petewarden.com/2015/04/20/why-gemm-is-at-the-heart-of-deep-learning/), 

# CNN using GeMM

GeMM doesn't exploited the data reuse in CNN. 

![CNN using GeMM](CNN_GeMM_1.png)

![CNN using GeMM](CNN_GeMM_2.png)

# GeMM Details

This is an example of input size 16 by 16. 

![GeMM Details](GeMM_1.png)
![GeMM Details](GeMM_2.png)
![GeMM Details](GeMM_3.png)

# GeMM on CPU

Example: for Intel and AMD, Vector Neural Network Instructions (AVX512 VNNI) for fused (integer) multiply add. 
-  AVX512, with additional instructions to multiply e.g. 64 parallel 8-bit int numbers, and accumulate neighbors
- Limited parallelism: up to 64 16bit ops/CC per AVX instruction (7 Tops/Xeon)
- Large energy consumption: consume ~150W (~ 10-100 Gops/W)
- Limited bandwidth (caching helps, yet limited IO bandwidth)