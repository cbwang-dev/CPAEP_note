![[A13.png]]

Summary: 
- 2 big + 4 little heterogeneous design
- CPU, GPU, ISP (ordering with decreasing flexibility)
- Unified memory. 

Details and differences:
- CPUs:
	- big: Lightning, 2.6GHz
		- Based on ARM 
		- 7-wide OOO decode front-end 
		- 6 ALUs and 3 FP/vector pipelines 
		- [[AMX]]: machine learning extensions (see L2/3) 
	- (not so) little: Thunder, 1.7GHz    
		- 3-wide OOO decode
		- 2 main execution pipelines, working alongside L/S units
	- For small workload, small cores are used in a scaled frequency, thus less energy is consumed. If small workload is added on big cores with scaled frequency, the energy efficiency is lower than small cores. This is an advantage of heterogeneous multicore design. 
- GPU #todo
- ISP
	- more parallelism and pipelining + dedicated buffers and datapath, while with limited flexibility

      

Why all there: *trade energy efficiency with area, while boosting performance*

Moore’s law again gave us more transistors, CPU could not exploit it, as most workloads did not benefit from more cores, or more within core parallelism, this would result in vast core under-utilization, so we can use them for something else. Considering some applications always perform the same functions and have humongous inherent parallelism, we need hardcode functions in logic, and massive parallelism respectively for such kind of scenario. So, we add additional “domain specific” cores (GPU, ISP,…) to boost performance and turn them on when needed.