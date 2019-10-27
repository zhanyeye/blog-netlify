---
title: Chap1 MATLAB 入门基础
date: 2019-04-13 13:39:23
tags:
- matlab
categories:
- 数学建模
---



###### 数据的输入

```matlab
% 换行分号，行末分号
A = [1 2 3; 4 5 6; 7 8 10]
% 加减乘除，幂
a=29*(2+23/3)-5^2
% 返回值=函数名（参数1，参数2，……）
a=magic(3)
```

<!--more-->

###### Tab键补全功能，语法提示，错误纠正

```matlab
>> Sin(pi) 
Did you mean:
>> sin(pi)

>> a=[1 2 4 9;16 25 36 49];
>> a([3 6])=nan;  %将数组第6个元素和第7个元素赋为NaN, 注意数组元素下标是竖着数的
>> mean(a)  %求平均数，默认是对对第一维度，即列 => mean(a, 1)
ans =
    8.5000       NaN       NaN   29.0000
    
>> mean(a,'omitnan')  %忽略NaN
ans =
    8.5000   25.0000    4.0000   29.0000
    
>> mean(a,2,'omitnan')   %对第二维度求平均值，忽略NaN

ans =
    4.6667
   30.0000
```



###### 科学计数法

```matlab
>> a=10000000000000000000000000000000000000
a =
   1.0000e+37
```



###### 相对精度

eps  =  2.2204e-16

```matlab
>> a=10000000000000000;   %1E16
>> b=10000000000000001;
>> c=a-b
c =
     0

  	 
>> a1=1000000000000000;   %1E15
>> b1=1000000000000001;
>> c1=a1-b1
c1 =
    -1
    
```



###### ans，NaN，inf，eps，i，j，pi

```matlab
>> 2+3
ans = 5
     
>> 0/0
ans = NaN
   
>> 2/0
ans = Inf
   
>> i
ans = 0.0000 + 1.0000i
   
>> j
ans = 0.0000 + 1.0000i

>> 2+3i
ans = 2.0000 + 3.0000i

>> pi
ans = 3.1416
```



###### 开立方根

```matlab
>> a=-8;
>> r=a^(1/3)
r =
   1.0000 + 1.7321i  %默认放回第一象限解

m=[0,1,2];                    	%  为3个方根而设
R=abs(a)^(1/3)              	%  模的开3次方
theta=(angle(a)+2*pi*m)/3 	%  -pi<theta<=pi的3个相位角
r=R*exp(i*theta)             	%  将得到的结果赋给r
```



###### 命令行换行输入

`>> a=1 `     %  按Shift+Enter快捷键暂不执行此行命令，并进入下一行输入
`b=2  `     %  按 Shift+Enter快捷键进入下一行输入，**此时还可以编辑本行或上面一行命令**
`c=a+b`    %  按回车键运行全部3行命令

当用户输入有关键词的多行循环命令时，例如for和end，并不需要使用Shift+Enter快捷键，直接按回车键即可进入下一行输入，直到完成了循环体之后，MATLAB才会将各行程序一起执行。

```matlab
for r=1:5    	%  按回车键
a=pi*r^2       	%  按回车键
end           	%  按回车键并执行循环体内的命令
```



###### 在同一行内输入多个函数

`x = (1:10)'; logs = [x log10(x)]`



###### 长命令行的分行输入

```matlab
headers = ['Author First Name, Author Middle Initial ' ...
'Author Last Name ']
```

**标识符(...)如果出现在两个单引号的中间，MATLAB则会报错**

###### Format long /short /compact/loose 调整数据显示格式

`format long`	  %长整型显示格式
`format short`	 %短整型显示格式
`format compact`   %压缩空格
`format loose`	%松散



###### clc，clear，close all   清屏,  清除内存,  关闭所有的Figure窗口



###### 空格的作用

```matlab
>> a3=[7 - 2 + 5]
a3 =
    10
>> a4=[7 -2 +5]
a4 =
     7    -2     5
```



###### 冒号的作用

`a=2:2:20`   %从2 ~ 20 步长为2



###### 特殊矩阵的生成函数

```
ones(4)         %  创建所有元素为1的矩阵
rand(2,3)       %  创建2*3的均匀分布随机数矩阵

>> randperm(7)     %  创建由1∶7构成的随机数列
ans =
     6     3     7     5     1     2     4
```

+ 随机种子的设置

不同版本设置方式有所不同，根据提示来

