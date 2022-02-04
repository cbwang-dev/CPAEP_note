# for slow changes: AFS and instruction throttling

instruction throttling: If I detect an error, I will replay in a slower pace 
- instead of executing 32 instructions per CC, do 24 instead
- fine grained than AFS 

adaptive frequency scaling: If I detect an error, I will replay in a lower frequency
- a D FF can half the frequency. 


# for fast changes: adaptive clocking

Leaf clock changes w.r.t voltage. 

| voltage | error | leaf clock |
| ------- | ----- | ---------- |
| low     | yes   | stretch    |
| high    | no    | back       | 
