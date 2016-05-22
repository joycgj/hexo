---
title: A collection of Unix/Linux find command examples
date: 2016-04-17 13:16:07
tags: [linux, shell, find, alvinalexander]
categories: [linux, shell, alvinalexander]
---

[A collection of Unix/Linux find command examples](http://alvinalexander.com/unix/edu/examples/find.shtml)

The Linux find command is very powerful. It can search the entire filesystem to find files and directories according to the search criteria you specify. Besides using the find command to locate files, you can also use it to execute other Linux commands (grep, mv, rm, etc.) on the files and directories you find, which makes find extremely powerful.

In this article I’ll take a look at the most common uses of the find command.

## Abridged find command examples

If you just want to see some examples and skip the reading, here are a little more than 30 find command examples to get you started. Almost every command is followed by a short description to explain the command; others are described more fully at the URLs shown:

```
basic 'find file' commands
--------------------------
find / -name foo.txt -type f -print             # full command
find / -name foo.txt -type f                    # -print isn't necessary
find / -name foo.txt                            # don't have to specify "type==file"
find . -name foo.txt                            # search under the current dir
find . -name "foo.*"                            # wildcard
find . -name "*.txt"                            # wildcard
find /users/al -name Cookbook -type d           # search '/users/al'

search multiple dirs
--------------------
find /opt /usr /var -name foo.scala -type f     # search multiple dirs

case-insensitive searching
--------------------------
find . -iname foo                               # find foo, Foo, FOo, FOO, etc.
find . -iname foo -type d                       # same thing, but only dirs
find . -iname foo -type f                       # same thing, but only files

find files with different extensions
------------------------------------
find . -type f \( -name "*.c" -o -name "*.sh" \)                       # *.c and *.sh files, \(\) 将 -name "*.c" -o -name "*.sh" 视作一个整体
find . -type f \( -name "*cache" -o -name "*xml" -o -name "*html" \)   # three patterns

find files that don't match a pattern (-not)
--------------------------------------------
find . -type f -not -name "*.html"                                # find all files not ending in ".html"

find files by text in the file (find + grep)
--------------------------------------------
find . -type f -name "*.java" -exec grep -l StringBuffer {} \;    # find StringBuffer in all *.java files
find . -type f -name "*.java" -exec grep -il string {} \;         # ignore case with -i option
find . -type f -name "*.gz" -exec zgrep 'GET /foo' {} \;          # search for a string in gzip'd files

5 lines before, 10 lines after grep matches
-------------------------------------------
find . -type f -name "*.scala" -exec grep -B5 -A10 'null' {} \;
     (see http://alvinalexander.com/linux-unix/find-grep-print-lines-before-after-...)

find files and act on them (find + exec)
----------------------------------------
find /usr/local -name "*.html" -type f -exec chmod 644 {} \;      # change html files to mode 644
find htdocs cgi-bin -name "*.cgi" -type f -exec chmod 755 {} \;   # change cgi files to mode 755
find . -name "*.pl" -exec ls -ld {} \;                            # run ls command on files found

find and copy
-------------
find . -type f -name "*.mp3" -exec cp {} /tmp/MusicFiles \;       # cp *.mp3 files to /tmp/MusicFiles

copy one file to many dirs
--------------------------
find dir1 dir2 dir3 dir4 -type d -exec cp header.shtml {} \;      # copy the file header.shtml to those dirs

find and delete
---------------
find . -type f -name "Foo*" -exec rm {} \;                        # remove all "Foo*" files under current dir
find . -type d -name CVS -exec rm -r {} \;                        # remove all subdirectories named "CVS" under current dir

find files by modification time
-------------------------------
find . -mtime 1               # 24 hours
find . -mtime -7              # last 7 days
find . -mtime -7 -type f      # just files
find . -mtime -7 -type d      # just dirs

find files by modification time using a temp file
-------------------------------------------------
touch 09301330 poop           # 1) create a temp file with a specific timestamp
find . -mnewer poop           # 2) returns a list of new files
rm poop                       # 3) rm the temp file

find with time: this works on mac os x
--------------------------------------
find / -newerct '1 minute ago' -print

find and tar
------------
find . -type f -name "*.java" | xargs tar cvf myfile.tar
find . -type f -name "*.java" | xargs tar rvf myfile.tar
     (see http://alvinalexander.com/blog/post/linux-unix/using-find-xargs-tar-crea...
     for more information)

find, tar, and xargs
--------------------
find . -name -type f '*.mp3' -mtime -180 -print0 | xargs -0 tar rvf music.tar
     (-print0 helps handle spaces in filenames)
     (see http://alvinalexander.com/mac-os-x/mac-backup-filename-directories-space...)

find and pax (instead of xargs and tar)
---------------------------------------
find . -type f -name "*html" | xargs tar cvf jw-htmlfiles.tar -
find . -type f -name "*html" | pax -w -f jw-htmlfiles.tar
     (see http://alvinalexander.com/blog/post/linux-unix/using-pax-instead-of-tar)
```     
     
On a related note, don’t forget the locate command. It keeps a database on your Unix/Linux system to help find files very fast:

```
locate command
--------------
locate tomcat.sh          # search the entire filesystem for 'tomcat.sh' (uses the locate database)
locate -i spring.jar      # case-insensitive search
```

If you know of any more good find commands to share, please leave a note in the Comments section below.

The remaining sections on this page describe more fully the commands just shown.

## Basic find command examples

This first Linux find example searches through the root filesystem ("/") for the file named Chapter1. If it finds the file, it prints the location to the screen.

```
find / -name Chapter1 -type f -print
```

On Linux systems and modern Unix system you no longer need the -print  option at the end of the find command, so you can issue it like this:

```
find / -name Chapter1 -type f
```

The -type f option here tells the find command to return only files. If you don't use it, the find command will returns files, directories, and other things like named pipes and device files that match the name pattern you specify. If you don't care about that, just leave the -type f option off your command.

This next find command searches through only the /usr and /home directories for any file named Chapter1.txt:

```
find /usr /home -name Chapter1.txt -type f
```

To search in the current directory -- and all subdirectories -- just use the . character to reference the current directory in your find commands, like this:

```
find . -name Chapter1 -type f
```

This next example searches through the /usr directory for all files that begin with the letters Chapter, followed by anything else. The filename can end with any other combination of characters. It will match filenames such as Chapter, Chapter1, Chapter1.bad, Chapter-in-life, etc.:

```
find /usr -name "Chapter*" -type f
```

This next command searches through the /usr/local directory for files that end with the extension .html. These file locations are then printed to the screen.

```
find /usr/local -name "*.html" -type f
```

## Find directories with the Unix find command

Every option you just saw for finding files can also be used on directories. Just replace the -f option with a -d option. For instance, to find all directories named build under the current directory, use this command:

```
find . -type d -name build
```

Find files that don't match a pattern
To find all files that don't match a filename pattern, use the -not argument of the find command, like this:

```
find . -type f -not -name "*.html"
```

That generates a list of all files beneath the current directory whose filename DOES NOT end in .html, so it matches files like *.txt, *.jpg, and so on.

## Finding files that contain text (find + grep)

You can combine the Linux find and grep commands to powerfully search for text strings in many files.

This next command shows how to find all files beneath the current directory that end with the extension .java, and contain the characters StringBuffer. The -l argument to the grep command tells it to just print the name of the file where a match is found, instead of printing all the matches themselves:

```
find . -type f -name "*.java" -exec grep -l StringBuffer {} \;
```

(Those last few characters are required any time you want to exec a command on the files that are found. I find it helpful to think of them as a placeholder for each file that is found.)

This next example is similar, but here I use the -i argument to the grep command, telling it to ignore the case of the characters string, so it will find files that contain string, String, STRING, etc.:

```
find . -type f -name "*.java" -exec grep -il string {} \;
```

## Acting on files you find (find + exec)

This command searches through the /usr/local directory for files that end with the extension .html. When these files are found, their permission is changed to mode 644 (rw-r--r--).

```
find /usr/local -name "*.html" -type f -exec chmod 644 {} \;
```

This find command searches through the htdocs and cgi-bin directories for files that end with the extension .cgi. When these files are found, their permission is changed to mode 755 (rwxr-xr-x). This example shows that the find command can easily search through multiple sub-directories (htdocs, cgi-bin) at one time:

```
find htdocs cgi-bin -name "*.cgi" -type f -exec chmod 755 {} \;
```

## Running the ls command on files you find

From time to time I run the find command with the ls command so I can get detailed information about files the find command locates. To get started, this find command will find all the *.pl files (Perl files) beneath the current directory:

```
find . -name "*.pl"
```

In my current directory, the output of this command looks like this: 

```
./news/newsbot/old/3filter.pl
./news/newsbot/tokenParser.pl
./news/robonews/makeListOfNewsURLs.pl
```

That's nice, but what if I want to see the last modification time of these files, or their filesize? No problem, I just add the ls -ld command to my find command, like this:

```
find . -name "*.pl" -exec ls -ld {} \;
```

This results in this very different output:

```
-rwxrwxr-x 1 root root 2907 Jun 15  2002 ./news/newsbot/old/3filter.pl
-rwxrwxr-x 1 root root 336  Jun 17  2002 ./news/newsbot/tokenParser.pl
-rwxr-xr-x 1 root root 2371 Jun 17  2002 ./news/robonews/makeListOfNewsURLs.pl
```

The "-l" flag of the ls command tells ls to give me a "long listing" of each file, while the -d flag is extremely useful in this case; it tells ls to give me the same output for a directory. Normally if you use the ls command on a directory, ls will list the contents of the directory, but if you use the -d option, you'll get one line of information, as shown above.

## Find and delete
Be very careful with these next two commands. If you type them in wrong, or make the wrong assumptions about what you're searching for, you can delete a lot of files very fast. Make sure you have backups and all that, you have been warned.

Here's how to find all files beneath the current directory that begin with the letters 'Foo' and delete them.

```
find . -type f -name "Foo*" -exec rm {} \;
```

This one is even more dangerous. It finds all directories named CVS, and deletes them and their contents. Just like the previous command, be very careful with this command, it is dangerous(!), and not recommended for newbies, or if you don't have a backup.

```
find . -type d -name CVS -exec rm -r {} \;
```

## Find files with different file extensions

The syntax to find multiple filename extensions with one command looks like this:

```
find . -type f \( -name "*.c" -o -name "*.sh" \)
```

Just keep adding more "-o" (or) options for each filename extension. Here's a link to

## Case-insensitive file searching

To perform a case-insensitive search with the Unix/Linux find command, use the -iname option instead of -name. So, to search for all files and directories named foo, FOO, or any other combination of uppercase and lowercase characters beneath the current directory, use this command:

```
find . -iname foo
```

If you're just interested in directories, search like this:

```
find . -iname foo -type d
```

And if you're just looking for files, search like this:

```
find . -iname foo -type f
```

## Find files by modification time

To find all files and directories that have been modified in the last seven days, use this find command:

```
find . -mtime -7
```

To limit the output to just files, add the "-type f" option as shown earlier:

```
find . -mtime -7 -type f
```

and to show just directories:

```
find . -mtime -7 -type d
```

## More find command resources

If you're just looking for a file by name, and you want to be able to find that file even faster than you can with the find command, take a look at the Linux locate command. The locate command keeps filenames in a database, and can find them very fast.

For more details on the find command, check out our online version of the find man page.

Also, if you have any favorite Linux and Unix find commands you'd like to share, please use our comment form below.