```matlab
% rand('state', 0);  % 14b
% rng(0)   % 17a
```



###### 数组矩阵寻访

+ 全下标标识

    ```matlab
    >> a=[1 2 3; 4 5 6]       %  创建测试矩阵
    >> A=a(2,2)             %  全下标寻访
    A =
         5
    ```

+ 单下标标识

  ```matlab
  >> b=a(4)               %  单下标寻访
  b =
       5
  ```

+ 逻辑1标识

  ```
  >> B=a>5                    %  返回逻辑下标
  B =
    2×3 logical array
     0   0   0
     0   0   1
     
  >> c=a(B)               %  逻辑下标寻访
  c =
       6
       
  >> C=a(a>5)  
  C =
       6
  ```

+ 冒号

  ```matlab
  >> d=a(1,:)                 %  通过使用冒号可以寻访全行元素
  d =
       1     2     3
  
  >> e=a(:,2)                 %  通过使用冒号可以寻访全列元素
  d =
       2
       5
       
  >> f=a(:)                %  单下标寻访
  f =
       1
       4
       2
       5
       3
       6
  ```

+ 向量/数组

  ```matlab
  >> g=a(:,[1 3])            %  寻访地址可以是向量，以同时寻访多个元素
  g =
       1     3
       4     6
  ```

  

###### 数组矩阵操作

+ 矩阵的合并

    ```matlab
    >> A = ones(2, 5) * 6;         %  元素全部为6的2´5矩阵
    >> B = rand(3, 5);                 %  3´5 的随机数矩阵
    >> C = [A; B]
    C =
        6.0000    6.0000    6.0000    6.0000    6.0000
        6.0000    6.0000    6.0000    6.0000    6.0000
        0.5469    0.1576    0.4854    0.4218    0.9595
        0.9575    0.9706    0.8003    0.9157    0.6557
        0.9649    0.9572    0.1419    0.7922    0.0357
    
    ```

+ 矩阵的赋值

  ```matlab
  a=magic(4)
  a(3,4)=0    % 对单个元素进行赋值
  a(:,1)=1    % 对第一列进行赋值
  a(14)=16    % 采用全下标对第14个元素进行赋值
  ```

+ 矩阵的转置

  ```matlab
  >> A = [1 2; 3 4]
  A =
       1     2
       3     4
  >> A'
  ans =
       1     3
       2     4
  
  ```

  

+ 数组运算和矩阵运算的区别

  ```matlab
  a=[1 2 4 9;16 25 36 49]
  b=sqrt(a)                    %  应用函数对矩阵中的每一个元素分别开方
  ```

+ 矩阵的形状信息

  ```matlab
  >> A = rand(5,3) * 10       %  生成5´5的随机矩阵
  >> size(A)
  ans = 
       5     3
  
  >> a=length(A)   %对于维度较多的数组，长度为max(size(A))
  
  b=sum(A(:))/numel(A) %  使用Sum和numel函数计算矩阵A的平均值 numel:Number of array elements
  mean(A) 	 %第一维度（列）求平均数
  mean(A,2) 	 %第二维度（行）求平均数
  mean(A(:))	 %整个数组平均数
  mean(mean(A))
  ```

+ 数组运算和矩阵运算的比较

  ```matlab
  A=[1 2;3 4]       % 测试矩阵A
  B=[4 3;2 1]       % 测试矩阵B
  r1=100+A           % 矩阵A加上一个常数
  r2_1=A*B            % 两个矩阵相乘，矩阵乘法
  r2_2=A.*B            % 两个矩阵相乘，数组乘法
  r3_1=A\B             % 矩阵左除
  r3_2=A.\B            % 数组除法
  r4_1=B/A             % 矩阵右除
  r4_2=B./A           % 数组除法
  r5_1=A.^2           % 数组幂
  r5_2=A^2            % 矩阵幂
  r6_1=2.^A           % 数组幂
  
  r6_1 =
       2     4
       8    16
  ```

+ 矩阵元素的扩展与删除

  ```matlab
  A=magic(4)
  A(6,7)=17          %直接赋值就可以扩充
  A(:,8)=ones(6,1)
  A(:,1)=[]          %  删除矩阵A的第1列
  A(2,:)=[]          %  删除矩阵A的第2行
  ```

