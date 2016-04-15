---
title: bash 的环境配置文件
tags: [linux,bash]
categories: [linux,bash]
---

[原文地址](http://vbird.dic.ksu.edu.tw/linux_basic/0320bash.php#settings_bashrc)

## login 与 non-login shell

* login shell：取得 bash 时需要完整的登录流程的，就称为 login shell。举例来说，你要由 tty1 ~ tty6 登录，需要输入用户的账号与密码，此时取得的 bash 就称为 login shell；

* non-login shell：取得 bash 接口的方法不需要重复登录的举动，举例来说，(1)你以 X window 登录 Linux 后， 再以 X 的图形化接口启动终端机，此时那个终端接口并没有需要再次的输入账号与密码，那个 bash 的环境就称为 non-login shell 了。(2)你在原本的 bash 环境下再次下达 bash 这个命令，同样的也没有输入账号密码， 那第二个 bash (子进程) 也是 non-login shell。

之所以要先知道 login shell 和 non-login shell，是因为它们取得 bash 的情况中，读取的配置文件数据并不一样。

一般来说，login shell 其实只会读取这两个配置文件：

1. /etc/profile：这是系统整体的配置，你最好不要修改这个文件；
2. ~/.bash_profile 或 ~/.bash_login 或 ~/.profile：属于使用者个人配置，你要改自己的数据，就修改这里！

## /etc/profile (login shell 才会读)

这个文件是每个使用者登录取得 bash 时一定会读取的配置文件，所以如果你想要帮所有使用者配置整体环境，那就是改这里，不过，没事还是不要随便改这个文件。

## ~/.bash_profile (login shell 才会读)

bash 在读完了整体环境配置的 /etc/profile 后，接下来则是会读取使用者的个人配置文件。 在 login shell 的 bash 环境中，所读取的个人偏好配置文件其实主要有三个，依次分别是：

1. ~/.bash_profile
2. ~/.bash_login
3. ~/.profile

其实 bash 的 login shell 配置只会读取上面三个文件的其中一个， 而读取的顺序则是依照上面的顺序。也就是说，如果 ~/.bash_profile 存在，那么其他两个文件不论有无存在，都不会被读取。 如果 ~/.bash_profile 不存在才会去读取 ~/.bash_login，而前两者都不存在才会读取 ~/.profile 的意思。 

先让我们来看一下 root 的 /root/.bash_profile 的内容是怎样呢？

``` shell
# .bash_profile

# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi

# User specific environment and startup programs

PATH=$PATH:$HOME/bin

export PATH
```

该段的内容指的是: 判断家目录下的 ~/.bashrc 存在否，若存在则读入 ~/.bashrc 的配置。 bash 配置文件的读入方式比较有趣，主要是透过一个命令 source 来读取的，也就是说 ~/.bash_profile 其实会再呼叫 ~/.bashrc 的配置内容.

## source ：读入环境配置文件的命令

由于 /etc/profile 与 ~/.bash_profile 都是在取得 login shell 的时候才会读取的配置文件，所以，如果你将自己的偏好配置写入上述的文件后，通常都是得注销再登陆后，该配置才会生效。那么，能不能直接读取配置文件而不注销登陆呢？ 可以的！那就得要利用 source 这个命令了！

利用 source 或小数点 (.) 都可以将配置文件的内容读进来目前的 shell 环境中！ 举例来说，我修改了 ~/.bashrc，那么不需要注销，立即以 source ~/.bashrc 就可以将刚刚最新配置的内容读进来目前的环境中，还有，包括 ~/bash_profile 以及 /etc/profile 的配置中，很多时候也都是利用到这个 source (或小数点) 的功能。

## ~/.bashrc (non-login shell 会读)

谈完了 login shell 后，那么 non-login shell 这种非登录情况取得 bash 操作接口的环境配置文件又是什么？ 当你取得 non-login shell 时，该 bash 配置文件仅会读取 ~/.bashrc 而已啦！那么默认的 ~/.bashrc 内容是如何？

``` shell
# .bashrc

# User specific aliases and functions

alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# Source global definitions
if [ -f /etc/bashrc ]; then
        . /etc/bashrc
fi
```



