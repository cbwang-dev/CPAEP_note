

# GPU Optimization
GPU core designs achieve high throughput by aggressively employing single-instruction multiple-data (SIMD) processing to pack cores densely with arithmetic logic units (ALUs). Modern designs range from 4-wide SIMD execution (ARM Mali) to 16-wide (Imagination PowerVR) and 32-wide (NVIDIA Tegra) configurations. Designs also mix wide SIMD execution with limited superscalar (or very long instruction word) execution to achieve additional parallelism. For example, an Imagination 7-Series GPU core decodes up to two single-precision (fp32) instructions per clock and executes each on a 16-wide group of SIMD ALUs. Similarly, each of the two cores in the NVIDIA Tegra X1 GPU execute up to four 32-wide fp32 instructions per clock.

# 2D SIMD