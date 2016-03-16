---
title: C程序设计语言
---

# CHAPTER 1:  A Tutorial Introduction

## Getting Started

``` c
#include <stdio.h>

main()
{
    printf("hello world\n");

}
```

## Variables and Arithmetic Expressions

``` c
#include <stdio.h>

int main() {
    int fahr, celsius;
    int lower, upper, step;

    lower = 0;
    upper = 300;
    step = 20;

    fahr = lower;
    while (fahr <= upper) {
        celsius = 5 * (fahr - 32) / 9;
        printf("%d\t%d\n", fahr, celsius);
        fahr = fahr + step;

    }

    return 0;
}
```

```
#include <stdio.h>

int main() {
    float fahr, celsius;
    int lower, upper, step;

    lower = 0;
    upper = 300;
    step = 20;

    fahr = lower;
    while (fahr <= upper) {
        celsius = (5.0 / 9.0) * (fahr - 32.0);
        printf("%3.0f %6.1f\n", fahr, celsius);
        fahr = fahr + step;

    }

    return 0;
}
```


