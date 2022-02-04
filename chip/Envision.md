# General GeMM workload

[[parallelization (spatial)]]: weight and input reuse
[[stationarity (temporal)]]: output stationarity
[[arithmetic intensity]] 256/32
sparsity: input and weight sparsity. once the input or weight is zero, the corresponding one row or one column multiplications can be skipped.

# Speed up convolution better than GeMM

For convolution, it is less efficient to transform it into GeMM, but it is efficient to directly do convolution. 
- partial stationarity. 
- [[arithmetic intensity]] 256/17
- benefits more if the kernel is large (7x7). Now smaller kernel is the trend. 

# Speed up MAC: DVAFS
subword parallel variable precision

Dynamic Voltage-Accuracy-Frequency Scaling (DVAFS) MAC. 

![DVAFS](DVAFS.png)
cfr. [[Q L4]] and [[L4_Camus.pdf]]. 

| name     | P                                                | note                  |
| -------- | ------------------------------------------------ | --------------------- |
| baseline | $\alpha CfV^2$                                   | nothing               |
| DAS      | $\frac{\alpha}{K_0} CfV^2$                       | reduce switching      |
| DVAS     | $\frac{\alpha}{K_1} Cf\frac{V}{K_2}^2$           | CP shorter            |
| DVAFS    | $\frac{\alpha}{K_3} C\frac{f}{N}\frac{V}{K_4}^2$ | change f for parallel | 

# Utilizing sparsity

![[sparse_envision.png]]