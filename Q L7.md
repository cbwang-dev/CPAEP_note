# Question 24

> Why are IP cores so important nowadays and what is the role of RISC-V in this? How can individuals use it in their SoC system, without giving up on customization?

![[./plots/design_gap.png]]

**IP bridges silicon design gap**.  

RISC-V is an open source ISA. 
- It includes definition of register file allocation. 
- Basic everything you need to link code to hardware. 
- not the implementation itself
- allow to share compilers, migrate code, etc. 
- open source cores with different implementations are available
	- easy to deploy (open source RTL or open source HCL, advantage is parametrized code)
	- easy to integrate (often come with complete periphery and system architecture that is open source as well)
	- easy to extend (to multi-core, custom accelerators)



# Question 25

> Discuss the [[L7_GAP8.pdf]] processor diagram as an example of bringing together necessary IP blocks towards performance, yet low power SoCs. What blocks, extensions and modifications where introduced to improve performance and power efficiency?

- RISC_V cores extended for 
	- vector processing
	- SIMD with various data widths and bit manipulations
	- single cycle complex number multiplication
- dedicated accelerators for CNN in same memory system
- shared memory
- HW sync: HW support for low overhead core synchronization
- sleep mode, deep sleep mode with own separate clocks. 

# Question 26

> How can hardware designers make heterogeneous multiprocessing systems based on CPU cores (big.LITTLE) easier to program? How is scheduling and synchronization organized in such systems? (see also [[L7_ARMscheduling.pdf]])

1. make it easier to program on big.LITTLE
	- why hard to program?
	- Methods:
		- code switching: [[HSAIL]]. 
		- data switching: *heterogeneous UMA* (cfr. [[L7_HSA.pdf]]). 
			- Shared memory system (cfr. multi-core and multi-CPU systems) CPU and the GPU (and DSP, NNA, …) *have full access to the entire system memory*. 
			- Can be really shared (cfr. [[Apple M1]]), or just virtually shared (cfr. [[CUDA]]). *No explicit copies by the programmer*. 
			- sidenote: for pure software, you cannot get any pure really shared memory like [[Apple M1]]. This is a constraint of software efficiency w.r.t hardware. 
		- heterogeneous scheduler (cfr. [[L7_HSA.pdf]]). Each processor can assign tasks for itself and the other processors (once the workload ran on GPU is too large to handle, GPU can ask CPU to do this task. Every core can push task to other core’s task queue). More rights to GPUs and DSPs are given compared to the Von Neumann architecture. Heterogeneous / unpredictable workload patterns! This makes many heterogeneous /unpredictable workloads pattern.
	- How is scheduling and synchronization organized in such systems?
		- Use scheduler to schedule which workload should go to which core online. Scheduler is a part of operating system. Have some privilege, every-time there is a new job launching, it has to access a pool of tasks (Ready Queue, tasks list need to be executed, each task in the queue has to be known the real requirements, such as resource needs by monitoring the historical operations/sec needs of this task to get these information, execution time), scheduler will look into the ready queue regularly or when resource frees up, to Find best assignment between tasks and resources (pool of resource). Lecture 8 has some supplements.

scheduling GAP8 hw synchronizer

# Question 27

> What are the challenges in running code on truly heterogeneous systems? And what solutions are proposed to alleviate programming truly heterogeneous systems? What challenges remain?

- Challenges:
	- Every compilation target still needs own (custom written/optimized) “finalizer” (optimized kernel functions)
		- Not easy to add new/own HW accelerators (or e.g. parametrized accelerators)
	- Difficult (impossible) for tools to automatically allocate code to cores
		- Detect / optimize fused kernel opportunities, ...
	- Difficult (impossible) for tools to automatically optimize scheduling across cores
		- Detect / optimize parallelization opportunities, ...
- Solutions:
	- Common principles
		- Find a unified intermediate representation
	- Many initiatives
		- HSAIL (Heterogeneous System Architecture Intermediate Language)
			- 不能自动分配CPU和GPU的内容
			- 很难自己加进去accelerator因为没有相关的语言

---

HS (refers just to HSoC!) contains sets of different compute units, everyone with a different architecture and a different Instruction Set. Compile efficiently a workload is already a complex task, that is the reason why many systems in the past didn’t emerge(see Intel).

Evaluating compute time and workload become also complex for scheduling tasks. 

Moreover each processor will have its own local memory it’s complicated to keep memory consistency and coherence whitin all the cores. Bandwidth between different cores trade-off can also be difficult to balance.

HSA foundation, established by AMD, consists mainly of three paradigms:

-   hUMA gives co-processor full rights and access to the complete memory system, like the CPU. (This is true only in APUs, not in desktop/server systems)
-   hQ allows any processor to assign tasks to other processors.
-   HSAIL is an intermediate general language that can be then compiled to processor specific ISA. Works along with OpenCL SPIR.