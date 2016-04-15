---
title: C程序设计语言--第四章
---

# Chapter 4 - Functions and Program Structure

## 4.1 Basics of Functions

``` c
#include<stdio.h>
#define MAXLINE 1000

int getline2(char line[], int max);
int strindex2(char source[], char searchfor[]);

char pattern[] = "ould";

int main() {
    char line[MAXLINE];
    int found = 0;
    
    while (getline2(line, MAXLINE) > 0) {
        if (strindex2(line, pattern) >= 0) {
            printf("%s", line);
            found++;
        }
    }
    
    return found;
}

/* getline2: get line into s, return length */
int getline2(char s[], int lim) {
    int c = 0;
    int i = 0;
    
    while (--lim > 0 && (c = getchar()) != EOF && c != '\n') {
        s[i++] = c;
    }
    
    if (c == '\n') {
        s[i++] = c;
    }
    
    s[i+] = '\0';
    
    return i;
}

int strindex2(char s[], char t[]) {
    int i, j, k;
    
    for (i = 0; s[i] != '\0'; i++) {
        for (j = i, k = 0; t[k] != '\0' && s[j] == t[k]; j++, k++) {
            ;
        }
        
        if (k > 0 && t[k] == '\0') {
            return i;
        }
    }
    
    return -1;
}
```

``` c
#include <stdio.h>
#include <ctype.h>

#define MAXLINE 100

int getline2(char line[], int max);
double atof2(char s[]);

/* rudimentary calculator */
int main() {
    double sum, atof2(char []);
    char line[MAXLINE];
    int getline2(char line[], int max);
    
    sum = 0;
    while (getline2(line, MAXLINE) > 0) {
        printf("\t%g\n", sum += atof2(line));
    }
    
    return 0;
}

/* getline2: get line into s, return length */
int getline2(char s[], int lim) {
    int c = 0;
    int i = 0;
    
    while (--lim > 0 && (c = getchar()) != EOF && c != '\n') {
        s[i++] = c;
    }
    
    if (c == '\n') {
        s[i++] = c;
    }
    
    s[i] = '\0';
    
    return i;
}

/* atof2: convert string s to double */
double atof2(char s[]) {
    double val, power;
    int i, sign;
    
    for (i = 0; isspace(s[i]); i++) { /* skip white space */
        ;
    }
    
    sign = (s[i] == '-') ? -1 : 1;
    
    if (s[i] == '+' || s[i] == '-') {
        i++;
    }
    
    for (val = 0.0; isdigit(s[i]); i++) {
        val = 10.0 * val + (s[i] - '0');
    }
    
    if (s[i] == '.') {
        i++;
    }
    
    for (power = 1.0; isdigit(s[i]); i++) {
        val = 10.0 * val + (s[i] - '0');
        power *= 10;
    }
    
    return sign * val / power;
}
```

