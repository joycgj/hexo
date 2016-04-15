---
title: shell 判断
date: 2016-04-15 15:35:17
tags: [linux, shell]
categories: [linux, shell]
---

## 判断符号[]

使用中括号必须要特别注意，因为中括号用在很多地方，包括通配符与正则表达式等，所以如果要在 bash 的语法当中使用中括号作为 shell 的判断式时，必须要注意中括号的两端需要有空格符来分割。

判断 $HOME 这个变量是否为空

``` shell
[root@default-tpl vbird_shell]# [ -z "$HOME" ] ; ECHO $?
1
```

* 在中括号[]内的每个组件都需要有空格键来分割；
* 在中括号内的变量，最好都以双引号括起来；
* 在中括号内的常量，最好都以单引号或双引号括起来；
* 判断式中使用两个等号 == 与使用 = 的结果是一样的，建议使用两个等号；

为什么变量要用双引号括起来

``` shell
[root@default-tpl vbird_shell]# name="hello world"
[root@default-tpl vbird_shell]# echo $name
hello world
[root@default-tpl vbird_shell]# [ $name == "hello" ]
-bash: [: too many arguments
```

一个例子：

``` shell
#!/bin/sh

read -p "please input (Y/N):" yn

[ "$yn" == "Y" -o "$yn" == "y" ] && echo "ok, continue" && exit 0
[ "$yn" == "N" -o "$yn" == "n" ] && echo "oh, interrupt" && exit 0

echo "I don't know what your choice is" && exit 0
```









