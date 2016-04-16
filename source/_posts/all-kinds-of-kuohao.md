---
title: shell中各种括号的作用()、(())、[]、[[]]、{} todo
date: 2016-04-15 22:37:45
tags: [linux,shell]
categories: [linux,shell]
---

[原文地址](http://blog.csdn.net/taiyang1987912/article/details/39551385)

## 一、 小括号、圆括号（）(())

### 单小括号 ()

* 命令组。括号中的命令将会新开一个子 shell 顺序执行，所以括号中的变量不能够被脚本余下的部分使用。括号中多个命令之间用分号隔开，最后一个命令可以没有分号，各命令和括号之间不必有空格。

	* 一串的命令执行()和{}，()和{}都是对一串的命令进行执行，但有所区别
		1. ()只是对一串命令重新开一个子shell进行执行
		2. {}对一串命令在当前shell执行
		3. ()和{}都是把一串的命令放在括号里面，并且命令之间用;号隔开
		4. ()最后一个命令可以不用分号
		5. {}最后一个命令要用分号
		6. {}的第一个命令和左括号之间必须要有一个空格
		7. ()里的各命令不必和括号有空格
		8. ()和{}中括号里面的某个命令的重定向只影响该命令，但括号外的重定向则影响到括号里的所有命令

```bash
#!/bin/sh

var=test

(var=test01; echo $var)     ## 变量var值为test01,在子shell中有效

echo $var                   ## 父shell中值仍为test

{ var=test02; echo $var;}   ## 注意左括号和var之间有一个空格

echo $var                   ## 父shell中的var变量的值变为test02
```

```
test01
test
test02
test02
```

* 命令替换。等同于 `cmd`，shell扫描一遍命令行，发现了 $(cmd) 结构，便将 $(cmd) 中的 cmd 执行一次，得到其标准输出，再将此输出放到原来命令。

```bash
$ ls 
a b c 

$ echo $(ls) 
a b c 

$ echo `ls` 
a b c
```

* 用于初始化数组。如：array=(a b c d)。

### 双小括号 (())

* 只要括号中的运算符、表达式符合c语言运算规则，都可用在$((exp))中，甚至是三目运算符。作不同进位(如二进制、八进制、十六进制)运算时，输出结果全都自动转化成了十进制。如：echo $((16#5f)) 结果为95 (16进位转十进制)

* 单纯用 (()) 也可重定义变量值，比如 a=5; ((a++)) 可将 $a 重定义为6；

* 常用于算术运算比较，双括号中的变量可以不使用 $ 符号前缀，也可以使用 $。括号内支持多个表达式用逗号分开。只要括号中的表达式符合c语言运算规则，比如可以直接使用 for((i=0;i<5;i++))， 如果不使用双括号, 则为 for i in `seq 0 4` 或者 for i in {0..4}。再如可以直接使用 if (($i<5)) 或者 if ((i<5)), 如果不使用双括号, 则为if [ $i -lt 5 ]。

```bash
#!/bin/sh

for i in $(seq 0 2)
do
    echo $i
done

for i in `seq 0 2`
do
    echo $i
done

for i in {0..2}
do
    echo $i
done

for ((i=0;i<3;i++))
do
    echo $i
done
```

```
0
1
2
0
1
2
0
1
2
0
1
2
```

```bash
#!/bin/sh

i=5

if ((i<10)); then
    echo "less than 10" 
fi

if (($i<10)); then
    echo "less than 10"
fi

if [ $i -lt 10 ]; then
    echo "less than 10"
fi
```

```
less than 10
less than 10
less than 10
```

## 二、中括号，方括号[]

### 单中括号 []

* bash 的内部命令，[ 和 test 是等同的。if/test 结构中的左中括号是调用 test 的命令标识，右中括号是关闭条件判断的。这个命令把它的参数作为比较表达式或者作为文件测试，并且根据比较的结果来返回一个退出状态码。

```bash
$ test 1 -eq 2 && echo equal || echo "not equal"
not equal

$ [ 1 -eq 2 ] && echo equal || echo "not equal"
not equal
```

* Test 和 [] 中可用的比较运算符只有 == 和 !=，两者都是用于字符串比较的，不可用于整数比较，整数比较只能使用 -eq，-gt 这种形式。无论是字符串比较还是整数比较都不支持大于号小于号。如果实在想用，对于字符串比较可以使用转义形式，如果比较 "ab" 和 "bc"：[ ab \< bc ]，结果为真，也就是返回状态为0。[] 中的逻辑与和逻辑或使用 -a 和 -o 表示。

*	字符范围。用作正则表达式的一部分，描述一个匹配的字符范围。作为 test 用途的中括号内不能使用正则。

*	在一个 array 结构的上下文中，中括号用来引用数组中每个元素的编号。

### 双中括号[[]]

* [[ 是 bash 程序语言的关键字。并不是一个命令，[[]] 结构比 [] 结构更加通用。在 [[ 和 ]] 之间所有的字符都不会发生文件名扩展或者单词分割，但是会发生参数扩展和命令替换。

* 支持字符串的模式匹配，使用 =~ 操作符时甚至支持 shell 的正则表达式。字符串比较时可以把右边的作为一个模式，而不仅仅是一个字符串，比如[[ hello == hell? ]]，结果为真。[[]] 中匹配字符串或通配符，不需要引号。

* 使用[[ ... ]]条件判断结构，而不是[ ... ]，能够防止脚本中的许多逻辑错误。比如，&&、||、< 和 > 操作符能够正常存在于 [[]] 条件判断结构中，但是如果出现在 [] 结构中的话，会报错。比如可以直接使用 if [[ $a != 1 && $a != 2 ]], 如果不使用双括号, 则为 if [ $a -ne 1 ] && [ $a != 2 ] 或者 if [ $a -ne 1 -a $a != 2 ]。

* bash 把双中括号中的表达式看作一个单独的元素，并返回一个退出状态码。

### test, [], [[]] 区别

因为 shell 和我们通常编程语言不同，更多的情况是和它交互，总是调用别人。 所以有些本属于程序语言本身的概念在 shell 中会难以理解。

以 bash 为例 (其他兼容 shell 差不多)： 

*	test 和 [ 是 bash 的内部命令，GNU/linux 系统的 coreutils 软件包通 常也带 /usr/bin/test 和 /usr/bin/[ 命令。如果我们不用绝对路径指明，通常我们用的都是 bash 自带的命令。 
*	[[ 是 bash 程序语言的关键字！ 

```bash
$ ls -l /usr/bin/[ /usr/bin/test 
-rwxr-xr-x 1 root root 37400  9月 18 15:25 /usr/bin/[ 
-rwxr-xr-x 1 root root 33920  9月 18 15:25 /usr/bin/test 

$ type [ [[ test 
[ is a shell builtin 
[[ is a shell keyword 
test is a shell builtin 
```

绝大多数情况下，这个三个功能通用。但是命令和关键字总是有区别的。命令和关键字的差别有多大呢？
 
如果是命令，它就和参数组合为一体被 shell 解释，那样比如 ">" "<" 就被 shell 解释为重定向符号了。关键字却不这样。 
在 [[ 中使用 && 和 || 
[ 中使用 -a 和 -o 表示逻辑与和逻辑或。 
[[ 中可以使用通配符，[[ 中匹配字符串或通配符，不需要引号 

```bash 
arch=i486 
[[ $arch = i*86 ]] && echo "arch is x86!" 

arch is x86
```

## 三、大括号、花括号 {}

### 常规用法

* 大括号拓展。(通配(globbing))将对大括号中的文件名做扩展。在大括号中，不允许有空白，除非这个空白被引用或转义。
	* 第一种：对大括号中的以逗号分割的文件列表进行拓展。如 touch {a,b}.txt 结果为 a.txt b.txt。
	* 第二种：对大括号中以点点（..）分割的顺序文件列表起拓展作用，如：touch {a..d}.txt 结果为 a.txt b.txt c.txt d.txt

* 代码块，又被称为内部组，这个结构事实上创建了一个匿名函数。与小括号中的命令不同，大括号内的命令不会新开一个子shell运行，即脚本余下部分仍可使用括号内变量。括号内的命令间用分号隔开，最后一个也必须有分号。{} 的第一个命令和左括号之间必须要有一个空格。

### 几种特殊的替换结构

* ${var:-string}, ${var:+string}, ${var:=string}, ${var:?string}
      
	1. ${var:-string} 和 ${var:=string}: 若变量 var 为空，则用在命令行中用 string 来替换 ${var:-string}，否则变量 var 不为空时，则用变量 var 的值来替换 ${var:-string}；对于 ${var:=string} 的替换规则和 ${var:-string} 是一样的，所不同之处是 ${var:=string} 若 var 为空时，用 string 替换 ${var:=string} 的同时，把 string 赋给变量var: ${var:=string} 很常用的一种用法是，判断某个变量是否赋值，没有的话则给它赋上一个默认值。

```bash
#!/bin/sh

a=${var:-"hello"}
echo $a
echo $var

b=${var:="world"}
echo $b
echo $var

var2="hi"
c=${var2:-"hello"}
echo $c
echo $var2

result:

hello

world
world
hi
hi
```

2. ${var:+string} 的替换规则和上面的相反，即只有当 var 不是空的时候才替换成 string，若 var 为空时则不替换或者说是替换成变量 var 的值，即空值。(因为变量 var 此时为空，所以这两种说法是等价的) 。

3. ${var:?string} 替换规则为：若变量 var 不为空，则用变量 var 的值来替换 ${var:?string}；若变量 var 为空，则把 string 输出到标准错误中，并从脚本中退出。我们可利用此特性来检查是否设置了变量的值。
	
补充扩展：在上面这五种替换结构中 string 不一定是常值的，可用另外一个变量的值或是一种命令的输出。
      
### 四种模式匹配替换结构

模式匹配记忆方法：

1. # 是去掉左边(在键盘上#在$之左边)
2. % 是去掉右边(在键盘上%在$之右边)

注意：#和%中的单一符号是最小匹配，两个相同符号是最大匹配。

* ${var%pattern}, ${var%%pattern}, ${var#pattern}, ${var##pattern}

	1. 第一种模式：${variable%pattern}，这种模式时，shell 在 variable 中查找，看它是否以给的模式 pattern 结尾，如果是，就从命令行把 variable 中的内容去掉右边最短的匹配模式
     
   2. 第二种模式： ${variable%%pattern}，这种模式时，shell 在 variable 中查找，看它是否以给的模式 pattern 结尾，如果是，就从命令行把 variable 中的内容去掉右边最长的匹配模式
    
   3. 第三种模式：${variable#pattern} 这种模式时，shell在variable中查找，看它是否一给的模式pattern开始，如果是，就从命令行把variable中的内容去掉左边最短的匹配模式
     
   4. 第四种模式： ${variable##pattern} 这种模式时，shell在variable中查找，看它是否一给的模式pattern结尾，如果是，就从命令行把variable中的内容去掉左边最长的匹配模式

```bash
#!/bin/sh

var=testcase  
echo $var  
echo ${var%s*e} 
echo $var  
echo ${var%%s*e} 
echo ${var#?e}  
echo ${var##?e}  
echo ${var##*e}  
echo ${var##*s}  
echo ${var##test}  

testcase
testca
testcase
te
stcase
stcase

e
case
```
     
这四种模式中都不会改变 variable 的值，其中，只有在 pattern 中使用了 * 匹配符号时，% 和 %%，# 和 ## 才有区别。结构中的 pattern 支持通配符，* 表示零个或多个任意字符，? 表示仅与一个任意字符匹配，[...] 表示匹配中括号里面的字符，[!...] 表示不匹配中括号里面的字符。

### 字符串提取和替换
     
* ${var:num}, ${var:num1:num2}, ${var/pattern/pattern}, ${var//pattern/pattern}

	1. 第一种模式：${var:num}，这种模式时，shell 在 var 中提取第 num 个字符到末尾的所有字符。若 num 为正数，从左边 0 处开始；若 num 为负数，从右边开始提取字串，但必须使用在冒号后面加空格或一个数字或整个 num 加上括号，如 ${var: -2}、${var:1-3} 或 ${var:(-2)}。         
        
   2. 第二种模式：${var:num1:num2}，num1是位置，num2是长度。表示从$var字符串的第$num1个位置开始提取长度为$num2的子串。不能为负数。
       
   3. 第三种模式：${var/pattern/pattern}表示将var字符串的第一个匹配的pattern替换为另一个pattern。。         
       
   4. 第四种模式：${var//pattern/pattern}表示将var字符串中的所有能匹配的pattern替换为另一个pattern。

```bash
[root@centos ~]# var=/home/centos
[root@centos ~]# echo $var
/home/centos
[root@centos ~]# echo ${var:5}
/centos
[root@centos ~]# echo ${var: -6}
centos
[root@centos ~]# echo ${var:(-6)}
centos
[root@centos ~]# echo ${var:1:4}
home
[root@centos ~]# echo ${var/o/h}
/hhme/centos
[root@centos ~]# echo ${var//o/h}
/hhme/cenths
```
       
## 符号 $ 后的括号

1. ${a} 变量 a 的值, 在不引起歧义的情况下可以省略大括号。

2. $(cmd) 命令替换，和 `cmd` 效果相同，结果为 shell 命令 cmd 的输出

3. $((expression)) 和 `exprexpression` 效果相同, 计算数学表达式exp的数值, 其中 exp 只要符合c语言的运算规则即可, 甚至三目运算符和逻辑表达式都可以计算。

## 多条命令执行

* 单小括号，(cmd1;cmd2;cmd3) 新开一个子 shell 顺序执行命令 cmd1,cmd2,cmd3, 各命令之间用分号隔开, 最后一个命令后可以没有分号。

* 单大括号，{ cmd1;cmd2;cmd3;} 在当前 shell 顺序执行命令 cmd1,cmd2,cmd3, 各命令之间用分号隔开, 最后一个命令后必须有分号, 第一条命令和左括号之间必须用空格隔开。
对 {} 和 () 而言, 括号中的重定向符只影响该条命令， 而括号外的重定向符影响到括号中的所有命令。            








