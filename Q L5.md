# Question 19 

> What is neural network sparsity and how can it improve energy efficiency and/or throughput in neural network processing?

1. Network is sparse means that
	1. Structured sparsity can be described as *generalized* sparsity, or, *hierarchical* sparsity (borrowed from cache hierarchy). It is sparse in a higher level, but it is dense in a lower level. Thus, structured sparsity can give benefit to HW efficiency (property of dense network), while maintaining a decent model size (property of sparse network). 
	2. Sometimes applying sparsity can improve accuracy of network because adding more constraints means regularization, avoiding stucked into local minima. That is also the answer to *why STOA networks are not as compressable as older networks*. 
3. Sparsity can be imposed by such methods (while not hurting accuracy too much): 
	1. Common sparsity: regularization during training (bad after training) #todo plot
	2. Structured sparsity: Lasso regularization
4. Sparsity (broad sense) can be utilized to increase IO BW and reduce processing energy (sparse processors)
	- dense CNN processor + MAC gating and DRAM compression: [[CHIP Envision#Utilizing sparsity by Huffman encoding]]. 
		- Overhead as common dense CNN processor. 
		- No benefit to TP while underutilizing HW. 
	- sparse CNN processor: [[CHIP EIE]]. Send only nonzero inputs and non-zero outputs to MACs. Utilize all MACs while preserving many book keepings. **There exists tradeoff between overhead introduced by book keepings and speed up w.r.t. utilization of MACs.** 
		- Overhead bigger than common dense CNN processor. 
		- Benefit more when huge sparsity. 
5. Structured sparsity can be more beneficial. #todo comparison plot

# Question 20
> How can one analytically estimate the energy for executing a given neural network layer on a given hardware architecture? (cfr. [[#Question 22#Example for loop]])

1. MAC energy : number of MACs \* energy per MAC (depends on technology and width of MAC) 
2. Memory access energy 

# Question 21

> How can one analytically estimate the latency for executing a given neural network layer on a given hardware architecture? (cfr. [[#Question 22#Example for loop]])

(both this and how to compute BW are not going into details)

1. Spatial utilization
	- Programming array fully mapped. In the example, we are using 256 MACs, so compared to the MACs (or PEs) in the system, we can say whether the system is utilized or not. 
2. Temporal utilization 
	- data loading: How much data I need to feed into MACs in one CC? Is that enough for BW/size of memory hierarchy?
	- stalling cycles: Do my MACs need to wait for the memory? It depends on BW. 

# Question 22

> What is the impact of changing the relative order of the for-loops and their memory allocation on this energy and/or latency model?
> Exercise: given a set of nested for-loops and a possible memory allocation scheme, discuss required memory sizes and potential scheduling or bandwidth optimizations.


## Example for loop

```C
// === L3 DRAM === //
for (k1 = 0 to K/K2-1)           // for each output channel
	for (c = 0 to C-1)             // for each input channel
// === L2 SRAM, within chip === //
		for (y = 0 to Y-1)           // for each input row
			for (x1 = 0 to X/X2-1)     // for each input column
				for (fy = 0 to FY-1)     // for each kernel column
					for (fx = 0 to FX-1)   // for each kernel row
// === L1 RF, within processing array (PE) === //
						parfor(x2 = 0 to X2-1) // for input column  (spatially unrolled)
						parfor(k2 = 0 to K2-1) // for output column (spatially unrolled)
							o[k][x][y] += i[c][x+fx][y+fy] * w[k][c][fx][fy]
```

| type | output size                 | input size           | weight size                |
| ---- | --------------------------- | -------------------- | -------------------------- |
| DRAM | $K\cdot X\cdot Y$           | $C\cdot X\cdot Y$    | $K\cdot C\cdot FX\cdot FY$ |
| SRAM | $K2\cdot X2\cdot X1\cdot Y$ | $X2\cdot X1\cdot Y$  | $K2\cdot FX\cdot FY$       |
| RF   | $K2\cdot X2$                | $X2\cdot FX\cdot FY$ | $K2\cdot FX\cdot FY$       | 

comments: 
- ($K2=16$, $X2=16$)
- The question is, does the 256-MAC operation per CC get sufficient data? 
- What spatial and temporal reuse of this for loop? 
- #todo 我觉得RF错了， input size=X2, weight size=K2

## alternative for loop 1

```C
// === L3 DRAM === //
for (k1 = 0 to K/K2-1)           // for each output channel
	for (c = 0 to C-1)             // for each input channel
		for (y = 0 to Y-1)           // for each input row
			for (x1 = 0 to X/X2-1)     // for each input column
// === L2 SRAM, within chip === //
				for (fy = 0 to FY-1)     // for each kernel column
					for (fx = 0 to FX-1)   // for each kernel row
// === L1 RF, within processing array (PE) === //
						parfor(x2 = 0 to X2-1) // for input column  (spatially unrolled)
						parfor(k2 = 0 to K2-1) // for output column (spatially unrolled)
							o[k][x][y] += i[c][x+fx][y+fy] * w[k][c][fx][fy]
```

| type | output size          | input size           | weight size                |
| ---- | -------------------- | -------------------- | -------------------------- |
| DRAM | $K\cdot X\cdot Y$    | $C\cdot X\cdot Y$    | $K\cdot C\cdot FX\cdot FY$ |
| SRAM | $K2\cdot X2\cdot X1$ | $X2\cdot X1$         | $K2\cdot FX\cdot FY$       |
| RF   | $K2\cdot X2$         | $X2\cdot FX\cdot FY$ | $K2\cdot FX\cdot FY$       |

comments:
- smaller SRAM, bout more access to DRAM. 
- If the SRAM on chip is not big enough to store the original version of topology. *Only now it is feasible to be implemented!*
- Cost energy, possibly stalling

## alternative for loop 2

```C
// === L3 DRAM === //
for (k1 = 0 to K/K2-1)           // for each output channel
	for (c = 0 to C-1)             // for each input channel
// === L2 SRAM, within chip === //
		for (y = 0 to Y-1)           // for each input row
			for (x1 = 0 to X/X2-1)     // for each input column
// === L1 RF, within processing array (PE) === //
				for (fy = 0 to FY-1)     // for each kernel column
					for (fx = 0 to FX-1)   // for each kernel row
						parfor(x2 = 0 to X2-1) // for input column  (spatially unrolled)
						parfor(k2 = 0 to K2-1) // for output column (spatially unrolled)
							o[k][x][y] += i[c][x+fx][y+fy] * w[k][c][fx][fy]
```

| type | output size                 | input size           | weight size                |
| ---- | --------------------------- | -------------------- | -------------------------- |
| DRAM | $K\cdot X\cdot Y$           | $C\cdot X\cdot Y$    | $K\cdot C\cdot FX\cdot FY$ |
| SRAM | $K2\cdot X2\cdot X1\cdot Y$ | $X2\cdot X1\cdot Y$  | $K2\cdot FX\cdot FY$       |
| RF   | $K2\cdot X2$                | $X2\cdot FX\cdot FY$ | $K2\cdot FX\cdot FY$       | 

#todo 我觉得这个是对的

## conclusion of loop juggling

- loop splitting (cfr. tiling)
- spatial loops (change `parfor` loops): data reuse
- loop reordering: stationarity
- part of them is done by compiler #todo


# Question 23

> How does HW-aware neural network design works and what are the challenges? Use the insights from [[L5_MNASnet.pdf]] to illustrate this.

Challenges: 
- Still HW unaware in AutoML: The number of operation maybe is not the thing that hurts most. 
	- matbe because of the poor design of loss function or reward in RL
	- follow up: MNASNet (combine phones or actual HW into the loop)
		- ahrd to model real hardware because there are some other tasks run simultaneously in an actual phone.
		- Still, search space is huge, only Google has such huge compute power

Also see gurst lecture. 
