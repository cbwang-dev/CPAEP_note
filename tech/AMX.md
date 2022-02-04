
# Intel AMX

- x86 Advanced Matrix eXtension
- 2D FMA (fused multiply add, output reuse in [[parallelization (spatial)]]) (cfr. [[GPU#Nvidia Tensor Cores]])
- Tiled Matrix Multiply: $T_C[M][N]=T_A[M][K]\cdot T_B[K][N]$
- executed in accelerators net to processor

Downsides:
- limited reuse, limited AI in RF
- if no batching, low efficiency in 2D matrixes