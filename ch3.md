
### Function

- [ ] a function return a function, **`foo()()`**
- [ ] a function return an array, **`foo()[]`**
- [ ] an array hold a function, **`foo[]()`**
- [x] a function returning a pointer to a function, **`(* foo())()`**
- [x] a function returning a pointer to an array, **`(* foo())[]`**
- [x] an array holding pointers to functions, **`(* foo[])()`**

> **NOTICE:** Arguments that don't fit into registers are pushed to  **caller's** stack that can be access by callee **from right to left** when function calls

  x86 function call convention, 64-bit(via linux_src/arch/x86/entry/calling.h):

 | arguments [callee-clobbered] |    callee-saved^[1]^    | extra caller-saved^[2]^ [callee-clobbered] |    return     |
 | :--------------------------: | :---------------------: | :----------------------------------------: | :-----------: |
 |     rdi rsi rdx rcx r8-9     | rbx rbp^[3]^ [*] r12-15 |                   r10-11                   | rax, rdx [**] |

 ( rsp is obviously invariant across normal function calls. (gcc can 'merge' functions when it sees tail-call optimization possibilities) rflags is clobbered. Leftover arguments are passed over the stack frame.)

 * In the frame-pointers case rbp is fixed to the stack frame.
 * for struct return values wider than 64 bits the return convention is a bit more complex: up to 128 bits width we return small structures straight in rax, rdx. For structures larger than that (3 words or larger) the caller puts a pointer to an on-stack return struct [allocated in the caller's stack frame] into the first argument - i.e. into rdi. All other arguments shift up by one in this case. Fortunately this case is rare in the kernel.
  
  [1] **Callee-saved registers** (AKA(also known as) non-volatile registers) are used to hold long-lived values that should be preserved across calls.
  [2] **Caller-saved registers** (AKA(also known as) volatile registers) are used to hold temporary quantities that need not be preserved across calls.
  [3] Callee's (%rbp) contains the value of caller's rbp, while 0x8(%rbp) contains return addr.

---

### Struct

> **Alignment:** In general, a struct instance will have the alignment of its widest scalar member. All the members in struct are self-aligned. Thus, reorder the structure members may save memory.

```c
    struct st
    {
        int a       : 1;          
        /* 
            * ": 1" -- one bit length, bit field.  A bit field must have a type 
        of int, unsigned int, or signed int (or a qualified version of one of 
        these). It's implementation-dependent whether bit fields that are 
        int's can be negative. 
            * C99 guarentees that bit-fields will be packed as tightly as 
        possible, provided they don’t cross storage unit boundaries. But in C11
        a bit-field can span multiple allocation units instead of starting a
        new one. It’s up to the implementation to decide and gcc prevent them 
        from sharing an allocation unit for x64.
            * C standard does not specify that bits are allocated low-to-high.
         */
        int         : 9;
        unsigned    : 0; 
        /* 
            * Unnamed bit fields cannot be referenced or initialized
            * "int : 9" -- equivalent to pad 9 bit
            * "unsigned : 0" -- pad to next int boundary, equivalent to pad 
              (32 - 9 - 1) bit in this case. Note that bit fields with a length 
              of 0 must be unnamed. 
        */
        int b;
    }; 
``` 

`struct s_tag { int a[100]; };` -- The array in struct can be treated as a first-type-class. The entire array will be **copied** with an assignment statement, passed to a function by **value**. 

>> **By the Way**
>>
>> **`struct foo { int foo; } foo;`** is legal.
>>
>> More interestingly, the code below is legal too.
>> ```c
>>      typedef struct foo 
>>      { 
>>          struct foo * foo;
>>          // struct foo foo;         
>>          /* compile error, field ‘foo’ has incomplete type. */
>>      } foo;
>>      ...
>>      foo foo;
>> ```
>> 
>>  *Forward declaration:*
>> ```c
>>      struct a 
>>      {
>>          struct b * b_ptr;       // forward declaration, legal
>>      };
>>
>>      struct b 
>>      {
>>          struct a * a_ptr;
>>      };
>> ```
>> ```c
>>      struct a 
>>      {
>>          struct b * b_ptr;       // legal
>>          // b * b_ptr;           // illegal
>>      };
>>
>>      typedef struct b 
>>      {
>>          struct a * a_ptr;
>>      } b;
>> ```
>> ```c
>>      typedef struct b b;
>>
>>      struct a 
>>      {
>>          b * b_ptr;              // legal
>>      };
>>
>>      struct b 
>>      {
>>          struct a * a_ptr;
>>      };
>> ```
>> ```c
>>      typedef struct b b;
>>
>>      struct a 
>>      {
>>          b * b_ptr;              
>>      };
>>
>>      typedef struct b            // legal
>>      {
>>          struct a * a_ptr;
>>      } b;
>> ```

---

### Enum

```c
    enum em
    {
        o;          // The integer values start at zero by default.
        t = 2;
    }
    ...
    enum em emmm = 50;
    emmm = t;
    const int c = 3;
    ...
    switch(emmm)
    {
    case o: break;          // Enum members are constants 
    case t: break;
    // case c:              // compile error, const only means read-only
    }
    ...
```