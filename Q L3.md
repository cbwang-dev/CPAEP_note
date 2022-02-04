# Question 11

> What is the difference between spatial and temporal unrolling of for-loops, and how does it impact the arithmetic intensity?

![temporal and spatial unrolling](unrolling.png)

- "Spatial" refers to the MACs available to do parallel computation, while "temporal" means one single step requires one clock cycle. 
- in the `parfor` loop, we can use parallel hardware to execute MAC. 
	- TP gain if BW sufficient, but not too much energy gains. This does not hold true after exploiting [[parallelization (spatial)]] by possibly sharing data in these MAC elements. 
- [[arithmetic intensity]] cfr. [[Q L5#Question 22]]. 
- [[parallelization (spatial)]] and [[stationarity (temporal)]] unrolling optimization can be conducted. 

| unrolling | utilization     | parfor  | name       |
| --------- | --------------- | ------- | ---------- |
| temporal  | stationarity    | outside | stationary |
| spatial   | parallelization | inside  | reuse      | 

# Question 12

> What is GeMM and how its execution improved in GPU’s and (future) CPU’s? Can convolutions also be executed like this? Are there any disadvantages to this for CONV? 

- What is GeMM: [[GeMM#Introduction]]. 
- How its execution improved in GPU’s and (future) CPU’s: 
	- [[GeMM#GeMM on CPU]] and next step [[AMX#Intel AMX]]
	- Next step in GPU: [[Jetson Xavier]] 
- Disadvantage of [[GeMM#CNN using GeMM]]: less efficient execution for reusing same data.
	- Limited reuse / parameter (eg. only 4x in [[GPU#Nvidia Tensor Cores]]), resulting in limiting achievable [[arithmetic intensity]]. 
	- If using 2D GeMM acceleration for CNN (like [[AMX#Intel AMX]] and [[Jetson Xavier]]), CNN can only be used by `im2col`, and is no batching, then it is less efficient besause I have to copy same values into this large matrix. Low utilization if K, resp B.X.Y < 96, data repetition. 

# Question 13

> How can the concepts of spatial and temporal data reuse be applied at higher abstraction levels to further improve processing efficiency and throughput of neural network processing?

- temporal reuse in higher levels
	- high level parfor in different chips.
	- local *storage*: multi-level stationarity
	- hierarchical temporal reuse: *tiling*
- spatial reuse in higher levels
	- multi core processors, parfor loops are repeated at higher loop level (*orthogonal* to low level parallelism)

# Question 14

> Explain what stationarity, parallelism and sparsity opportunities are exploited in: ([[L3_Moons.pdf]], [[L3_TPU.pdf]], [[L4_Binareye.pdf]], [[L4_inMemCompute.pdf]], [[GPU#Nvidia Tensor Cores]])? 

- [[Envision]]
- [[Google TPU]]
- [[BinarEye]]
- [[GPU#Nvidia Tensor Cores]]
- [[in memory computing]]