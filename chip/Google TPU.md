Inference chip TPU v1. 

# Main difference: Systolic MAC array (weight (or row) stationarity)

For each MAC, instead of passing data to the main bus or memory, only pass outputs to **neighboring** MACs in the next CC, thus allows efficient matrix multiplications. 

Fully weight stationarity, row (eg. $i_{0,0}$ to $i_{0,4}$) stationarity

# Pipeline Demonstration (GeMM)

![[plots/TPU_1.png]]
![[plots/TPU_2.png]]
- pass inputs to right, and pass result to bottom. 
- 3x3 W-matrix * 3xN I-matrix

# Comments

- It is not a power efficient solution regardless of saving access to memory, because of switching huge amount of registers. 
- Big on chip memory, and lots of data reuse. 

roofline model explaination. 