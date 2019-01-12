
### Segment

> * Data segment contains the initialized global and static variables, complete with their assigned values. 

> * BSS(Block Started by Symbol) segment --  The size that BSS will require at runtime is recorded in the object file, but BSS (unlike the data segment) doesn't take up any actual space in the object file. The size of the BSS segment is then obtained from the executable, and the loader obtains ablock of this size, putting it right after the data segment. This block is **zeroed out** as it is put in the program's address space. 

> * Stack segment is allocated for local variables, temporaries, parameter passing in function calls, and the like.
>> **By the Way**
>> **`alloca()`** -- the stack version of **`malloc()`**. It will be automatically freed when the function that called **`alloca()`** returns to its caller.  Do not attempt to **`free()`** space allocated by **`alloca()`**.

> * The lowest part of the virtual address space  within the address space of the process is unmapped, so any references to it will be illegal. It catches references through null pointers, and pointers that have small integer values.

> * Operating systems and linkers can even assign appropriate permissions to the different sections in segments, for example, text can be made read-andexecute-only, some data can be made read-write-no-execute, other data made read-only, and so on

---

### `setjmp()` and `longjmp()`

* `setjmp(jmp_buf j)` must be called first. It says use the variable `j` to remember where you are now. Return 0 from the call.
* `longjmp(jmp_buf j,int i)` can then be called. It says go back to the place that the `j` is remembering. Make it look like you're returning from the original `setjmp()`, but return the value of `i` so the code can tell when you actually got back here via `longjmp()`. If `i = 0`, `setjmp()` return 1,else `setjmp()` return `i`. (different between `goto` and `longjmp()`: `goto` can't jump out of the current function in C)
* The contents of the `j` are destroyed once it is used in a `longjmp()`.
* `<setjmp.h>` is needed.
* Usually used in exception handling.