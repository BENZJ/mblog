---
title: NASM汇编
date: 2017-05-25 16:10:25
tags:
categories: 学习笔记
---
# NASM汇编
解释chapter3/a/pmtest1.asm中的汇编代码
## 常用寄存器的作用
|通用寄存器|用途|常用于|
|---|---|---|
|eax|累加器|常用于乘、除法和函数返回值|
|ebx|基址(Base)寄存器|常做内存数据的指针, 或者说常以它为基址来访问内存.|
|ecx|计数器(Counter)寄存器|常做字符串和循环操作中的计数器|
|edx|数据(Data)寄存器|常用于乘、除法和 I/O 指针|
|esi|来源索引(Source Index)寄存器	|常做内存数据指针和源字符串指针|
|edi|目的索引(Destination Index)寄存器|常做内存数据指针和目的字符串指针|
|esp|堆栈指针(Stack Point)寄存器	|只做堆栈的栈顶指针; 不能用于算术运算与数据传送|
|ebp|基址指针(Base Point)寄存器|只做堆栈指针, 可以访问堆栈内任意地址, 经常用于中转 ESP 中的数据, 也常以它为基址来访问堆栈; 不能用于算术运算与数据传送|


|指令指针寄存器|用途|常用于|
|---|---|---|
|eip|指令指针寄存器|总是指向下一条指令的地址; 所有已执行的指令都被它指向过.|


|段寄存器|常用于|
|---|---|
|ECS|其值为代码段的段值|
|EES|其值为附加数据段的段值|
|ESS|其值为堆栈段的段值|
|EFS|其值为附加数据段的段值|
|EGS|其值为附加数据段的段值|

## linux二进制查看命令
```bash
xxd -b filename
xxd filename
```

## eax,ax,ah,al的区别
这几个寄存器是整体于部分的关系，eax是32位，ax是eax中的低16位，ah是ax中的高8位，al是ax中的低8位
## 基本命令
### move指令
```	x86asm
mov ax,cs         ;cs寄存器的值赋给ax
mov ds,ax         ;ax的值赋給ds
mov es,ax         ;ax的值赋給es
mov ss,ax         ;ax的值赋給ss
mov sp,0100h      ;把sp的值赋成0100h

```

### xor指令
```	x86asm
xor eax,eax         ;这句话的作用是eax与eax异或操作其再赋值给eax其实意思就是把eax的值设为0
move ax,ds
```
>为何这里不直接用move操作进行赋值：因为move指令比较长，而异或指令比较短

### shl指令
```	x86asm
shl eax,4           ;eax左移4位
```

### add指令
```	x86asm
add eax,LABEL_GDT   ;eax位eax+gdt基地址
mov dword[GdtPtr+2],eax ;
```
>dword 表示移动双字也就是32位

### cli指令
```	x86asm
cli                 ;关中断
```
