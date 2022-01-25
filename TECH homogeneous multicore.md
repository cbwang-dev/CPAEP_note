Aim: general purpose processors making exact copies of a single core. 
Intel
 

- Each cores runs separate threads
- OS assigns threads to cores (see L6!)
- Memory management and coherence is key!
	- Distributed memory architecture (typical in asymmetric multi-processors, AMP): each operate in isolation, little communication, explicitly processed, not common
	- Shared memory (typical in [[TECH SMP]]): share memory address space, can have some split and some shared cache levels. Coherent caching through bus snooping, or directory based protocols, most common

thermal: 
Each core runs at relaxed frequency to keep thermal budget within limit!
Unused cores ([[EVAL Amdahl's Law]]) are put to sleep
Play with nb of active cores & their frequency (dim and dark silicon) to exploit total thermal power budget in active cores, e.g. Intel's Turbo Boosting (see also lecture 8!)

Examples of chip design: