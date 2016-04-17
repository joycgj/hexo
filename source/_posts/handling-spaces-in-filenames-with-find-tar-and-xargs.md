---
title: Mac backups - handling spaces in filenames with find, tar, and xargs
date: 2016-04-17 14:41:26
tags: [linux, shell, tar, xargs, alvinalexander]
categories: [linux, shell, alvinalexander]
---

[原文地址](http://alvinalexander.com/mac-os-x/mac-backup-filename-directories-spaces-find-tar-xargs)

This morning I decided to take a few minutes to backup all the songs I've purchased over the last half-year. These are all on my Mac OS X system, under the Music folder in my home directory.

The problem with trying to do this with standard Unix tools is that all these subdirectories and filenames have spaces in their names. Just looking at the Music folder, it contains many directory names like this:

```
Willie Nelson & Merle Haggard
```

If you know Unix, you know spaces in directory and filenames wreaks havoc with Unix tools. Fortunately we can get around this problem on Mac OS X systems, as you'll see here.

## A `find` + `tar` command to handle spaces

After opening a Mac Terminal window and changing to the Music folder on my computer (/Users/Al/Music), all I needed was this one command to create a tar file of all the songs on my system that have been modified in the last 180 days (which is essentially six months):

```
find . -name -type f '*.mp3' -mtime -180 -print0 | xargs -0 tar rvf music.tar
```

This command successfully created the file named music.tar, which I just copied to another Mac and tested there.

The important part of this solution is that it shows how to handle spaces in filenames and directory names when using the find and tar commands. The secret is using the find command’s -print0 option, and the -0 argument for xargs, both of which are explained below.

## Breaking the commands down

What this find command means:

* find is the Unix find command
* -type f means just look for files
* -name '*.mp3' means any filename that ends with '.mp3'
* -mtime -180 means only return files that have been modified in the last 180 days
* -print0 is a find option specifically meant to work with xargs (see below)
* 
Here’s what the xargs and tar commands mean:

* xargs is a Unix command that can read long argument lists and feed them to whatever other program you like, in this case the tar command. (see below for more)
* -0 tells xargs to expect NUL characters as line separators
* tar is the standard Unix and Linux "tape archive" utility, used mostly these days to create backup files
* 'r' both creates and then appends to the filename
* 'v' means verbose, and shows me the filenames as they are written to the output file
* 'f' is used to specify the filename that follows (music.tar)

**The xargs command is often used when you have a lot of input that would normally be sent to a command like tar, but tar can't handle all that input at once. xargs specializes in taking the input, finding each line in the input, and then sending the lines — one at a time — to the command you specify, which is that tar command in this example.**

## Using "find -print0" and "xargs -0" together

The "find -print0" and "xargs -0" options are expected to be used together. Here is some documentation from the man pages on these two commands that explains this.

First, some information on the find command's print0 option:

```
This primary always evaluates to true. It prints the pathname 
of the current file to standard output, followed by an ASCII NUL
character (character code 0).

(good to use with "xargs -0")
```

And here's some information on the xargs -0 option:

```
Change xargs to expect NUL ("\0") characters as separators, 
instead of spaces and newlines. This is expected to be used 
in concert with the -print0 function in find.
```
 
## Mac backups, find, tar, and spaces - Summary

I hope this tip on backing up files and directories with spaces in their names has been helpful. As you have seen, with a little tweaking, the find, tar, and xargs commands can help you create a tar backup of your Music, and any other content with spaces in their file and directory names.