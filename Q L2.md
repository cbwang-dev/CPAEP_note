# Question 6

> Comment on the trend in ISP processors, from fixed function to programmable and the consequences on processing architectures in this domain. What are the main characteristics that distinguish these from CPU’s? Use the example of the Google Pixel visual core.

Trend in ISP processors: (for embedded computational photography tasks)
- Limited set of basic functions and are more likely to become parallelizable. 
- Examples of functions (from RAW to RGB): demosaicing, deblurring, dynamic range correction, removing broken pixels...
- hardwired: no flexibility, fixed to one single application, but fast. 
- programmable: 
	- massively exploiting parallelism
	- increase flexibility with limited function coverage. 
	- dedicated buffers and dataflows. 
- (cfr. [[Google Pixel Visual Core]])

Differences from ISPs to CPUs: 
1. massively exploiting parallelism and pipelining, has gradually increased flexibility, but still remains limited function coverage
2. uses dedicated buffer and dataflow.


# Question 7

> Explain the workload of embedded rendering tasks and how this leads to new GPU processing architectures beyond CPU’s. What are the main characteristics that distinguish them from CPU’s?

cfr. [[GPU#Common GPU Design]] 

workload of embedded rendering tasks: (details explained in [[GPU#GPU Workload]]) 
1.  Primitives (vertex) fetching from memory
2.  Vertex Shading: assign each primitive a spot in the space (in the screen) and modify the shape (vectorial operations)
3.  Rasterization: assign pixels to the all the vertexes
4.  Pixel Shading: assign a color to pixel based on important/heavy computations (texture, light)
5.  Rendering output: display just those pixels that are actually part of the point of view.

how this leads to new GPU processing architectures beyond CPU’s: Maximize throughput under power and area limitation
1. Such workloads can be highly parallelizable in different threads, but for each execution thread there might be conditional jumps, whose outcome differ between units (SIMT). Also a common trend in SIMD cores is to make space to processing unit reducing local memories. This may cause stalls for lack of available data, so it’s possible to divide PE in warps or even implement a fine grain multi-threading to avoid stalling execution of the whole SIMD group.
2. GPUs can be seen as SIMT multi-core with dedicated caches and dataflows between them that deliver a high level parallelism of execution reaching multiple TOPs.
3. In modern GPUs there are also other special function units dedicated for ROM tables and different type of PEs for less used operations (viewpoint transform, tessellation).

Differences from GPUs to CPUs:
1. remove components (data cache, out of order control logic, branch predictor, memory pre-fetcher, ...) that help single instruction stream run faster. This is the root design criterion of GPUs. 

# Question 8

> What additional optimizations are introduced in mobile GPU’s? (from class and [[L2_Fatahalian.pdf]]? 

For GPU design, we want both fruitily resourced and bandwidth efficient processing (cfr. [[GPU]]). Main optimization points are listed below. 

1. For GPU programmable resources: 
	1. wide SIMD execution (cfr. [[SIMD#GPU Optimization]])
	2. accelerated half-precision floating point (`fp16`): enough for shading operations. 
	3. multithreaded execution (cfr. [[multithreading]])
2. Bandwidth efficient 3D graphics (optimize data buffering and data flow to reduce off-chip access)
	1. tiled rendering 
		- *reduce memory bandwidth with utilizing temporal locality* 
		- (the most notable difference between current desktop and mobile GPU implementations) 
		- (cfr. [[rendering#Tiled Rendering to Reduce Memory Bandwidth]])
	2. hardware-accelerated data compression
	3. rendering vertexes before rasterization + aggressive discarding of rendering work (cfr. [[rendering#Aggressively Discarding]])
3. Tightly integrate GPU and CPU (cfr. [[Q L8]]) in order to reduce memory traffic from external memory.
	1. on the same chip (UMA, [[Apple A13]])
	2. shared memory space (coherent memory) and on-chip buffers
4. Mobile GPUs also perform additional work-elimination optimizations such as skipping updates to pixels that don’t change from frame to frame (for example, ARM’s memory transaction elimination).

# Question 9

> How does a layer of a typical neural network look like, and why did hardware concerns cause a shift from FC to CNN layers? #todo

From hardware perspective, FC layer requires too much parameters. For a layer with input size $M^2$ and output size $N^2$, this layer requires $M^2N^2$ parameters (weights), thus not suitable for memory. (cfr. [[Q L3#Question 12]])

However, CNN reusing data. 

flexible post processing (pooling, normalization, ReLU, ...) (cfr. [[Tesla NPU]]). 

# Question 10

> (After having studied L3) How can different types of CNN layers differ, and why can something be efficient from an algorithm point of view and at the same time inefficient from a HW point of view?

Theoretically, algorithm efficiency is defined by by measuring complexity, in big O notation. 

| Notation    | Name                             |
| ----------- | -------------------------------- |
| $O(1)$      | use a constant size LUT          |
| $O(\log n)$ | binary search                    |
| $O(n)$      | find an item in an unsorted list |

- 算法不考虑数据流动所造成的开销
- 算法不考虑是否能够并行
- 算法不考虑overhead
- algorithm point of view: CNN is better than FC
- hardware point of view: data exchange, utilizing MAC (reuse and stationarity)