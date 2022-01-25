# General GeMM workload

[[TECH parallelization (spatial)]]: weight and input reuse
[[TECH stationarity (temporal)]]: output stationarity
[[EVAL arithmetic intensity]] 256/32

# Speed up convolution better than GeMM

For convolution, it is less efficient to transform it into GeMM, but it is efficient to directly do convolution. 
- partial stationarity. 
- [[EVAL arithmetic intensity]] 256/17
- benefits more if the kernel is large (7x7). Now smaller kernel is the trend. 

# Speed up MAC: DVAFS
subword parallel variable precision

Dynamic Voltage-Accuracy-Frequency Scaling (DVAFS) MAC. 

![DVAFS](DVAFS.png)
cfr. [[Q L4]] and [[L4_Camus.pdf]]. 

# Utilizing sparsity by Huffman encoding