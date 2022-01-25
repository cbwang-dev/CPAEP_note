# Nvidia Tensor Cores

Details can be found in [[TECH GeMM#GeMM Details]]. 
- In a tensor core, 4-way input reuse, 4-way output reuse, and 4-way weight reuse -> $4\cdot 4\cdot 4=64$ MAC operations. 
- BW: $4\cdot 4\cdot 3=48$, so [[EVAL arithmetic intensity]]=64/48. 