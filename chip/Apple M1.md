Big core: Firestorm (even bigger and wider)
- The width of this core (8-wide decode block) is wider than other designs in the industry. 
	- The M1 has 8 instruction decoders while every Intel processor maxes out at 4. This enables many more instructions in flight than on Intel: *This is an oversimplified statement.* Extra decode width only helps if the rest of the CPU can actually process so many instructions in parallel without blocking on other resources like memory. x86 CPUs also have their own features to alleviate the limited decode width such as micro-op caches.
	- x86 CPUs today still only feature a 4-wide decoder designs that is seemingly limited from going wider at this point in time due to the ISA’s inherent variable instruction length nature, making designing decoders that are able to deal with aspect of the architecture more difficult compared to the ARM ISA’s fixed-length instructions.
- reorder buffer is 630 instruction range deep. (cfr. [[superscalar]])
- cache hierarchy
	- huge cache: Apple has confirmed that it’s actually a massive 192KB L1 instruction cache. That’s absolutely enormous and is 3x larger than the competing Arm designs, and 6x larger than current x86 designs, which yet again might explain why Apple does extremely well in very high instruction pressure workloads, such as the popular JavaScript benchmarks.
	- fast cache
	- hUMA! 
		- 16 GB DRAM in package
		- speed benefit is large
		- different from [[CUDA]] unified memory (virtually unified, migrates data without programmers noticing)

[source](https://outline.com/WL6YHF)