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

`NAME
     test, [ -- condition evaluation utility

SYNOPSIS
     test expression
     [ expression ]

DESCRIPTION
     The test utility evaluates the expression and, if it evaluates to true, returns a zero (true) exit status; otherwise it returns 1 (false).  If there is no
     expression, test also returns 1 (false).

     All operators and flags are separate arguments to the test utility.

     The following primaries are used to construct expressions:

     -b file       True if file exists and is a block special file.

     -c file       True if file exists and is a character special file.

     -d file       True if file exists and is a directory.

     -e file       True if file exists (regardless of type).

     -f file       True if file exists and is a regular file.

     -g file       True if file exists and its set group ID flag is set.

     -h file       True if file exists and is a symbolic link.  This operator is retained for compatibility with previous versions of this program. Do not rely on
                   its existence; use -L instead.

     -k file       True if file exists and its sticky bit is set.

     -n string     True if the length of string is nonzero.

     -p file       True if file is a named pipe (FIFO).

     -r file       True if file exists and is readable.

     -s file       True if file exists and has a size greater than zero.

     -t file_descriptor
                   True if the file whose file descriptor number is file_descriptor is open and is associated with a terminal.

     -u file       True if file exists and its set user ID flag is set.

     -w file       True if file exists and is writable.  True indicates only that the write flag is on.  The file is not writable on a read-only file system even
                   if this test indicates true.

     -x file       True if file exists and is executable.  True indicates only that the execute flag is on.  If file is a directory, true indicates that file can
                   be searched.

     -z string     True if the length of string is zero.

     -L file       True if file exists and is a symbolic link.

     -O file       True if file exists and its owner matches the effective user id of this process.

     -G file       True if file exists and its group matches the effective group id of this process.

     -S file       True if file exists and is a socket.

     file1 -nt file2
                   True if file1 exists and is newer than file2.

     file1 -ot file2
                   True if file1 exists and is older than file2.

     file1 -ef file2
                   True if file1 and file2 exist and refer to the same file.

     string        True if string is not the null string.

TEST(1)                   BSD General Commands Manual                  TEST(1)

NAME
     test, [ -- condition evaluation utility

SYNOPSIS
     test expression
     [ expression ]

DESCRIPTION
     The test utility evaluates the expression and, if it evaluates to true, returns a zero (true) exit status; otherwise it returns 1 (false).  If there is no
     expression, test also returns 1 (false).

     All operators and flags are separate arguments to the test utility.

     The following primaries are used to construct expressions:

     -d file       True if file exists and is a directory.

     -e file       True if file exists (regardless of type).

     -f file       True if file exists and is a regular file.

     -n string     True if the length of string is nonzero.

     -r file       True if file exists and is readable.

     -s file       True if file exists and has a size greater than zero.

     -w file       True if file exists and is writable.  True indicates only that the write flag is on.  The file is not writable on a read-only file system even
                   if this test indicates true.

     -x file       True if file exists and is executable.  True indicates only that the execute flag is on.  If file is a directory, true indicates that file can
                   be searched.

     -z string     True if the length of string is zero.

     file1 -nt file2
                   True if file1 exists and is newer than file2.

     file1 -ot file2
                   True if file1 exists and is older than file2.

     file1 -ef file2
                   True if file1 and file2 exist and refer to the same file.

     string        True if string is not the null string.

     s1 = s2       True if the strings s1 and s2 are identical.

     s1 != s2      True if the strings s1 and s2 are not identical.

     s1 < s2       True if string s1 comes before s2 based on the ASCII value of their characters.

     s1 > s2       True if string s1 comes after s2 based on the ASCII value of their characters.

     n1 -eq n2     True if the integers n1 and n2 are algebraically equal.

     n1 -ne n2     True if the integers n1 and n2 are not algebraically equal.

     n1 -gt n2     True if the integer n1 is algebraically greater than the integer n2.

     n1 -ge n2     True if the integer n1 is algebraically greater than or equal to the integer n2.

     n1 -lt n2     True if the integer n1 is algebraically less than the integer n2.

     n1 -le n2     True if the integer n1 is algebraically less than or equal to the integer n2.

     These primaries can be combined with the following operators:

     ! expression  True if expression is false.

     expression1 -a expression2
                   True if both expression1 and expression2 are true.

     expression1 -o expression2
                   True if either expression1 or expression2 are true.

     (expression)  True if expression is true.

     The -a operator has higher precedence than the -o operator.

RETURN VALUES
     The test utility exits with one of the following values:

     0       expression evaluated to true.

     1       expression evaluated to false or expression was missing.

     >1      An error occurred.

```

[原文地址](http://www.letuknowit.com/topics/20120402/linux-shell-test-command.html/)