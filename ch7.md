
### Heap

`calloc()` --  is like malloc, but clears the memory to zero before giving you the pointer.
`realloc()` --  changes the size of a block of memory pointed to, either growing or shrinking it, *often* by copying the contents somewhere else and giving you back a pointer to the new location. Don't assign the return value from `realloc()` directly to the character pointer; if the reallocation fails, it will nullify the pointer, and you'll lose the reference to the existing table!

The end of the heap is marked by a pointer known as the "break".Your programs will "break" if they reference past the break. When the heap manager needs more memory, it can push the break further away using `brk()` and `sbrk()`. 

> **Take Care**
> In a **`for (p = start; p; p = p->next)`** loop, a **`free(p)`** is done in the loop body, which leads a freed pointer to be dereferenced on the next loop iteration, with unpredictable results