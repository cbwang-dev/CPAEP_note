# Question 11

> What is the difference between spatial and temporal unrolling of for-loops, and how does it impact the arithmetic intensity?

![temporal and spatial unrolling](unrolling.png)

- "Spatial" refers to the MACs available to do parallel computation, while "temporal" means one single step requires one clock cycle. 
- in the `parfor` loop, we can use parallel hardware to execute MAC. 
	- TP gain if BW sufficient, but not too much energy gains. This does not hold true after exploiting [[TECH parallelization (spatial)]] by possibly sharing data in these MAC elements. 
- [[EVAL arithmetic intensity]] #todo cfr. [[Q L5#Question 22]]. 
- [[TECH parallelization (spatial)]] and [[TECH stationarity (temporal)]] unrolling optimization can be conducted. 

# Question 12

> What is GeMM and how its execution improved in GPU’s and (future) CPU’s? Can convolutions also be executed like this? Are there any disadvantages to this for CONV? 

- What is GeMM: [[TECH GeMM#Introduction]]. 
- How its execution improved in GPU’s and (future) CPU’s: 
	- [[TECH GeMM#GeMM on CPU]] and next step [[TECH AMX#Intel AMX]]
	- Next step in GPU: [[CHIP Jetson Xavier]] 
- Disadvantage of [[TECH GeMM#CNN using GeMM]]: less efficient execution for reusing same data. #todo 解释更完整一点
	- Limited reuse / parameter (eg. only 4x in [[TECH GPU#Nvidia Tensor Cores]]), resulting in limiting achievable [[EVAL arithmetic intensity]]. 
	- If using 2D GeMM acceleration for CNN (like [[TECH AMX#Intel AMX]] and [[CHIP Jetson Xavier]]), CNN can only be used by `im2col`, and is no batching, then it is less efficient besause I have to copy same values into this large matrix. Low utilization if K, resp B.X.Y < 96, data repetition. 

# Question 13

> How can the concepts of spatial and temporal data reuse be applied at higher abstraction levels to further improve processing efficiency and throughput of neural network processing?

# Question 14

> Explain what stationarity, parallelism and sparsity opportunities are exploited in: ([[L3_Moons.pdf]], [[L3_SzeI.pdf]], [[L3_TPU.pdf]], [[L4_Binareye.pdf]], [[L4_inMemCompute.pdf]], [[TECH GPU#Nvidia Tensor Cores]])? 

- [[CHIP Envision]]
- [[CHIP Eyeriss]]
- [[CHIP Google TPU]]
- [[CHIP BinarEye]]
- [[TECH GPU#Nvidia Tensor Cores]]
- [[TECH in memory computing]]