+ 矩阵的重构

  ```matlab
  >> a=reshape(1:9,3,3)               %  创建测试矩阵
  a = 
       1     4     7
       2     5     8
       3     6     9
  
  >> a= [1,7;2,8;3,9;4,10;5,11;6,12];  %  创建测试矩阵
  >> a  = reshape(a,4,3)           %  使用reshape改变a的形状
  a = 
       1     5     9
       2     6    10
       3     7    11
       4     8    12
  
  %  注意前后两个a每一个单下标对应的元素是一致的
  >> b=rot90(a,3)                       %  将矩阵a逆时针旋转3×90°
  b = 
       4     3     2     1
       8     7     6     5
      12    11    10     9
  
  c=fliplr(a)                      %  将矩阵a左右翻转
  d=flipud(a)                    %  将矩阵a上下翻转
  ```

+ 稀疏矩阵，创建，与普通矩阵转换，视图

  ```matlab
  >> A=[0 0 0 1;0 1 0 0;1 2 0 0;0 0 3 0];
  >> s=sparse(A)
  s =    
     (3,1)        1
     (2,2)        1
     (3,2)        2
     (4,3)        3
     (1,4)        1
  
  B=full(s)  %还原成普通矩阵
  ```

  使用函数sparse，可以用一组非零元素直接创建一个稀疏矩阵。该函数调用格式为：
  `S=sparse(i,j,s,m,n)`
  其中i和j都为矢量，分别是指矩阵中非零元素的行号与列号，s是一个全部为非零元素矢量，元素在矩阵中排列的位置为(i,j)。m为输出的稀疏矩阵的行数，n为输出的稀疏矩阵的列数。

  ```matlab
  S=sparse([1 3 2 1 4],[3 1 4 1 4],[1 2 3 4 5],4,4) %行号矢量，列号矢量， 值矢量
  full(S)
  B = bucky;
  spy(B) %画图
  ```

  

###### 多维数组

创建多维数组最常用的方法有以下4种。
  1. 直接通过“全下标”元素赋值的方式创建多维数组。
  2. 由若干同样尺寸的二维数组组合成多维数组。
  3.  由函数ones、zeros、rand、randn等直接创建特殊多维数组。
  4.  借助cat、repmat、reshape等函数构建多维数组。

```matlab
>> clear     %%%%记得要清内存啊啊
>> A(3,3,3)=1                %  创建3*3*3数组，未赋值元素默认设置为0

A = 
(:,:,1) =
     0     0     0
     0     0     0
     0     0     0
     
(:,:,2) =
     0     0     0
     0     0     0
     0     0     0
     
(:,:,3) =
     0     0     0
     0     0     0
     0     0     1
```

```matlab
B(3,4,:)=1:4              %  创建3*4*4数组

B = 
(:,:,1) =
     0     0     0     0
     0     0     0     0
     0     0     0     1
     
(:,:,2) =
     0     0     0     0
     0     0     0     0
     0     0     0     2
     
(:,:,3) =
     0     0     0     0
     0     0     0     0
     0     0     0     3
     
(:,:,4) =
     0     0     0     0
     0     0     0     0
     0     0     0     4


```

```
C(:,:,1)=magic(4);                %  创建数组A第1页的数据
C(:,:,2)=ones(4);                 %  创建数组A第2页的数据
C(:,:,3)=zeros(4)                 %  创建数组A第3页的数据
C = 
(:,:,1) =
    16     2     3    13
     5    11    10     8
     9     7     6    12
     4    14    15     1

(:,:,2) =
     1     1     1     1
     1     1     1     1
     1     1     1     1
     1     1     1     1

(:,:,3) =
     0     0     0     0
     0     0     0     0
     0     0     0     0
     0     0     0     0
```

```matlab
D=rand(3,4,3)   % 由函数rand直接创建特殊多维数组
>> E3=cat(3,ones(2,3),ones(2,3)*2,ones(2,3)*3)   %借助cat函数构建多维数组
E3 = 
(:,:,1) =

     1     1     1
     1     1     1


(:,:,2) =

     2     2     2
     2     2     2


(:,:,3) =

     3     3     3
     3     3     3


>> E2=cat(2,ones(2,3),ones(2,3)*2,ones(2,3)*3)   %借助cat函数构建多维数组
E2 = 
     1     1     1     2     2     2     3     3     3
     1     1     1     2     2     2     3     3     3

>> E1=cat(1,ones(2,3),ones(2,3)*2,ones(2,3)*3)   %借助cat函数构建多维数组
E1 = 
     1     1     1
     1     1     1
     2     2     2
     2     2     2
     3     3     3
     3     3     3

```

