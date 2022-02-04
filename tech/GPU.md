# GPU Workload

![[GPU_workload.png]]

-   **Input Assembly**. The GPU reads the vertex and index buffers from memory, determines how the vertices are connected to form triangles, and feeds the rest of the pipeline.
    
-   **Vertex Shading**. The vertex shader gets executed once for every vertex in the mesh, running on a single vertex at a time. Its main purpose is to **_transform_** the vertex, taking its position and using the current camera and viewport settings to calculate where it will end up on the screen.
    
-   **Rasterization**. Once the vertex shader has been run on each vertex of a triangle and the GPU knows where it will appear on screen, the triangle is **_rasterized_** – converted into a collection of individual pixels. Per-vertex values – UV coordinates, vertex color, normal, etc. – are interpolated across the triangle’s pixels. So if one vertex of a triangle has a black vertex color and another has white, a pixel rasterized in the middle of the two will get the interpolated vertex color grey.
    
-   **Pixel Shading**. Each rasterized pixel is then run through the pixel shader (although technically at this stage it’s not yet a pixel but ‘fragment’, which is why you’ll see the pixel shader sometimes called a fragment shader). This gives the pixel a color by combining material properties, textures, lights, and other parameters in the programmed way to get a particular look. Since there are so many pixels (a 1080p render target has over two million) and each one needs to be shaded at least once, the pixel shader is usually where the GPU spends a lot of its time.
    
-   **Render Target Output**. Finally the pixel is written to the render target – but not before undergoing some tests to make sure it’s valid. For example in normal rendering you want closer objects to appear in front of farther objects; the depth test can reject pixels that are further away than the pixel already in the render target. But if the pixel passes all the tests (depth, alpha, stencil etc.), it gets written to the render target in memory.

# Common GPU Design

![[GPU1.png]]

# Nvidia Tensor Cores

Details can be found in [[GeMM#GeMM Details]]. 
- In a tensor core, 4-way input reuse, 4-way output reuse (FMA, fused multiply add), and 4-way weight reuse -> $4\cdot 4\cdot 4=64$ MAC operations. 
- BW: $4\cdot 4\cdot 3=48$, so [[arithmetic intensity]]=64/48. 
-  it can reduce precision from Fp32 to Fp16, to INT8, INT4, which can bring x8, x16, x32 times throughput improvement respectively, and only one time memory access to fit many different operations, using this way to improve the efficiency per watt.