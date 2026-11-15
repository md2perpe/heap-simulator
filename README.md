# Heap Simulator

An interactive, single-page visualisation that compares four memory allocators side by side. The same sequence of `malloc` and `free` calls is run through each allocator over a 16 KiB heap, so you can see how each one handles fragmentation and overhead. The UI is in Swedish.

## Allocators

| Allocator | How it works |
| --- | --- |
| **First-fit** | Free list in address order with 16-byte headers. Takes the first free block that fits, splits it, and coalesces free neighbours. |
| **Best-fit** | Same heap and headers, but picks the smallest free block that fits. |
| **Buddy** | Power-of-two blocks from 16 B to 16 KiB. Blocks are halved until they fit and merged with their buddy on free. No in-heap header. |
| **Size classes** | Each 1 024-byte page serves one size class (16, 32, 48, 64, 96, 128, 192, 256, 384, 512 B) split into equal slots. Larger requests get whole pages. No in-heap header. |

## Usage

No build step or dependencies. Open [index.html](index.html) in a browser. (Web fonts are loaded from Google Fonts but the page falls back to system fonts offline.)

- **malloc**: allocate the given number of bytes in all four allocators.
- **free slumpvis**: free a random live allocation.
- **Arbetslast** (workload) + **Kör** / **Ett steg**: run or single-step a generated workload. Speed and fill target are adjustable.
  - Mixed sizes, small objects only, hole punching, stack (LIFO), growing buffers.
- **Töm heapen**: reset all heaps.
- Hover a block for details; click it to free that allocation in all four allocators.

Each map row is one 1 024-byte page. Colours show requested bytes, headers, padding (internal fragmentation), free space, and free slots in a size class. Per-allocator stats show the totals and the largest free block.

## Model simplifications

- Free-list payloads are rounded up to 16 bytes, with a 32-byte minimum block.
- Buddy and size-class metadata is tracked outside the heap.
- No thread caches, multiple arenas, or returning memory to the OS.
