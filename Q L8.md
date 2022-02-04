# Question 28

> How do DVFS and AFS work, what is the difference between them and what are their advantages/disadvantages?

1. Why we need to scale the frequency?
	- at run time, scheduler send different jobs to different processors based on deadlines, orders, etc.
	- It is hard to predict these workloads in design time, so there might be some drastic variations between different processors
	- we need to guarantee it works in worse scenario
	- scaling frequency, also scale voltage down (utilize critical path), energy consumption down ($P=\alpha CfV_{dd}^2$)
2. [[DVFS]]: If I have this much work, then I will run this core at this frequency and this supply voltage. 
3. [[adaptivity#for fast changes adaptive clocking]]: If I found the voltage is drooping, then the clock period is prolonged. 
	

# Question 29

> What is the difference between resilient and adaptive designs? Give and explain an example of both. Are they sometimes combined?

1. [[resiliency]] design: *tolerating errors*
2. Adaptive design: *avoiding errors*
	- [[adaptivity#for slow changes AFS and instruction throttling]] 
	- [[adaptivity#for fast changes adaptive clocking]] 
3. adaptive clock distribution ([[ACD]]) combined these two methods. 

# Question 30

> What is the difference between EDS and TRCs, and how can they be used in a pipelined processor? What are their advantages / disadvantages? Explain their different performance (L8, slide 29).

Discussed in [[resiliency]]. Summary:
- Differences:
	- 
	- 

# Question 31

> What can be done to overcome very fast voltage droops? 
> a) For a fixed supply voltage ➔ Explain adaptive clocking.
> b) Exploiting also supply control ➔ Explain UVFR: also use the L8_Zimmer paper to illustrate.