```matlab
e=[1,2;3,4;5,6]
F=repmat(e,[1,2,3])
G=reshape(1:60,5,4,3)
H=reshape(G,4,5,3)
I=flip(H,1)

J=shiftdim(H,1)    %  将各维向左移动1位，使2*3*3数组变成3*3*2数组
K=shiftdim(H,2)      %  将各维向左移动2位，使2*3*3数组变成3*2*3数组
```



运算D=shiftdim(A,1)实现以下操作：D(j,k,i)=A(i,j,k)，i, j, k分别是指各维的下标。对于三维数组，D=shiftdim(A,3)的操作就等同于简单的D=A。



###### 数据类型

+ 数值型

  ......

+ 逻辑型

```matlab
>> a=[1 2 3; 4 5 6]       %  创建测试矩阵
>> B=a>5                    %  返回逻辑下标
B =    0   0   0
   0   0   1

>> c=true(size(a))
c =   
	1   1   1
   	1   1   1

>> false([size(a),2])  %=>  false(2, 3, 2])
ans = 
(:,:,1) =

   0   0   0
   0   0   0

(:,:,2) =

   0   0   0
   0   0   0
```

练习

```
a=[1 2 3;4 5 6];
b=[1 0 0;0 -2 1];
A=a&b                %  逻辑“与”
B=a|b                %  逻辑“或”
C=~b                 %  逻辑“非”

a=[1 1 0; 1 0 0;1 0 1]
A=all(a)                %  每列元素均为非零时返回真
B=any(a)                %  每列元素存在非零时返回真

>> a=[0  -1  2];
>> b=[-3  1  2];
>> a<b                                %  对应元素比较大小
ans =    0   1   0

a>b                                %  对应元素比较大小
a<=b                                %  对应元素比较大小
a>=b                                %  对应元素比较大小
a==b                                %  对应元素比较相等
>> a~=b                                %  对应元素比较不相等
ans =    1   1   0
```





###### 运算符优先级

![1555136189103](/images/1555136189103.png)

###### 字符和字符串

```matlab
>> a = 'matlab'
a =
matlab

>> size(a)
ans =
     1     6
>> A='中文字符串输入演示';
>> A(3:5)
ans =
	字符串
	
>> S=['This string array '
'has multiple rows.']
S = 
This string array 
has multiple rows.

>> a=char('这','字符','串数组','','由5 行组成')
a = 这     
字符    
串数组   
      
由5 行组成
% 以字符最多的一行为准，而将其他行中的字符以空格补齐


```



###### 结构数组

![1555136234814](/images/1555136234814.png)

```matlab
>> employee.name='henry';
>> employee.sex='male';
>> employee.age=25;
>> employee.number=12345;
>> employee
employee =       
	  name: 'henry'
       sex: 'male'
       age: 25
    number: 12345


```

结构还可以通过赋值的方式扩展为结构数组

```matlab
>> employee(2).name='lee';
>> employee(2).sex='female';
>> employee(2).age=23;
>> employee(2).number=98765;
>> employee(2)
employee =       
	  name: 'henry'
       sex: 'male'
       age: 25
    number: 12345
    
>> employee                      %  查看employee结构数组
employee =     
	name
    sex
    age
    number


```

子域

```matlab
>> green_house.name='一号房'; 
>> green_house.volume='2000 立方米'; 
>> green_house.parameter.temperature=...
[31.2 30.4 31.6 28.7;29.7 31.1 30.9 29.6];       %子域温度
>> green_house.parameter.humidity=...
[62.1 59.5 57.7 61.5;62.0 61.9 59.2 57.5];        %子域湿度
>> green_house.parameter      %  显示域的内容
ans =     temperature: [2×4 double]
       humidity: [2×4 double]

>> green_house.parameter.temperature              %  显示子域中的内容
ans = 
   31.2000   30.4000   31.6000   28.7000
   29.7000   31.1000   30.9000   29.6000

```



###### 元胞数组

![1555136644305](/images/1555136644305.png)

创建元胞数组

```matlab
>> A = {[1 4 3; 0 5 8; 7 2 9], 'Anne Smith'; 3+7i, -pi:pi/4:pi}
A =           
	[3×3 double]    'Anne Smith'
    [3.0000 + 7.0000i]    [1×9 double]

>> header = {'Name', 'Age', 'Pulse/Temp/BP'};    			%  元胞数组的创建
>> records(1,:) = {'Kelly', 49, {58, 98.3, [103, 72]}};	%  嵌套元胞数组的创建
```

