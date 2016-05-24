---
title: 如何在shell中判断一个文件是否为空
date: 2016-05-24 23:39:18
tags: [linux,shell]
categories: [linux,shell]
---

在 Linux 中写脚本的时候，总免不了需要判断文件是否存在、文件内容是否为空等存在，而这些操作都可以用 test 指令来实现，通过 man test 指令可以查看关于test指令的手册，手册中有如下说明： 


```
-s FILE
              FILE exists and has a size greater than zero
              如果文件存在且文件大小大于零，则返回真
              
-e FILE
              FILE exists
              如果文件存在，则返回真
```


在shell中通过test指令测试文件是否为空的示例脚本如下：


```
#!/bin/sh

if test -s file; then
	echo "not empty"
else
	echo "empty"
fi
```


在shell中，test指令还有另外一种写法，上面的脚本和下面的脚本是等价的：


```
#!/bin/sh

if [ -s file ]; then
	echo "not empty"
else
	echo "empty"
fi
```


[原文地址](http://www.letuknowit.com/topics/20120402/linux-shell-test-command.html/)