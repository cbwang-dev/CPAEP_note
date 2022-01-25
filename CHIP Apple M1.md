Big core: Firestorm
- The width of this core (8-wide decode block) is wider than other designs in the industry. 
	- x86 CPUs today still only feature a 4-wide decoder designs that is seemingly limited from going wider at this point in time due to the ISA’s inherent variable instruction length nature, making designing decoders that are able to deal with aspect of the architecture more difficult compared to the ARM ISA’s fixed-length instructions.
- reorder buffer is 630 instruction range deep. 
- cache hierarchy
	- huge cache
	- fast cache

[source](https://outline.com/WL6YHF)