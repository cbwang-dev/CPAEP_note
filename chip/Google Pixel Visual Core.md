Introduction: Can do demosaicing; denoising; other (many) picture enhancement processing *before data hits the DRAM/CPU*

Properties: 
- massively exploiting parallelism: stencil processor (STP) 
	- share data only locally to their neighbors
	- data immediately consumed and sent to next processor
- increase flexibility with limited function coverage. 
- dedicated buffers and dataflows. 

- IPU, co-processor hold HDR+ algorithm
- A dedicated A53 aggregates application layer IPU resource requests and configures appropriately. 
- homogeneous design

![[PVC.png]]
![[stencil.png]]
