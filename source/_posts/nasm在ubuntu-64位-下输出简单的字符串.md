---
title: nasm在ubuntu(64位)下输出简单的字符串
date: 2017-06-13 10:08:11
tags:
---
# nasm在ubuntu(64位)下输出简单的字符串
这个例子是书本汇编语言：基于LINUX环境（第3版)中的例子代码如下。
```nasm
;eatsyscall.asm

SECTION .data

EatMsg: db "Eat at Joe's" , 10
EatLen: equ $-EatMsg

SECTION .bass
SECTION .text

global _start

_start:
        nop
        mov eax,4
        mov ebx,1
        mov ecx,EatMsg
        mov edx,EatLen
        int 80H
        mov eax,1
        mov ebx,0
        int 80H

```
在书本中用的是32位的系统所以编译命令如下
```bash
nasm -f elf -g -F stabs eatsyscall.asm
```
编译后进行链接
```bash
ld -o eatsyscall eatsyscall.o
```

若运行的是64位的系统就会发现如下错误
```bash
$ ld -o eatsyscall eatsyscall.o
ld: i386 architecture of input file `eatsyscall.o' is incompatible with i386:x86-64 output
```
# 解决方法
```bash
＃将nasm -f elf -g -F stabs eatsyscall.asm  改为如下

nasm -f elf64 -g -F stabs eatsyscall.asm  
```
