# Question 1

> What is Dennard’s law and how is it linked to the evolution of computer architectures over the last 30 years? Describe the different phases we see in this evolution, and what are the architectural consequences. Can you illustrate this with examples from processors we discussed in class, or from the papers to be read?

| Transistor Property | Dennardian | Post Dennardian |
| ------------------- | ---------- | --------------- |
| $\Delta$Quantity    | $S^2$      | $S^2$           |
| $\Delta$Frequency   | $S$        | $S$             |
| $\Delta$Capacitance | $1/S$      | $1/S$           |
| $\Delta$$V_{dd}^2$  | $1/S^2$    | 1               |
| $\Delta$Power       | 1          | $S^2$           |
| $\Delta$Utilization | 1          | $1/S^2$         | 

1. **Dennard’s law**: as transistors get smaller, the power density remains constant, so every generation (1.4 years) *power per transistor* drops by 2x. Details are povided in the above table. $V_{dd}$ is not shrinkable w.r.t the scaling because #todo . 
2. **Consequence of the void of Dennard's Law halted frequency increase** because power $P=CV^2f$. Additional heat will be hard to dissipate, resulting into meltdown of chip. (*$f$ cannot be higher*)
3. With the transistors scaling down, we can do something to use area to buy energy efficiency. 
4. Spending area for more efficiency in **single** cores, including datapath parallelism, [[multithreading]], and using large caches. 
	1. Datapath parallelism: keep more **parallel functional units** busy, to reduce clock speed, at the expense of **area**
		- instruction-level parallelism:
			- Compiler scheduled: in order [[VLIW]]
			- Online scheduled: out of order [[superscalar]]
		- data-level parallelism: 
			- Compiler scheduled: [[SIMD]] (or vector processing)
			- Online scheduled: [[GPU]]
	2. [[multithreading]]: keep more **functional units** busy, to reduce clock speed, at the expense of **register file area**
	3. larger caches: more on-chip memory area, to reduce IO-switching
		- In a highly paralized processor, majority of energy goes into memory load and store, especially off-chip memory. 
		- maximize on chip cache size and multi-level caches on-chip. 
5. Spending area for more efficiency in multiple **cores**
	1. [[homogeneous multicore]] 
	2. [[heterogeneous multicore]] (differentiate through custom cores)
6. Examples of processor design using these techniques
	1. [[Hexagon 680]] for [[VLIW]] and [[SIMD]]. 
	2. [[Intel Core i7]] for [[superscalar]], on chip cache. 
	3. [[Apple M1]] for [[heterogeneous multicore]], [[superscalar]]
	4. [[Apple A13]] for [[heterogeneous multicore]]
	5. [[Cortex A53-A57]] for [[heterogeneous multicore]]

#todo after completion of this course, go back to this question. (doamin specific accelerators)

# Question 2

> What is meant with the 4 horsemen in Taylor’s paper [[L1_Taylor.pdf]], and how does this link to the architectural evolutions we discussed?

1. The necessity for dark silicon. While Moore's Law is not breaking, Dennard's Law is breaking (cfr. [[Q L1#Question 1]]). With the contimuation of scaling (Moore's Law), more and more silicon needs to be dim or dark. Taylor explained what were *these 4 horsemen as four possible scenarios of how architectural evolutions would face the end of Dennard’s scaling*. In the paper he tries to predict pros and cons of each one of these possibilities.
	1. The shrinking horseman. From a cost point of view, shrinking chip area does not bring any benefit since most of the investment in a hardware product is related to non-recurring engineering costs, packaging and market, while just a little share is strictly related to silicon. This would lead to a decreasing interest on new technology node and would stop performance improvements. Also porting a design in a new node and shrinking a chip would increase power density of hotspots. （缩小芯片面积其实不像之前想的那样那么省钱，而且硬缩小面积会让热散不出去）
	2. The dim horseman. Dim silicon means general-purpose logic that is typically *underclocked* or used infrequently to meet the power budget. 
		- Near-Threshold Voltage processors: Although per-processor performance of NTV processors drops faster than the corresponding savings in energy-per- instruction (say *a 5× energy improvement for a 8× performance cost*), the performance loss can be offset by using 8× more processors in parallel if the workload allows it. As we will see with specialization, the more energy-limited the domain (i.e. runs off a small solar panel or battery), the less total parallelism in the workload needed to break even, and thus the broader the applicability across workloads.
		- Bigger caches: increase hit/miss ratio. However, when this is approximately 1 then it is bandwidth limited. 
	3. The specialized horseman. To design specialized core that can bring orders-of-magnitude of improvement in energy-efficiency and performance."Spend area to buy power efficiency". Problems:
		- hard for programmers to interface with hardware complexity
		- [[Amdahl's Law]]: spending area improving of 90% a task that represents just the 1% of the workload is not good investment.
	4. The deux ex machina horseman: fundamental breakthrough in semiconductor devices. Unpredictable. 
2. Link to architectual evolution:
	1. shrinking: 
	2. dim: 
	3. specialized: domain specific accelerators, such as ISP. 

# Question 3

> How does a [[superscalar]] [[multithreading]] processor work, and why is it not the solution for all compute workloads?

1. How super-scalar processor works: (cfr. [[superscalar]]) (trade area for energy efficiency)
	- Super-scalar dumps each instruction in the *reservation station* (instruction queue) of that instruction type. 
	- Instructions waits there till all inputs it needs for processing are available. 
	- It is out of order execution and has to put back in order before commit to memory of RF, it creates big memory and data shuffling cost (*reorder buffer*). 
	- It needs to keep the track which functional units are delivering inputs to next instruction (*scoreboard*). 
2. How multithreading works: (trade area for energy efficiency)
	- for single-threading program, it’s difficult to keep all functional units always busy, due to many data dependency. So for multi-threaded super scalar, first run processor at a slow speed, make it consumes less energy, then make every ALU can be fully used by filling the instruction queues using instructions from multiple programs, running in parallel. We can let the data dependency of one program spreads out, making many cycles in between, these cycles can be used to run other programs’ instructions. Each thread should contains one PC, one register file, one reorder buffer. Super-scalar multi-threaded processor has more complex scoreboard, commit and bypassing units, again spending area for energy efficiency (lower clock speed for same performance).
3. why is it not the solution for all compute workloads?
	- Because for some compute workloads themselves don’t have enough parallelism (based on [[Amdahl's Law]]).
	- The second reason is caused by *limited memory bandwidth*. one processor executes ten times instructions also needs ten times data, which brings big pressures for memory bandwidth. This problem can be alleviated by adding big on-chip DRAM, which is another tradeoff between area and energy efficiency. 

# Question 4

> Discuss the different types of processors seen on an [[Apple A13]] processor. In what sense do they differ? Why are they all there? (after L7: How would they exchange data and processing jobs?)

- Difference: 
	- CPUs: heterogeneous multicore
	- ISP is less flexible than GPU. 
	- Both GPU and ISP remove some of the components in CPU. 
- Why are they all there
	- big and small CPUs: 
	- GPUs:
	- ISPs:

The advantage is "choose the best processor for varying workloads"
#todo what is the difference between ISP and GPU?

heterogeneous UMA

# Question 5

> Apply the different concepts from this class to the Apple M1 processor. Which techniques do they exploit and why? (see paper [L1_Apple](https://outline.com/WL6YHF))

Discussed in [[Apple M1]].