---
title: shell script 的追踪与调试
date: 2016-04-17 12:46:43
tags: [linux,shell,debug]
categories: [linux,shell,debug]
---

## 语法

```
sh [-nvx] scripts.sh

-n : 不要执行 script, 仅查询语法的问题;
-v : 在执行 script 前, 先将 script 的内容输出到屏幕上;
-x : 将使用到的 script 内容显示到屏幕上，这是很有用的参数;
```

## shell script forseq.sh

```bash
#!/bin/sh

for animal in dog cat elephant
do
    echo "There are ${animal}s..."
done
```

```
$ sh -n forseq.sh
# 若语法没有问题，则不会显示任何信息

$ sh -v forseq.sh
#!/bin/sh

for animal in dog cat elephant
do
    echo "There are ${animal}s..."
done
There are dogs...
There are cats...
There are elephants...

$ sh -x forseq.sh
+ for animal in dog cat elephant
+ echo 'There are dogs...'
There are dogs...
+ for animal in dog cat elephant
+ echo 'There are cats...'
There are cats...
+ for animal in dog cat elephant
+ echo 'There are elephants...'
There are elephants...
```
