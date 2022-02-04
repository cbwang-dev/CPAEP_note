1. error detection
	- [[EDS]] (error detection sequential) *intrusive*
	- [[TRC]] (tunable replica circuit) *non-intrusive*, conservative, less TP gain
	- comments
		- TRC more conservative, thus less TP gain
		- EDS works bad at low supply voltage (low supply voltage leads to large variation of delay, min delay is so large, so we need to put a lot of inverters, thus inefficient)
2. error recovery
	- [[local recovery]]
		- inject correct value into pipeline
		- stall for one cycle and continue
		- complex, not used
		- works with [[EDS]]
	- [[instruction replay]]
		- invalidate instruction in pipeline
		- replay errant instruction with a higher voltage (or a lower frequency)
		- used in reality, only add one error detection unit and a flush unit
		- works with [[EDS]] and [[TRC]]
3. comments
	- good to solve timing error in single CC
	- not guarantee that it will be better at next CC, leading to problems for long droops
	- as a result, need to combine with run time adaptive behavior during replay to deal with frequency droops or long droops. 