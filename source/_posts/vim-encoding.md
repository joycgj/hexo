---
title: vim 乱码
---

## 现象

cat 文件不乱码，vim文件乱码，且提示没有vim这个命令

```
[root@docker100254 ~]# vim test
-bash: vim: command not found
```

## 解决方案



### 查看系统版本：
```
[root@docker100254 ~]# cat /proc/version
Linux version 3.10.0-327.18.2.el7.x86_64 (builder@kbuilder.dev.centos.org) (gcc version 4.8.3 20140911 (Red Hat 4.8.3-9) (GCC) ) #1 SMP Thu May 12 11:03:55 UTC 2016
```

### Centos安装vim

安装之前：

```
[root@docker100254 ~]# rpm -qa | grep vim
vim-minimal-7.4.629-5.el6.x86_64
```

Centos里的VI只默认安装了vim-minimal－7.x。所以无论是输入vi或者 vim查看文件，syntax功能都无法正常启用。因此需要用yum安装另外两个组件：vim-common-7.x和vim-enhanced- 7.x。
命令行里敲入：

yum -y install vim-enhanced

安装之后

```
[root@docker100254 ~]# rpm -qa | grep vim
vim-filesystem-7.4.629-5.el6.x86_64
vim-enhanced-7.4.629-5.el6.x86_64
vim-minimal-7.4.629-5.el6.x86_64
vim-common-7.4.629-5.el6.x86_64
```

### 解决乱码

编辑~/.vimrc

```
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
```

参考 http://www.cnblogs.com/joeyupdo/archive/2013/03/03/2941737.html