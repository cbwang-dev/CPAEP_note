# Question 15
 
> How can processors improve energy efficiency as well as throughput by playing with the precision of the MAC unit? Is it also possible to allow runtime precision scaling? How?

1. Reducinig precision -> Reduced MAC operation energy
	- Reducing precision may impact the resulting accuracy of the network, but there are several trick to improve it, using HW training, mixed precision along the network, asymmetrical “scaling” between activation and weights.
2. Reducing precision -> more PE (processing element) on chip or with multi-gating way like Envision -> more data reuse opportunity/more data in same memory space -> reduce memory access energy.
	- More PE on chip means also an higher level parallelism and increasing the throughput even if keeping same latency (not if all data is already in low level memories!).
3. Runtime precision scaling is possible with special technique for example the multi-gating way which is present in [[Envision#Speed up MAC DVAFS]], but also in Sirius(slide 15), the add and gated shift (slide 16), and the bit serial way (slide 17). There are several of such techniques in literature that combine the run time precision scaling (shorter critical paths and more relaxed timing constraints) with DVS.

# Question 16

> What are the opportunities that come from extreme quantization (binary/ternary/...) in the digital, mixed-signal and memory domain?

cfr. [[BinarEye]]

1. Some of the applications can use binary quantization. 
2. Digital domain, binary quantization saves energy efficiency and area smaller
	1. simplify multiplication to XNOR
3. However, adder too large, so switch into mixed-signal domain (near memory computing)
	1. pop counter is the most expensive part. 
	2. Weights and activations are binary, but internal counter needs higher precision. 
	3. use comparator to replace digital adder+activation (switch cap circuit)
	4. noise, stochastic, loss of accuracy
4. binary quantization can be applied to in memory computing

# Question 17

> What are near- and in-memory processing, and why are not all processors exploiting this? What are the remaining challenges?

Near memory computation can be reassumed as a way of designing a chip in a way memory units will be used specifically for supporting a specific operation (high band width) and not for general storing purpose. Such memories can be on-chip or even stand alone with enhanced functionality. To support more this design philosophy there are increasing number of chips that exploit 3D integration to stack memory space on top of logic.


find the bottleneck. 有的东西的读取不是瓶颈

analog+parallelism 

Why not?
- accuracy not guaranteed because of inherent noise, so precise at low quantization level
- benefits at array level, not system level efficiency (weight not that much)
- not all NN layers exploit large arrays

#todo in memory compute 重新搞一下

# Question 18

> What are the dataflow and algorithmic consequences (opportunities and challenges) of adapting the MAC precision? (also use content of L5 for this question)