依次创建元胞数组

```matlab
clear
A(1,1) = {[1 4 3; 0 5 8; 7 2 9]};
A(1,2) = {'Anne Smith'};
A(2,1) = {3+7i};
A(2,2) = {-pi:pi/4:pi};
A(3,3) = {5}
A =           
		  [3×3 double]    'Anne Smith'     []
    [3.0000 + 7.0000i]    [1×9 double]     []
                    []              []    [5]

>> str=A{1,2}       %  返回字符型数组str，a{1,2}表示对应元胞的内容
str = 
	'Anne Smith'
>> class(str)       %  查看变量str的数据类型，结果确为字符型
ans = 'char'

>> str2=A(1,2)      %  a(1,2)表示元胞数组中的一个元胞
str2 =
  cell
    'Anne Smith'
>> class(str2)      %  查看变量str2的数据类型，结果为元胞数组
ans = 'cell'

>> [nrows, ncols] = cellfun(@size, A)        %  将size函数应用于每一个元胞元素
ncols = 
     3    10     0
     1     9     0
     0     0     1

ncols = 
     3    10     0
     1     9     0
     0     0     1


cellplot(A)    %  以图片表示元胞数组的基本结构
```

![1555137533427](/images/1555137533427.png)



###### 日期和时间

```matlab
t = datetime(2017,8,28,6:7,0,0)
t.Day
t.Day = 27:28
t.Format
t.Format = 'MMMdd日, yyyy年'
t2 = datetime(2017,7,29,6,30,45)
d = t - t2
d.Format = 'h'
d.Format = 'd'

load('datahis.mat')   % 载入时间、风速、功率等测试数据
plot(datahis0.t_his,datahis0.v)
plot(datetime(datevec(datahis0.t_his)),datahis0.v)
```

###### Tables   表格数组

```
T = readtable('patients.dat')   % 读取表格数据
T(1:5,1:5)
```

可通过以下方式直接创建

```matlab
LastName = {'Smith';'Johnson';'Williams';'Jones';'Brown'};
Age = [38;43;38;40;49];
Height = [71;69;64;67;64];
Weight = [176;163;131;133;119];
BloodPressure = [124 93; 109 77; 125 83; 117 75; 122 80];
T = table(LastName,Age,Height,Weight,BloodPressure)
```


表可以像普通数值矩阵那样通过小括号加下标来进行寻访。除了数值和逻辑型下标之外，用户还可以使用变量名和行名来作为下标。例如本例中可以使用LastName作为行名，然后将这一列数据删除。

```matlab
clear
T = readtable('patients.dat')   % 读取表格数据
T.Properties.RowNames = T.LastName;
T.LastName = [];
size(T)                      %  查看当前表T的尺寸
T(1:5,6:9)
```

基于已有变量（身高和体重）用户可以创建新的变量BMI，也就是体重指数。然后还可以添加变量的单位和描述等属性。
T.BMI = (T.Weight*0.453592)./(T.Height*0.0254).^2;
T.Properties.VariableUnits{'BMI'} = 'kg/m^2';
T.Properties.VariableDescriptions{'BMI'} = 'Body Mass Index';
size(T)           %  查看当前表的尺寸

###### 导数

```
A=randperm(9)                     %  生成随机数列
B = diff(A)                        %  求数列的差分
C = pascal(6)
D = diff(C)              % 对矩阵C列方向各元素进行差分计算
```



###### 梯度

+ FX = gradient(F)：返回F的一维数值梯度，F是一个向量。

+ [FX,FY] = gradient(F)：返回二维数值梯度的x和y部分，F是一个矩阵。

+ [FX,FY,FZ,...] = gradient(F)：求高维矩阵F的数值梯度。

+ [...] = gradient(F,h)：h是一个标量，用于指定各个方向上点之间的间距。

+ [...] = gradient(F,h1,h2,...)：指定各个方向上的间距。

  ```
  v = -2:0.2:2;
  [x,y] = meshgrid(v);
  z = x .* exp(-x.^2 - y.^2);        %  创建测试数据
  [px,py] = gradient(z,.2,.2);       %  求梯度
  contour(v,v,z), hold on, quiver(v,v,px,py), hold off    %  绘制等高线和梯度方向
  ```

  