### String Malloc

- [ ] `malloc(strlen(str));`
- [x] `malloc(strlen(str) + 1);`
  
> * Don't forget the the trailing nul terminator **`\0`**

---

### Switch

> * All statements before the **`case`** that matches the expression will not be executed. 
> * A label (including case label) can only be part of a statement and a declaration is not a statement
>> ```c
>>     switch(c)
>>     {
>>         int j = 0;
>>         case 0:
>>             ;
>>             int i = 0;        // OK
>>             j = 1;            // OK
>>             break;
>>         case 1:
>>             i = 2;            // OK
>>     }
>> ```
>> **`or`**
>> ```c
>>     switch(c)
>>     {
>>         int j = 0;
>>         case 0:
>>         {
>>             int i = 0;        // OK
>>             j = 1;            // OK
>>             break;
>>         }
>>         case 1:
>>             i = 2;            // OK
>>     }
>> ```
---

### String Concatenation

```c
    printf("The book "
            "is "
            "mine.");
```
> **OUTPUT：** *The book is mine.*

>> **trick:** To output string separated by comma correctly
>> ```c
>>      void func(char *str)
>>      {
>>          static char separator = ' ';        // space
>>          printf("%c %s", separator, str);
>>          separator = ',';
>>      }
>> ```

---

### Function Visibility

`void func()`, `extern void func()` -- visible everywhere
`static void func()` -- not visible outside this file

>> **By the Way** 
>> **`extern var;`** -- a variable defined elsewhere
>> **`static var;`** -- retains its value between calls 

---

### Operators Precedence

|                   Precedence                   |       Expression       |               Expection               |             Actually Get              |
| :--------------------------------------------: | :--------------------: | :-----------------------------------: | :-----------------------------------: |
|              `.` higher than `*`               |         `*p.f`         |  `(*p).f`*(recommend to use `p->f`)*  |               `*(p.f)`                |
|              `[]` higher than `*`              |      `int *ap[]`       |     ap is a ptr to array of ints      |     ap is an array of ptrs-to-int     |
|         function `()` higher than `*`          |      `int *fp()`       | fp is a ptr to function returning int | fp is a function returning ptr-to int |
| `==` and `!=` higher than "bitwise operations" |   `val & mask != 0`    |          `(val & mask) != 0`          |          `val & (mask != 0)`          |
|     `==` and `!=` higher than "assignment"     | `c = getchar() != EOF` |       `(c = getchar()) != EOF`        |       `c = (getchar() != EOF)`        |
|       “arithmetic" higher than "shift"        |    `msb << 4 + lsb`    |          `(msb << 4) + lsb`           |          `msb << (4 + lsb)`           |
|                   `,` lowest                   |       `i = 1, 2`       |           `i = (1, 2)`^[1]^           |             `(i = 1), 2`              |

[1]:  the value of a comma operator is the value of the rightmost operand, `i = (1, 2)` is equivalent to `i = 2`.

>> **NOTICE:**  The order of precedence and association are well-defined. However, the order of expression evaluation is mostly unspecified. We say "mostly" because the order is defined for **`&&`**, **`||`**, **`? :`** and **`,`**(commas that separate function arguments are not comma operators). **`&&`** and **`||`** evaluate their operands in a **strict left-to-right** order, **stopping when the result is known**. The **`?:`** operator takes three operands: **`a ? b : c`** evaluates **`a`** first, and then evaluates either **`b`** or **`c`**, depending on the value of **`a`**. The **`,`** operator evaluates its left pperand and discards its value, then evaluates its right operand. However, the order of evaluation of the arguments in a function call is another unspecified order.

---

### Space
> * ***\newline*** -- is treated as one logical line, but ***\whitespace newline*** doesn't.
> * **`c = a+++b`** -- is parsed as **`c = a++ + b`** following  the *maximal munch strategy*^[1]^.
> * **`c = a+++++b`** -- is parsed as **`c = a++ ++ + b`** and compile error^[2]^ following [1].
> * **`c = *a/*b`** -- **`a`** and **`b`** are both ptr(to-int), but **`/*`** means to open a comment.

[1] Maximal munch says that if there's more than one possibility for the next token, the compiler will **prefer** to bite off the one involving the longest sequence of characters.
[2] Lvalue is required as increment operand. Both `a++` and `++a` is rvalue in C.

>> **trick:** get string from a function gracefully
>> ```c
>>      void func(char *dst, int size)
>>      {
>>          ...
>>          strncpy(dst, src, size);
>>          ...    
>>      }
>>
>>      ...
>>      buf = malloc(size);
>>      func(buf, size);
>>      ...
>>      free(buf);
>>      ...
>> ```
