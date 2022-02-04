Parallelization is for optimizing spatial unrolling. It reduces MEM/RF energy and BW needs by reusing some of the data that is used more than one time. 

![Datapath illustration](datapath.png)

In the simple `parfor` scenario, TP gain if BW sufficient for feeding these MACs, but not too much energy gains. This does not hold true after exploiting parallelization by possibly sharing data in these MAC elements. 
 
![Spatial data reuse](parallelization.png)
- Sidenote 1: FMA stands for Fused Multiply Add)
- Sidenote 2: Not all neural nets can exploit these parallelism. 
- Sidenote 3: We can combine these parallization schemes together (in the examples)
- Sidenote 4: Only concerning per layer, recently we have layer fusion techniques. 

| Weight reuse       | Input reuse                | Output reuse              |
| ------------------ | -------------------------- | ------------------------- |
| Iterate in batches | Iterate in output channels | Iterate in input channels | 

By doing parallization, one can improve [[arithmetic intensity]]. 

Examples:
- [[GPU#Nvidia Tensor Cores]]: reuse input, output and weights
- [[AMX#Intel AMX]]: 
- [[Tesla NPU]]: 96 by 96 MAC array 
- [[CHIP Huawei DaVinci]]: 3D spatial unrolling, best [[arithmetic intensity]]. 