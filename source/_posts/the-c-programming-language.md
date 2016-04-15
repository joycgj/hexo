---
title: C程序设计语言--第一章
---

# CHAPTER 1:  A Tutorial Introduction

## 1.1 Getting Started

``` c
#include <stdio.h>

main() {
    printf("hello world\n");
}
```

## 1.2 Variables and Arithmetic Expressions

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

``` c
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

## 1.3 The For Statement

``` c
#include <stdio.h>

int main() {
    int fahr;

    for (fahr = 0; fahr <= 300; fahr = fahr + 20) {
        printf("%3d %6.1f\n", fahr, (5.0 / 9.0) * (fahr - 32.0));
    }

    return 0;
}
```

## 1.4 Symbolic Constants

``` c
#include <stdio.h>

#define LOWER 0
#define UPPER 300
#define STEP  20

int main() {
    int fahr;

    for (fahr = LOWER; fahr <= UPPER; fahr = fahr + STEP) {
        printf("%3d %6.1f\n", fahr, (5.0 / 9.0) * (fahr - 32.0));
    }

    return 0;
}
```

## 1.5 Character Input and Output

### 1.5.1 File Copying

``` c
#include <stdio.h>

int main() {
    int c;

    c = getchar();
    while (c != EOF) {
        putchar(c);
        c = getchar();
    }

    return 0;
}
```

``` c
#include <stdio.h>

int main() {
    int c;

    while ((c = getchar()) != EOF) {
        putchar(c);
    }

    return 0;
}
```

### 1.5.2 Character Counting

``` c
#include <stdio.h>

int main() {
    long nc;

    nc = 0;
    while (getchar() != EOF) {
        ++nc;
    }

    printf("%ld\n", nc);

    return 0;
}
```

``` c
#include <stdio.h>

int main() {
    double nc;

    for (nc =0; getchar() != EOF; ++nc) {
        ;
    }

    printf("%.0f\n", nc);

    return 0;
}
```

### 1.5.3 Line Counting

### 1.5.4 Word Counting

## 1.6 Arrays

## 1.7 Functions

``` c
#include <stdio.h>

int power(int m, int n);

/* test power function */
int main() {
    int i;

    for (i = 0; i < 10; i++) {
        printf("%d %d %d\n", i, power(2, i), power(-3, i));

    }
    return 0;
}

/* power: raise base to n-th power; n >= 0 */
int power(int base, int n) {
    int i, p;

    p = 1;
    for (i = 1; i <= n; i++) {
        p = p * base;

    }

    return p;
}

```

``` c
#include <stdio.h>

float FtoC(float f)
{
    float c;
    c = (5.0 / 9.0) * (f - 32.0);
    return c;
}

int main(void)
{
    float fahr, celsius;
    int lower, upper, step;

    lower = 0;
    upper = 300;
    step = 20;

    printf("F     C\n\n");
    fahr = lower;
    while(fahr <= upper)
    {
        celsius = FtoC(fahr);
        printf("%3.0f %6.1f\n", fahr, celsius);
        fahr = fahr + step;

    }
    return 0;
}
```

## 1.8 Arguments - Call by Value

必要时，也可以让函数能够修改主调函数中的变量。这种情况下，调用者需要向被调用函数提供待设置值的变量的地址（从技术角度看，地址就是指向变量的指针），而被调用函数则需要将对应的参数声明为指针类型，并通过它间接访问变量。

## 1.9 Character Arrays

``` c
#include <stdio.h>

#define MAXLINE 1000    /* maximum input line length */

int getoneline(char line[], int maxline);
void copy(char to[], char from[]);

/* print the longest input line */
int main() {
    int len;                /* current line length */
    int max;                /* maximum length seen so far */
    char line[MAXLINE];     /* current input line */
    char longest[MAXLINE];  /* longest line saved here */

    max = 0;
    while ((len = getoneline(line, MAXLINE)) > 0) {
        if (len > max) {
            max = len;
            copy(longest, line);

        }

    }

    if (max > 0) {          /* there was a line */
        printf("%s", longest);
    }
    return 0;
}

/* getoneline: read a line into s, return length */
int getoneline(char line[], int maxline) {
    int c, i;

    for (i = 0; i < (maxline - 1) && (c = getchar()) != EOF && c != '\n'; ++i) {
        line[i] = c;

    }

    if (c == '\n') {
        line[i] = c;
        ++i;

    }

    line[i] = '\0';
    return i;
}

/* copy: copy 'from' into 'to', assume to is big enough */
void copy(char to[], char from[]) {
    int i;

    i = 0;
    while ((to[i] = from[i]) != '\0') {
        ++i;

    }
}

```

## 1.10 External Variables and Scope

``` c
#include <stdio.h>

#define MAXLINE 1000    /* maximum input line size */

int max;                /* maximum length seen so far */
char line[MAXLINE];     /* current input line */
char longest[MAXLINE];  /* longest line saved here */

int getoneline(void);
void copy(void);

/*  print longest input line; specialized version */
int main() {
    int len;
    extern int max;
    extern char longest[];

    max = 0;
    while ((len = getoneline()) > 0) {
        if (len > max) {
            max = len;
            copy();

        }

    }

    if (max > 0) {
        printf("\nlongest=%s", longest);

    }
    return 0;
}

/*  getoneline: specialized version */
int getoneline(void) {
    int c, i;
    extern char line[];

    for (i = 0; i < (MAXLINE - 1) && (c = getchar()) != EOF && c != '\n'; ++i) {
        line[i] = c;

    }

    if (c == '\n') {
        line[i] = c;
        ++i;

    }

    line[i] = '\0';
    return i;
}

/*  copy: specialized version */
void copy(void) {
    int i;
    extern char line[], longest[];

    i = 0;
    while ((longest[i] = line[i]) != '\0') {
        ++i;

    }
}
```
