---
title: makefile入门
date: 2017-06-13 20:28:13
tags: 汇编　linux
---
# makefile入门

接上一片博客的代码如下
```asm
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

为得到可执行文件应该使用如下命令：
```bash
nasm -f elf64 -g -F stabs eatsyscall.asm  # 通过eatsyscall.asm生成eatsyscall.o
ld -o eatsyscall eatsyscall.o #通过eatsyscall.o生成可执行文件eatsyscall
```

# makefile的编写
```makefile
eatsyscall : eatsyscall.o
        ld -o eatsyscall eatsyscall.o
eatsyscall.o: eatsyscall.asm
        nasm -f elf64 -g -F stabs eatsyscall.asm
```
首相要说明生成的文件顺序不能换，冒号前面是要生成的文件，冒号后面是依赖的文件，可以写多个，下一行生成的命令一定要与上一行有缩进不然会报错。
