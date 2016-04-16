---
title: Linux shell “(())” 双括号运算符使用
date: 2016-04-15 21:55:54
tags: [linux,shell,括号]
categories: [linux,shell,括号]
---

[原文地址](http://www.cnblogs.com/chengmo/archive/2010/10/19/1855577.html)

## 使用方法

```
语法：

((表达式1,表达式2...))

特点:

1、在双括号结构中，所有表达式可以像c语言一样，如：a++，b-- 等；

2、在双括号结构中，所有变量可以不加入 "$" 符号前缀；

3、双括号可以进行逻辑运算，四则运算；

4、双括号结构 扩展了for，while， if 条件测试运算；

5、支持多个表达式运算，各个表达式之间用 "," 分开。
```

## 实例

### 扩展四则运算

```bash
#!/bin/sh

a=1
b=2
c=3

((a=a+1))
echo $a

a=$((a+1,b++,c++))
echo $a,$b,$c
```
result:

```
2
3,3,4
```
双括号结构之间支持多个表达式，然后加减乘除等c语言常用运算符都支持。如果双括号带 "$"，将获得表达式值赋值给左边变量。

### 扩展逻辑运算

```bash
#!/bin/sh

a=1
b="ab"

echo $((a>1?8:9))

((b!="a")) && echo "err2"
((a<2)) && echo "ok"
```

result:

```
9
err2
ok
```

### 扩展流程控制语句（逻辑关系式）

```bash
#!/bin/sh

num=100
total=0

for((i=0;i<=num;i++))
do
    ((total+=i))
done

echo $total

total=0
i=0

while ((i<=num))
do
    ((total+=i,i++))
done

echo $total

if ((total>=5050)); then
    echo "ok"
fi
```

result:

```
5050
5050
ok
```
