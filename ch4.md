
### Array and Pointer

***difference***

> file 1: **`int mango[100];`**, file 2: **`extern int * mango;`** -- wrong. Array and pointer are not interchangeable in this case. The difference between them will be clear by disassembling the code below:
> 
> (To a compiler, an array indicates the address of a memory block that contain the value of **`array_name[0]`**, and a pointer indicates the address of a memory block that contain an address)
> 
>```c
>    int mango[10] = {0}; 
>    int * mg; 
>    mg = mange;
>    mange[2]++;
>    mg[2]++;
>```
>
> Actually, the declaration/definition of an array gives you an array, the declaration/definition of a pointer gives you a pointer (except the declaration of a function parameter). Hence the declaration in file 2 doesn't match the definition in file 1.

>   **`char * p = "ro_str";`** **`char q[] = "r_str";`** -- "ro_str" is read-only(put in ro-data segment by gcc), but "r_str" is writable.

***Equivalece***

> * An array name in the declaration of a function parameter is treated by the compiler as a pointer to the first element of the array -- the array as an argument becomes a true pointer inside function.
> * A subscript is always equivalent to an offset from a pointer.
> * An array name in an expression (in contrast with a declaration) is treatedby the compiler as a pointer to the first element of the array(with some exceptions, such as **`sizeof`** and **`&`**). 

> **NOTICE:** Don't confused by **`6[a]`** where **`a`** is an array. It is equivalent to **`a[6]`** which is familiar.

---

### Multidimensional Arrays

> **`typedef int arr[10]; arr a[10];`** is equivalent to **`int a[10][10];`**. Access them by **`a[i][j]`**.

```c
    int arr[2][3][5];
    int (* t)[2][3][5] = &arr;
    int (* p)[3][5] = arr;
    int (* q)[5] = arr[i];
    int * r = arr[i][j];
    int s = arr[i][j][k];
```

> When being parameters
> **`int arr[2][3]`** $\xrightarrow{decay\ to}$ **`int (* arr)[3]`**
> **`int * arr[5]`** $\xrightarrow{decay\ to}$ **`int ** arr`**
> **`int (* arr)[3]`** doesn't change

> **NOTICE:** You can only assign to an entire array during its definition. For instance, **`int arr[][3] ={ {0,0,0}, {1,1,1}, };`** -- you can only omit the most significant dimension(the same as a function parameter). If the array dimension is bigger than the number of initial values provided, the remaining elements are set to zero.

>> **By the Way**
>> `(char *)arr[5];` -- cast the type of `arr[5]` to `char *`, not the declaration of a 5-element array, each element of which is a pointer to a character

>> **Something Interesting**
>> swap two number
>> ```c
>>      a ^= b;         a = a + b;          a = a * b;
>>      b ^= a;         b = a - b;          b = a / b;
>>      a ^= b;         a = a - b;          a = a / b;
>> ```