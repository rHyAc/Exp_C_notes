
### Const

- [ ] `char * = const char *`
- [x] `const char * = char *`^[1]^

> * Both operands are pointers to qualified or unqualified versions of compatible types, and the type pointed to by the left has all the qualifiers of the type pointed to by the right.

- [ ] `char ** = const char **`

> * **`const type *`** -- a pointer to const-qualified *type*. 
> * Similarly, **`const char **`** means **a pointer to a pointer to a qualified char**.
> * Since the types **`char **`** and **`const char **`** are both pointers to unqualified types that are not the same type, they are not compatible types. 

> * The keyword **`const`** doesn't turn a variable into a constant! A symbol with the const qualifier merely means that the symbol cannot be used for assignment. 

[1] `const char *`, `char const *` -- ptr(to-read-only-char); `char * const` -- read-only ptr(to-char).

---

### Integral Promotions --  Value Preserving 

`(unsigned) char_8, (unsigned) short_16, enum_?` $\rightarrow$ `int_32` 
> * A char, a short int, or an int bit-field, or their signed or unsigned varieties, or an enumeration type, may be used in an expression wherever an int or unsigned int may be usedIf an int can represent all the values of the original type, the value is converted to an int; otherwise it is converted to an unsigned int. These are called the integral promotions.

---

### Arithmetic Conversions --  Value Preserving 

short $\rightarrow$ longest/floatiest with signed if possible

> * First, if either operand has type long double, the other operand is converted to long double. Otherwise, if either operand has type double, the other operand is converted to double. Otherwise, **if either operand has type float, the other operand is converted to float (NOTICE: no double promotions)**. Otherwise the integral promotions are performed on both operands. Then the following rules are applied.
> * If either operand has type unsigned long int, the other operand is converted to unsigned long int. Otherwise, if one operand has type long int and the other has type unsigned int, if a long int can represent all values of an unsigned int the operand of type unsigned int is converted to long int; if a long int cannot represent all the values of an unsigned int, both operands are converted to unsigned long int. Otherwise, if either operand has type long int, the other operand is converted to long int. Otherwise, if either operand has type unsigned int, the other operand is converted to unsigned int. Otherwise, both operands have type int.

>> **NOTICE:** **`sizeof`** has a **`unsigned long`** return type in *gcc*. And sizeof is an **operator** but not a function.
>> **Hence:**
>> * **`int p; sizeof p;`** -- valid;
>> * But **`sizeof int`** -- invalid, it must be **`sizeof(int)`**
>> * **`sizeof (int) * p`** -- the length of **`int`** mul **`p`**