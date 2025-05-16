# Memory Allocator (`vmalloc`/`vmfree`)

This project implements a simplified dynamic memory allocator in C, mimicking the behavior of `malloc` and `free`.

## Overview
The allocator manages a simulated heap using memory-mapped pages and supports dynamic memory requests with custom metadata handling, alignment, and block coalescing to mitigate fragmentation.

## Features
- **Custom Heap Initialization**: Heap created via `mmap`, aligned and initialized with a single large free block.
- **`vmalloc(size_t size)`**:  
  - Allocates memory using a **best-fit** strategy.
  - Ensures all allocations are **16-byte aligned**.
  - Supports **block splitting** for efficient memory usage.
- **`vmfree(void* ptr)`**:
  - Frees allocated blocks and **coalesces adjacent free blocks (both directions)**.
  - Uses **footers in free blocks** to coalesce with the previous block without scanning the entire heap.
- **`vminfo()`**:
  - Displays the current heap layout with metadata for each block.
  - Reports the largest available free block by index and size.

## Technical Details

- **Memory Alignment**: All allocations are 16-byte aligned. Block headers are 8 bytes.
- **Block Structure**:
  - Headers store size and status bits.
  - Free blocks include an 8-byte **footer** to support backward coalescing.
- **Pointer Arithmetic**: Direct manipulation of block pointers and offsets using C.
- **Bitwise Operations**: Used to manage block status and metadata efficiently.


## Files
- `vmalloc.c` / `vmfree.c`: Allocation and deallocation logic
- `vmlib.h`, `vm.h`: Public and internal interfaces
- `vminit.c`: Heap initialization using `mmap`
- `utils.c`: Helper and debug functions (e.g., `vminfo`)
- `tests/`: Test drivers and memory layout images

## Example Output

```
root@efs98010:/mnt/c/Users/yuhen/cpp/CSE-29-PA4-Malloc/tests# ./test_free_coalesce_l
vminit: heap created at 0x7f74f7fa4000 (4096 bytes).
vminit: heap initialization done.
---------------------------------------
 #      stat    offset   size     prev   
---------------------------------------
 0      FREE    8        4080     BUSY   
 END    N/A     4088     N/A      N/A    
---------------------------------------
Total: 4080 bytes, Free: 1, Busy: 0, Total: 1
---------------------------------------
 #      stat    offset   size     prev   
---------------------------------------
 0      BUSY    8        80       BUSY   
 1      FREE    88       4000     BUSY   
 END    N/A     4088     N/A      N/A    
---------------------------------------
Total: 4080 bytes, Free: 1, Busy: 1, Total: 2
---------------------------------------
 #      stat    offset   size     prev   
---------------------------------------
 0      BUSY    8        80       BUSY   
 1      BUSY    88       80       BUSY   
 2      FREE    168      3920     BUSY   
 END    N/A     4088     N/A      N/A    
---------------------------------------
Total: 4080 bytes, Free: 1, Busy: 2, Total: 3
---------------------------------------
 #      stat    offset   size     prev   
---------------------------------------
 0      BUSY    8        80       BUSY   
 1      BUSY    88       80       BUSY   
 2      BUSY    168      80       BUSY   
 3      FREE    248      3840     BUSY   
 END    N/A     4088     N/A      N/A    
---------------------------------------
Total: 4080 bytes, Free: 1, Busy: 3, Total: 4
---------------------------------------
 #      stat    offset   size     prev   
---------------------------------------
 0      FREE    8        80       BUSY   
 1      BUSY    88       80       FREE   
 2      BUSY    168      80       BUSY   
 3      FREE    248      3840     BUSY   
 END    N/A     4088     N/A      N/A    
---------------------------------------
Total: 4080 bytes, Free: 2, Busy: 2, Total: 4
---------------------------------------
 #      stat    offset   size     prev   
---------------------------------------
 0      FREE    8        160      BUSY   
 1      BUSY    168      80       FREE   
 2      FREE    248      3840     BUSY   
 END    N/A     4088     N/A      N/A    
---------------------------------------
Total: 4080 bytes, Free: 2, Busy: 1, Total: 3
---------------------------------------
 #      stat    offset   size     prev   
---------------------------------------
 0      BUSY    8        160      BUSY   
 1      BUSY    168      80       BUSY   
 2      FREE    248      3840     BUSY   
 END    N/A     4088     N/A      N/A    
---------------------------------------
Total: 4080 bytes, Free: 1, Busy: 2, Total: 3
```

## Code Sample  
- Code Sample is available upon request: [yuhenglin02042003@gmail.com](mailto:yuhenglin02042003@gmail.com)