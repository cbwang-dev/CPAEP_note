# Question 6

> Comment on the trend in ISP processors, from fixed function to programmable and the consequences on processing architectures in this domain. What are the main characteristics that distinguish these from CPU’s? Use the example of the Google Pixel visual core.

- hardwired: no flexibility, fixed to one single application, but fast. 
- 
- (cfr. [[Google Pixel Visual Core]])

Differences from CPUs:


# Question 7

> Explain the workload of embedded rendering tasks and how this leads to new GPU processing architectures beyond CPU’s. What are the main characteristics that distinguish them from CPU’s?

# Question 8

> What additional optimizations are introduced in mobile GPU’s? (from class and [[L2_Fatahalian.pdf]]? 

For GPU design, we want both fruitly resourced and bandwidth efficient processing (cfr. [[GPU]]). Main optimization points are listed below. 

1. For GPU programmable resources: 
	1. wide SIMD execution (cfr. [[SIMD#GPU Optimization]])
	2. accelerated half-precision floating point (fp16): enough for shading operations. 
	3. multithreaded execution (cfr. [[multithreading]])
2. Bandwidth efficient 3D graphics (optimize data buffering and data flow to reduce off-chip access)
	1. tiled rendering to reduce memory bandwidth
	2. hardware-accelerated data compression
	3. aggressive discarding of rendering work (cfr. [[rendering#Aggressively Discarding]])
3. Tightly integrate GPU and CPU (cfr. [[Q L8]])
	1. on the same chip
	2. shared memory space (coherent memory) and on-chip buffers

# Question 9

> How does a layer of a typical neural network look like, and why did hardware concerns cause a shift from FC to CNN layers? 

From hardware perspective, FC layer requires too much parameters. For a layer with input size $M^2$ and output size $N^2$, this layer requires $M^2N^2$ parameters (weights), thus not suitable for memory. 

CNN reusing data. 

flexible post processing (pooling, normalization, ReLU, ...) (cfr. [[Tesla NPU]]). 

# Question 10

> (After having studied L3) How can different types of CNN layers differ, and why can something be efficient from an algorithm point of view and at the same time inefficient from a HW point of view?


- FC layer
- CNN
- Pooling
- Activation functions
	- easy for HW: 

Theoretically, algorithm efficiency is defined by by measuring complexity, in big O notation. 

| Notation    | Name                      |
| ----------- | ------------------------- |
| $O(1)$      | use a constant size LUT |
| $O(\log n)$ | binary search             |
| $O(n)$      |       find an item in an unsorted list |                    |

算法不考虑数据流动所造成的开销，算法不考虑是否能够并行。