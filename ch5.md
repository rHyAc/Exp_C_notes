
> If an identifier is reserved, it means that the user is not allowed to redefine it. However, this is not a constraint, so it does not require an error message when it sees it happen. It just causes unportable, undefined behavior. In other words, if one of your function names is the same as a C library function name (deliberately or inadvertently), you have created a nonconforming program, but the translator is not obliged to warn you about it.

```c
#include <stdio.h>
#include <limits.h>

#define INT_MAX "666"
#define INT_MAX "777"

int main(void)
{
    printf(INT_MAX);        // 777
    return 0;
}
```
