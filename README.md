# 4c6ff5
### Keywords
WebGPU, GPU computing, parallel processing, compute shaders, real-time rendering, mouse picking, object selection, triangle intersection, inverse transformation, matrix composition, branchless algorithms, massively parallel, state machines

# Summary
This project demonstrates a novel approach to real-time object picking that achieves unprecedented scale and performance. By inverting the traditional picking problem -- transforming mouse coordinates into triangle space rather than testing complex shapes -- the system enables real-time interaction with 500,000+ triangles at mousemove frequency.

## Key Novel Technique
Instead of complex ray-triangle intersection tests, mouse coordinates are transformed using precomputed inverse matrices to map the picker (for example, mouse cursor coordinates) into a canonical "easy-to-pick" triangle space, reducing all picking operations to simple bitwise comparisons (x <= 1 && y >= 0 && x >= y), which can then be performed in parallel for all triangles to be picked.

## Architecture
1. Initial compute pipeline processes declarative transformation opcodes into forward/inverse matrix pairs (opcode 0x00 → identity/no-op, opcode 0x01 → scale, etc.)
2. Per-frame compute pipeline performs massively parallel picking tests using mouse coordinates
3. Render pipeline applies forward transformations with minimal vertex/fragment work
4. All triangles operate as independent state machines enabling rich interaction semantics (enabling room to handle things like mousedown, mouseover, click, dragover, dragaway states per-triangle, in parallel, entirely on the GPU-side)

## Performance
Tested with 524,288 triangles maintaining real-time mousemove responsiveness (response being the highlighting of mouseover'd triangles) on mid-range hardware (GTX 1650).

## Developer Experience
Opcode-based transformation system eliminates matrix mathematics from application code and the CPU while enabling complex geometric manipulations through simple parametric commands.
