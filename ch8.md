### Prototype

file 1:
```c
    int func(char);
    ...
    short a = 257;
    func(a);
    ...
```

file 2:
```c
    void func(long a) {}
```

Compile two files in gcc.

* File 1 and file 2 are compiled individually, hence even though the function declaration in file 1 doesn't match the function definition in file 2, it is valid. However, if the declaration and the definition in the same file, the former needs to match the latter.  
  
* When the argument is passed to the parameter: 
  `short` $\xrightarrow{converse}$ `char`(declaration) $\xrightarrow{promote}$ register/stack $\xrightarrow{retrieve\ Qword}$ `long`(definition)
  Watch out! Something unexpected may happen when retrieving Qwrod.

* If missing function declaration, it would default to `int func();`, and promoted types are passed directly.

> **NOTICE**
> ```c
> void a(void) {}
> ...
> void (* p)(void) = a;
> ...
> p();                  // valid
> (*P)();               // equivalent to p()
> (********P)();        // equivalent to p()
> *p();                 // error
> ...
> ```

---

### Cast
`int func(const int *i)` $\xrightarrow{cast\ to\ int (const\ void\ *)}$ `(int (const void *))func`

> **By the Way**
> Determine if a variable is signed or not
> ```c
>   #define ISUNSIGNED(a) (a >= 0 && ~a >= 0)   
>   // not work when the type of a is char or short, due to promotion
>   
>   #define ISUNSIGNED(type) ((type)0 - 1 > 0)  
>   // the argument is to be a type
> ```