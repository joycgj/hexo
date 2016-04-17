---
title: nohup-真正的Shell后台运行
date: 2016-04-16 15:37:18
tags: [linux,shell,nohup]
categories: [linux,shell,nohup]
---

[原文地址](http://blog.csdn.net/taiyang1987912/article/details/39559087)

## 1. &方式：

Unix/Linux 下一般想让某个程序在后台运行，很多都是使用 & 在程序结尾来让程序自动运行。比如我们要运行 mysql 在后台： 

/usr/local/mysql/bin/mysqld_safe --user=mysql &

## 2. nohup方式:

但是我们很多程序并不像 mysqld 一样可以做成守护进程，可能我们的程序只是普通程序而已，一般这种程序即使使用 & 结尾，如果终端关闭，那么程序也会被关闭。为了能够后台运行，我们需要使用 nohup 这个命令，比如我们有个 start.sh 需要在后台运行，并且希望在后台能够一直运行，那么就使用 nohup： 
            
nohup /root/start.sh & 
            
在 shell 中回车后提示：
 
appending output to nohup.out 
          
原程序的的标准输出被自动改向到当前目录下的 nohup.out 文件，起到了log的作用。

## 3. nohup 问题

但是有时候在这一步会有问题，当把终端关闭后，进程会自动被关闭，察看 nohup.out 可以看到在关闭终端瞬间服务自动关闭。

有个操作终端时的细节：当 shell 中提示了 nohup 成功后还需要按终端上键盘任意键退回到 shell 输入命令窗口，然后通过在 shell 中输入 exit 来退出终端；而我是每次在 nohup 执行成功后直接点关闭程序按钮关闭终端。所以这时候会断掉该命令所对应的 session，导致 nohup 对应的进程被通知需要一起 shutdown。

这个细节有人和我一样没注意到，所以在这儿记录一下了。

## 4. nohup 命令参考 

nohup 命令
用途：不挂断地运行命令。 
语法：nohup Command [ Arg ... ] [　& ] 
描述：nohup 命令运行由 Command 参数和任何相关的 Arg 参数指定的命令，忽略所有挂断（SIGHUP）信号。在注销后使用 nohup 命令运行后台中的程序。要运行后台中的 nohup 命令，添加 & （ 表示"and"的符号）到命令的尾部。 
　　
无论是否将 nohup 命令的输出重定向到终端，输出都将附加到当前目录的 nohup.out 文件中。如果当前目录的 nohup.out 文件不可写，输出重定向到 $HOME/nohup.out 文件中。如果没有文件能创建或打开以用于追加，那么 Command 参数指定的命令不可调用。如果标准错误是一个终端，那么把指定的命令写给标准错误的所有输出作为标准输出重定向到相同的文件描述符。 
　　
退出状态：该命令返回下列出口值：
 　　
126 可以查找但不能调用 Command 参数指定的命令。 
127 nohup 命令发生错误或不能查找由 Command 参数指定的命令。 
否则，nohup 命令的退出状态是 Command 参数指定命令的退出状态。 

　　
nohup命令及其输出文件
 
nohup命令：如果你正在运行一个进程，而且你觉得在退出帐户时该进程还不会结束，那么可以使用nohup命令。该命令可以在你退出帐户/关闭终端之后继续运行相应的进程。nohup就是不挂起的意思( no hang up)。 

该命令的一般形式为：nohup command & 
　
使用nohup命令提交作业 
　　
如果使用nohup命令提交作业，那么在缺省情况下该作业的所有输出都被重定向到一个名为nohup.out的文件中，除非另外指定了输出文件： 
　　
nohup command > myout.file 2>&1 & 
　　
在上面的例子中，输出被重定向到myout.file文件中。 

```
linux shell下常用输入输出操作符是：

1.  标准输入   (stdin) ：代码为 0，使用 < 或 <<； /dev/stdin -> /proc/self/fd/0   0代表：/dev/stdin 
2.  标准输出   (stdout)：代码为 1，使用 > 或 >>； /dev/stdout -> /proc/self/fd/1  1代表：/dev/stdout
3.  标准错误输出(stderr)：代码为 2，使用 2> 或 2>>； /dev/stderr -> /proc/self/fd/2 2代表：/dev/stderr
```

[linuxshell中"2>&1"含义](http://os.chinaunix.net/a2009/0903/996/000000996941.shtml)
　　
使用 jobs 查看任务。 
　　
使用 fg %n　关闭。 
　　
另外有两个常用的ftp工具ncftpget和ncftpput，可以实现后台的ftp上传和下载，这样就可以利用这些命令在后台上传和下载文件了。
