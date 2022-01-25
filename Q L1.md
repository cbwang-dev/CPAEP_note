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
2. **Consequence of the void of Dennard's Law halted frequency increase** because power $P=CV^2f$. Additional heat will be hard to dissipate, resulting into meltdown of chip. 
3. With the transistors scaling down, we can do something to use area to buy energy efficiency. 
4. Spending area for more efficiency in **single** cores, including datapath parallelism, [[multithreading]], and using large caches. 
	1. Datapath parallelism: keep more **parallel functional units** busy, to reduce clock speed, at the expense of **logic area**
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
5. Spending area for more efficiency in **multiple** cores
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

1. The necessity for dark silicon. While Moore's Law is not breaking, Dennard's Law is breaking (cfr. [[Q L1#Question 1]]). With the contimuation of scaling (Moore's Law), more and more silicon needs to be dim or dark.  
2. The shrinking horseman. 
3. The dim horseman. Dim silicon means general-purpose logic that is typically underlclocked or used infrequently to meet the power budget. 
4. The specialized horseman. "Spend area to buy efficiency". 
5. The deux ex machina horseman. 

# Question 3

> How does a [[superscalar]] [[multithreading]] processor work, and why is it not the solution for all compute workloads?

# Question 4

> Discuss the different types of processors seen on an [[Apple A13]] processor. In what sense do they differ? Why are they all there? (after L8: How would they exchange data and processing jobs?)

The advantage is "choose the best processor for varying workloads"

# Question 5

> Apply the different concepts from this class to the Apple M1 processor. Which techniques do they exploit and why? (see paper [L1_Apple](https://outline.com/WL6YHF))

Discussed in [[Apple M1]]. Summary: 
1. 