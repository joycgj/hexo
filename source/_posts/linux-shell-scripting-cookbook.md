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

* -print0: 

