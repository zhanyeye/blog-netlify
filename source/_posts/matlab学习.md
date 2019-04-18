---
title: matlab学习
date: 2019-04-08 15:39:41
tags:
- matlab
categories:
- 数学建模
---



###### 矩阵操作

对应位置相乘 `A .* B`

对应位置相除 `A ./ B`

对应位置平方 `A .^ B`

<!--more-->

```matlab
>> A = [1 2 3; 4 5 6; 7 8 9];
>> x = A(1, 3)

x =

     3
A =

     1     2     3
     4     5     6
     7     8     9

>> A(6)  %matlab 单索引是竖着数的

ans =

     8

>> A(1, :) %取出第一行

ans =

     1     2     3
     
>> A(1:2, 1:3) %取出第 1, 2 行

ans =

     1     2     3
     4     5     6
     

>> A(2, :) = [1 1 1] %给某一行赋值

A =

     1     2     3
     1     1     1
     7     8     9
     
>> A(1:2, 1:2) = [-1 -2; -3 -4]

A =

    -1    -2     3
    -3    -4     1
     7     8     9
     
```

###### 数组比较和逻辑运算

```matlab
%比较和逻辑运算     
>> x = [1 2 3 4 5 6 7 8 9];
>> y = [1 4 3 8 5 6 7 8 9];
>> eq = (x == y)   % 找出数组x和y中相等的元素

eq =

  1×9 logical array

   1   0   1   0   1   1   1   1   1

>> ep = (x>5)&(y<7)   % 找出数组x, y 中 x >5 && y < 7

ep =

  1×9 logical array

   0   0   0   0   0   1   0   0   0


>> xoy = xor(x>5, y<7)  % 异或，一个真一个假才为真

xoy =

  1×9 logical array

   1   1   1   0   1   0   1   1   1

```

```matlab
>> x = [1 -2 3 -4 5 -6 7 -8 9];
>> x(x<0) = 0  %把数组x 中小于0 的都变成0
x =
     1     0     3     0     5     0     7     0     9
```

```matlab
>> y = [1 2 3; -4 5 6; 7 8 9];
>> y(y(:,1)<0,:) = 0   %将第一列小于0 的那一行赋值为0
y =
     1     2     3
     0     0     0
     7     8     9
```



###### 数组操作函数：`flipud`,  `fliplr`, `rot90` , `sum` , `max`, `min`

`flipud `矩阵上下旋转

```matlab
>> A = [1 2 3; 4 5 6; 7 8 9];
>> B = flipud(A)
B =
     7     8     9
     4     5     6
     1     2     3
```

`fliplr` 矩阵左右旋转

```matlab
>> A = [1 2 3; 4 5 6; 7 8 9];
>> C = fliplr(A)
C =
     3     2     1
     6     5     4
     9     8     7
```

`rot90` 逆时针旋转90°

```matlab
>> A = [1 2 3; 4 5 6; 7 8 9];
>> D = rot90(A)
D =
     3     6     9
     2     5     8
     1     4     7
```

`sum` 求和

```matlab
>> A = [1 2 3];
>> sum(A)
ans =
     6
     
>> B = [1 2 3; 4 5 6; 7 8 9];
>> sum(B)  %sum对矩阵求和，是把每一列相加 => sum(B,1)
ans =
    12    15    18
    
>> sum(B, 2)  %sum对第2维度相加，即每一行求和
ans =
     6
    15
    24
    

>> sum(sum(B))
ans =
    45
    
>> sum(B(:))
ans =
    45
    
>> C = B(:)  %将B拉成一个列向量
C =
     1
     4
     7
     2
     5
     8
     3
     6
     9
```

`max`, `min`

```matlab
>> A = [1 2 3];
>> max(A)
ans =
     3
     
>> max(A,2)  %将2和数组中每一个元素比较，返回大的，也是一个数组
ans =
     2     2     3
     
>> B = [1 3 9; 4 8 6];
>> max(B)   %当是一个二维数组时取，每一列的最大值 = max(B, [], 1)
ans =
     4     8     9
     
>> max(B, [], 2)   %对每一行去最大值
ans =
     9
     8
```

###### 常用的数学函数：

`sin`,  `cos`,  `tan`,  `cot`,  `asin`,  `acos`,  `atan`,  `acot`

`abs`, `sqrt`

`ceil`,  `floor` , `fix` (向0靠), `round ` (四舍五入取整)



###### 基本语句

```matlab
for .. end
if .. else .. end
while .. end
switch .. case .. end
```

![untitled](/images/untitled-1554820079963.png)