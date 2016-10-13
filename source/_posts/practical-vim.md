---
title: practical vim 简记 
date: 2016-05-24 23:39:18
tags: [linux,shell]
categories: [linux,vim]
---


```
<C-h> 删除前一个字符（同退格键）
<C-w> 删除前一个单词
<C-u> 删至行首
```

```
<Esc> 等于 <C-[>
在插入模式,<C-o>,执行普通模式下的命令之后自动又进入插入模式
```

```
Practical Vim, by Drew Neil
Read Drew Neil's

TO

Practical Vim, by Drew Neil
Read Drew Neil's Practical Vim.

yt,
jA空格
<C-r>0
.<Esc>

f{char} F{char} 把光标移动到指定字符{char}上;
t{char} T{char} 把光标移动到{char}前面的那个字符上；
```

## 技巧59

### 调换字符

```
Practica lvim

TO

Practical vim

F空格
x
p

one word: xp
```

### 调换文本行

```
2) line two
1) line one
3) line three

TO

1) line one
2) line two
3) line three

dd
p

one word: ddp
```

### 复制文本行

```
1) line one
2) line two

TO

1) line one
2) line two
2) line two

yy
p

one word: yyp
```

xp, ddp, yyp, yiw, diw, jww, yap, gP

## 深入理解vim寄存器

```
"{register}  指定要用的寄存器，若不指明，vim将缺省使用无名寄存器
"_d{motion}  引用黑洞寄存器，只删除内容而不把内容复制到任何寄存器

""           无名寄存器用两个双引号表示，即 ""p 和 p 是一样的命令
"0			   复制专用寄存器

当 y{motion} 时，不仅会复制到无名寄存器中，也会复制到复制专用寄存器中

"ayiw 			把当前单词复制到寄存器 a 中 
"bdd			把当前整行文本剪切至寄存器 b 中
```

```
collection = getCollection();
process(somethingInTheWay, target);

TO

collection = getCollection();
process(collection, target);

有5种实现方式
```

### 使用复制寄存器解决问题

```
yiw
jww
diw
"0P    大p表示粘贴到逗号前面
```

### 使用有名寄存器解决问题

```
"ayiw
jww
diw
"aP
```

### 使用黑洞寄存器解决问题

```
yiw
jww
"_diw
P
```

### 使用可视模式

```
yiw
jww
ve
p

u
gv
"0p
```

### 使用<C-r>0

```
yiw
jww
ciw<C-r>0<Esc>
```

## 相互复制

### 从外部复制到vim

```
从外部复制文本, 在vim普通模式下 "+p 或者在插入模式下 <C-r>+ 可以将其粘贴到vim内部
```

### 从vim复制到外部

```
在vim下选中之后 "+y, 就可以在外部进行粘贴操作了
```

交换两个单词

```
I like chips and fish.

TO

I like fish and chips.

fc
de
mm		负责设置标记
ww
ve
p
`m		将跳转到该标记
P
```


