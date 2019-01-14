### Prototype

file 1:
```c
    char i;
    int func(char);
    int main(void)
    ...
        short a = 257;
        func(a);
    ...
```

file 2:
```c
    void func(long a) 
    {
        // extern long i = 1;    // extern variable cannot have initializer
        extern long i;       
        i = 1;
    }
```

Compile two files in gcc(or clang).

* File 1 and file 2 are compiled individually, hence even though the function declaration in file 1 doesn't match the function definition in file 2, two file can be compiled successfully. However, if the declaration and the definition in the same file, the former needs to match the latter. Hence declaring the function in a head file which should be included by files use the function as well as by the file defines the function can solve this issue. 
  
* In this case, when the argument is passed to the parameter: 
  `short` $\xrightarrow{converse}$ `char`(declaration) $\xrightarrow{promote}$ register/stack $\xrightarrow{retrieve\ Qword}$ `long`(definition)
  Watch out! Something unexpected may happen when retrieving Qwrod.

*  A function definition with parameters type that has a default promotion including `float`(double promotion only here), `char` and `short` in general can’t match the function declaration with an empty parameter name. Call passes promoted types when calling a function with an empty parameter name declaration.
  
* In the case, the two instances of `i` require different amounts of storage, but they happen to share storage in such a way that the values assigned to one are valid for the other, though the compiler doesn't complain about it too.

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