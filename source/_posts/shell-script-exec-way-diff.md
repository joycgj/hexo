---
title: shell script 的执行方式区别（source,sh script,./script）
date: 2016-04-15 14:58:01
tags: [linux,shell]
categories: [linux,shell]
---

## 直接执行

系统会提供一个子进程 bash 让我们让来执行 shell 脚本，当脚本执行完了之后，子进程中的所有数据便被删除。

## source方式

当用 source 方式执行时，会在父进程中执行 shell 脚本，因此各项操作都会在父进程的 bash 内有效，这也是你不注销系统而要让某些写入 ~/.bashrc 的设置生效时，需要使用 source ~/.bashrc 而不能使用 bash ~/.bashrc 的原因。