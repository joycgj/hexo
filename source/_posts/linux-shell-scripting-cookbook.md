---
title: Linux Shell 脚本攻略
date: 2016-04-17 13:16:07
tags: [linux, shell]
categories: [linux, shell]
---

# Linux Shell 脚本攻略

##  第二章 命令之乐

### 2.4 文件查找与文件列表

find 命令的工作方式：沿着文件层次结构向下遍历，匹配符合条件的文件，并执行相应的操作。

* -print: '\n' 作为用于分割文件的定界符

```bash
find . -print
.
./a
./b
./c
```

* -print0: 使用 '\0' 作为用于分割文件的定界符，当文件中含有空格时，这个方法就有了用武之地。

* -path 可以使用通配符来匹配文件路径和文件

```
$ find /home/users -path "*slynux*" -print
/home/users/list/slynux.txt
/home/users/slynux/eg.css
```

* ! not 一样

``` 
$ find . ! -name "*.txt" 
$ find . not -name "*.txt"
```

* -maxdepth -mindepth

如下命令只列出当前目录下的所有普通文件，即使有子目录，也不会遍历

```
$ find . -maxdepth 1 -type f -print
```

如下命令打印出深度距离当前目录至少两个子目录的所有文件，即使当前目录或 dir1 和 dir3 中包含有文件，它们也不会被打印出来。

```
$ find . -mindepth 2 -type f -print
./dir1/dir2/file1
./dir3/dir4/f2
```

### 2.5 玩转 xargs

我们可以用管道将一个命令的 stdout（标准输出）重定向到另一个命令的 stdin（标准输入）。例如 cat foo.txt | grep "test"

但是，有些命令只能以命令行参数的形式接收数据，而无法通过 stdin 接收数据流。在这种情况下，我们没法用管道来提供那些只有通过命令行参数才能提供的数据。

```
find /sbin -perm +700 | ls -l         ## 这个命令是错误的
find /sbin -perm +700 | xargs ls -l   ## 这样才是正确的
```

xargs 擅长将标准输入流数据转化成命令行参数。

xargs 把从 stdin 接收到的数据重新格式化，再将其作为参数提供给其他命令。

* 将多行输入转换成单行输出

利用 xargs，我们可以用空格替换掉换行符，这样一来，就能够将多行文本转换成单行文本

```
# cat example.txt
1 2 3 4 5 6
7 8 9 10
11 12

# cat example.txt | xargs
1 2 3 4 5 6 7 8 9 10 11 12
```

* 将单行输入转换成多行输出

```
# cat example.txt | xargs -n 3
1 2 3
4 5 6
7 8 9
10 11 12
```

```
# cat cecho.sh
#!/bin/sh

echo $* '#'
```

```
# cat args.txt
arg1
arg2
arg3
```

要达到如下这种阵型的效果

```
./cecho -p arg1 -l
./cecho -p arg2 -l
./cecho -p arg3 -l
```

xargs 有一个选项 -I，可以提供上面这种形式的命令执行序列，我们可以用 -I 指定一个替换字符串，这个字符串在 xargs 扩展时会被替换掉。当 -I 与 xargs 结合使用时，对于每一个参数，命令都会被执行一次。

```
# cat args.txt | xargs -I {} ./cecho.sh -p {} -l
-p arg1 -l #
-p arg2 -l #
-p arg3 -l #
```

-I {} 指定了替换字符串，对于每一个命令参数，字符串 {} 会被 stdin 读取到的参数所替换，使用 -I 的时候，命令就似乎是在一个循环中执行一样。如果有三个参数，那么命令就会连同 {} 一起被执行三次，而 {} 在每一次执行中都会被替换为相应的参数。

* 结合 stdin，巧妙运用 while 语句和子 shell

```
# cat files.txt | ( while read arg; do cat $arg; done )
# cat files.txt | xargs -I {} cat {}
```






