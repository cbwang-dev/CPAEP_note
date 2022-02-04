Introduction	
- attain $(V_{dd},f)$ in LUT. 
- set by OS (scheduler)
- open loop system

Disadvantages (regarding to LUT and open loop system)
- not taking runtime slow variations into account (ms)
	- I just run a big rendering job, the chip is still hot, but the workload is not big
	- another heavy loaded core's temperature affect this core's temperature
	- *fix: closed loop system* such as Intel Turbo Boost
- too slow for fast variations (ns)
	- resulting into potential error, system not working anymore. Illustrated below.

![[fast_variation.png]]

![[ff_fast_variation.png]]
From this figure, we can see that the delay is longed because of the drop of voltage, resulting into malfunctioning of the circuit. 

![[margin.png]]
We can take margin to alleviate this problem. 
- more margin in low power mode because delay is sensitive to supply voltage, we need more guard
- power overhead, performance reduction

