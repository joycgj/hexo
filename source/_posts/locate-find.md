---
title: locate find
---

[原文地址](http://blog.chinaunix.net/uid-24648486-id-2998767)
## locate

locate这个命令是对其生成的数据库进行遍历（生成数据库的命令：updatedb）,这一特性决定了用locate查找文件速度很快，但是locate命令只能对文件进行模糊匹配，在精确度上来说差了点，简单介绍下它的两个选项：

``` bash
-i  //查找文件的时候不区分大小写,比如：locate  –i   passwd
-n  //只显示查找结果的前N行,比如：locate  -n  5   passwd
```

## find

find在不指定查找目录的情况下是对整个系统进行遍历查找

使用格式 ：   find  [指定查找目录]  [查找规则]  [查找完后执行的action]

### 查找规则

#### 根据文件名查找

    -name   // 根据文件名查找（精确查找）
    -iname  // 根据文件名查找，但是不区分大小写
    
    find /etc -name passwd
    
    /etc/passwd
    /etc/pam.d/passwd

####  根据文件所属用户和组来查找文件

```
-user         //根据属主来查找文件
-group        //根据属组来查找文件
```

```
f // 普通文件
d // 目录文件
l // 链接文件
b // 块设备文件
c // 字符设备文件
p // 管道文件
s // socket文件


```
### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)
