---
title: shell 判断、循环、函数
date: 2016-04-15 15:35:17
tags: [linux, shell]
categories: [linux, shell]
---

## 判断符号[]

使用中括号必须要特别注意，因为中括号用在很多地方，包括通配符与正则表达式等，所以如果要在 bash 的语法当中使用中括号作为 shell 的判断式时，必须要注意中括号的两端需要有空格符来分割。

判断 $HOME 这个变量是否为空

```bash
[root@default-tpl vbird_shell]# [ -z "$HOME" ] ; ECHO $?
1
```

* 在中括号[]内的每个组件都需要有空格键来分割；
* 在中括号内的变量，最好都以双引号括起来；
* 在中括号内的常量，最好都以单引号或双引号括起来；
* 判断式中使用两个等号 == 与使用 = 的结果是一样的，建议使用两个等号；

为什么变量要用双引号括起来

```bash
[root@default-tpl vbird_shell]# name="hello world"
[root@default-tpl vbird_shell]# echo $name
hello world
[root@default-tpl vbird_shell]# [ $name == "hello" ]
-bash: [: too many arguments
```

一个例子：

```bash
#!/bin/sh

read -p "please input (Y/N):" yn

[ "$yn" == "Y" -o "$yn" == "y" ] && echo "ok, continue" && exit 0
[ "$yn" == "N" -o "$yn" == "n" ] && echo "oh, interrupt" && exit 0

echo "I don't know what your choice is" && exit 0
```

## if...then

### 单层、简单条件判断式

``` 
if [ 条件判断式 ]; then
	code here
fi
```

当有多个条件要判断时， 除了 

```
[ "$yn" == "Y" -o "$yn" == "y" ]
```

这种写法，也就是将多个条件写入一个中括号内的情况之外，还可以有多个中括号来隔开，即

```
[ "$yn" == "Y" ] || [ "$yn" == "y" ]
```

而括号与括号之间，则以 && 或 || 来分割，其中 && 代表 AND, || 代表 OR。

之所以这样改，很多人是习惯问题，很多人则是习惯一个中括号中仅有一个判断式的原因。

### 多重、复杂条件判断式

```
# 一个条件判断
if [ 条件判断式 ]; then
	code here
else
	code here
fi
```

```

# 多个条件判断(if...elif...else)
if [ 条件判断式一 ]; then
	code here
elif [ 条件判断式二 ]; then
	code here
else
	code here
fi
```

注意：elif 后面都要接 then 来处理，但是 else 已经是最后了，所以 else 后面并没有 then。

```bash
#!/bin/sh

read -p "please input (Y/N):" yn

if [ "$yn" == "Y" ] || [ "$yn" == "y" ]; then
	echo "ok, continue"
elif [ "$yn" == "N" ] || [ "$yn" == "n" ]; then
	echo "oh, interrupt"
else
	echo "I don't know what your choice is"
fi
```

## case...esac

```
case $变量名 in
	"第一个变量内容")   	## 每个变量内容建议用双引号括起来，关键字则为小括号)
		code here
		;;					## 每个类型结尾使用两个连续的分号来处理
	"第二个变量内容")
		code here
		;;
	*)						## 最后一个变量内容都会用 * 来代表所有其它值
		code here
		;;
esac
```

```bash
#!/bin/sh

echo "this program will print your selection"

case $1 in
    "one")
        echo "one"
        ;;
    "two")
        echo "two"
        ;;
    "three")
        echo "three"
        ;;
    *)
        echo "usage $0 {one|two|three}"
        ;;
esac
```

## function

shell script 的执行方式是由上而下、由左而右，因此 function 的设置一定要在程序的最前面。

function 也是拥有内部变量的，它的内置变量与 shell script 很类似，函数名称 $0，而后续的变量以 $1，$2... 来代替。

```bash
#!/bin/sh

function printit() {
    echo "your choice is $1"
}

echo "this program will print your selection"

case $1 in
    "one")
        printit 1
        ;;
    "two")
        printit 2
        ;;
    "three")
        printit 3
        ;;
    *)
        echo "usage $0 {one|two|three}"
        ;;
esac
```

## while do done, until do done(不定循环)

当 condition 条件成立时，就进行循环，直到 condition 的条件不成立才停止

```
while [ condition ]
do
	code here
done
```

```bash
#!/bin/sh

while [ "$yn" != "Y" -a "$yn" != "y" ]
do
    read -p "please input y/Y to stop this program: " yn
done

echo "OK! you input the correct answer."
```

当条件成立时，就终止循环，否则就持续进行循环

```
until [ condition ]
do
	code here
done
```

```bash
#!/bin/sh

until [ "$yn" == "Y" -o "$yn" == "y" ]
do
    read -p "please input y/Y to stop this program: " yn
done

echo "OK! you input the correct answer."
```


















