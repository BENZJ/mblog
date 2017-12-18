---
title: matlab基本语法
date: 2017-05-31 16:36:03
tags:
categories: 应用
---
# matlab grammar

## Basic Operation

### elementary math operations
```	bash
#  elementary math operations
octave:1> 5+6
ans =  11
octave:2> 1/2
ans =  0.50000
octave:3> 2^6
ans =  64
# logical operations
octave:4> 1==2 % false
ans = 0
octave:5> 1~=2 % one is not equal two   true
ans =  1
octave:6> 1 && 2 %AND
ans =  1
octave:7> 1 || 0 % OR
ans =  1
octave:8> xor(1,0)
ans =  1

```
### change the prompt
```bash
# change the prompt
octave:9> PS1('>> ')
>>
```
### variables
```bash
# variables
>> a = 3
a = 3
>> a = 3; # You want to assign a variable, but you don't want to print out the result. If you put a semicolon, the semicolon suppresses the print output.
>>
```
### print
```bash
# print
>> disp(sprintf('2 decimals = %.2f',a))
2 decimals = 3.14
>> sprintf('%.2f',a)
ans = 3.14
>>
```
### vectors and matrices
```bash
>> A = ones(2,3)
A =

   1   1   1
   1   1   1

>> C = A*2
C =

   2   2   2
   2   2   2

>> C = 2*A
C =

   2   2   2
   2   2   2

>> rand(1,3)
ans =

   0.577424   0.066718   0.250353

>> eye(4)
   ans =

   Diagonal Matrix

      1   0   0   0
      0   1   0   0
      0   0   1   0
      0   0   0   1
```

## Move Data Around

### show the variables in the scope
```bash
>> who
Variables in the current scope:

A    C    V    a    ans  w

>> whos
Variables in the current scope:

   Attr Name        Size                     Bytes  Class
   ==== ====        ====                     =====  =====
        A           3x2                         48  double
        C           2x3                         48  double
        V           1x6                         24  double
        a           1x1                          8  double
        ans         1x2                         16  double
        w           1x10000                  80000  double

Total is 10021 elements using 80144 bytes

```

### load Data
```bash
>>load('flile.dat') % file.date is the name of file
```
