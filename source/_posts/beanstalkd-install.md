---
title: beanstalkd安装
date: 2016-10-09 17:20:11
tags: [beanstalkd,gcc,queue]
categories: [beanstalkd,gcc,queue]
---

## insall gcc

参考：
http://blog.patpig.com/2015/09/04/beanstalkd-a-simple-fast-work-queue/
http://www.hiceon.com/topic/centos-6-config-163-yum/

make时报错误：

```
make cc command not found
```

需要安装 gcc，结果 yum install gcc 时报：

```
Error: Cannot retrieve repository metadata (repomd.xml) for repository: base. Please verify its path and try again
```

发现 yum 源有问题

参考 http://www.hiceon.com/topic/centos-6-config-163-yum/，一下是我的 /etc/yum.repos.d/ 下的文件列表

```
Centos-6.repo  local.repo  nginx.repo
```

我把local.repo删除了

然后把下载下来的 repo 重命名为 Centos-6.repo，执行

```
yum clean all
yum makecache
```

之后就可以安装gcc了，也可以安装beanstalkd了。

