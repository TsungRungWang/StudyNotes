# MATLAB学习笔记

### Get Started

#### help me!

帮助文档: `help something`, 注意不是`help(something)`!!! 或者直接上[官网](https://ww2.mathworks.cn/help/)查文档



#### 基本运算

1. 矩阵的运算
  - 相同大小的矩阵可以在对应位置进行加减乘除运算. 注意乘除需使用`.*`&`./`(点乘&点除), 否则变成矩阵乘法
  - 若某个操作可以对单个数进行运算, 那么将这个操作用于矩阵, 则该矩阵中所有元素都会分别经历一次该运算
2. 逻辑运算
  - 非运算是`~` (不是`!`). 逻辑`logical`类型: `True`为`1`, `False`为`0`
3. MATLAB中有内置了许多数据和函数, 比如`pi`, `sin`, `abs`, `sqrt`, 等, 可以直接拿来用; 不像Python中必须`import math`. 使用的时候要注意避开这些关键字
4. MATLAB中没有`//`整除这种用法, 整除用`idivide()`*, 但是个人感觉这个函数并不好用, 不如直接除再取整*
5. MATLAB中没有`+=`这种用法, 没有`++`这种用法



#### 基本语法

1. MATLAB变量名必须以严格的字母开头, 数字或下划线都不行
2. 注释用`%`, 快捷键`Ctrl +R`; 取消注释`Ctrl + T`. 取余用`mod(a,b)`
3. `clear`清除所有变量, `clear varname`清除某个变量; `clc`清空命令行窗口
4. MATLAB内置虚数. 虚数单位用`i`, `j`均可. 虚数是`Complex`类
5. MATLAB中`if`, `for`等语句没有强制缩进 (但最好用缩进对齐, 美观一些), 且必须用`end`来结束. `if`, `else`等语句后面都没有冒号



#### 基本统计函数

- `sum()`
- `mean()`
- 

##### 参数

- `'omitnan'`



#### 路径

和Linux里一样啦

1. 获取当前工作路径`pwd`
	- 打开MATLAB后第一件事看当前路径
2. 设定工作路径`cd(path)`
  - 注意, 工作路径设定之后, 打开文件只需要相对路径
3. `.\` 当前文件夹
4. `..\` 上级文件夹



---

---

### 数据结构及其创建

#### 矩阵


1.  用方括号`[]`创建矩阵. 用`,`或空格分行, 用`;`分列

2.  `'`是转置符

3.  `start:step:end`, 创建一个向量. 类似Python的列表推导式, 不过注意, MATLAB中`end`为实心 (而Python中为空心)

4.  `linspace`, 格式`linspace(first, last, number_of_elements)`用来创建指定首尾和数量的数组

5.  `rand(x)`, 用来创建$x\times x$的随机矩阵. `rand(a,b)`用来创建$a\times b$的随机矩阵
- `zeros()`, 创建全0矩阵, 传入变量和`rand()`一致
- `ones()`, 创建全1矩阵
- 如果要创建一个全`NaN`矩阵, 可以先用上述方法创建一个矩阵, 再将其全部置`NaN`
6.  `repmat()`, 用来叠置矩阵. 详见文档







`squeeze()`, 将一个多维, 但是只有一个维度上的大小大于一的矩阵/数组变为一维向量



`sort()`, 排序, 默认升序



`flipud()`



---

#### 结构体struct





---

#### 元胞数组cell

创建空的元胞数组: `cell()`

`()`切出来的是子元胞数组, 仍是一个元胞数组. `{}`切出来的是元胞数组中的内容, 如果花括号下标的范围不止一个元胞, 那么这些内容会被逐次返回

char和string类型都能作为元胞数组的内容, 但是赋值的时候要注意数据类型的匹配. 即, 如果赋值号左边是元胞数组的**内容**, 那么可以直接赋值, 注意, 元胞数组也可以作为内容被赋值给元胞数组的一个元胞; 如果等号右边是一个元胞数组, 那么它应为和左边相同大小的元胞数组(?)

*总之可以层层套娃*

```matlab
testCell={12,23,34;45,56,67;78,89,90}
testCell(3)% {78}
testCell(2,2)% {56}
testCell{3}% 78
testCell{2,2}% 56
testCell(2,1)='char'% error, a string cannot be assigned to a cell
testCell(2,1)={'char'}% OK
testCell{2,2}='char2'% OK
testCell{2,3}={'char3'}% OK, but content of cell would be another cell

Result:
    {[  12]}    {[   23]}    {[     34]}
    {'char'}    {'char2'}    {{'char3'}}
    {[  78]}    {[   89]}    {[     90]}
```



---

#### 字符数组char和字符串string

注意MATLAB中单引号和双引号括起来的字符串是不一样的, 单引号的字符串是`char`类型, 而双引号的是`string`类型

> 类似于C++中C风格的字符数组和STL string的关系

字符数组用单引号`''`括起来, 可以用`[char1 char2]`的方式进行拼接

字符串用双引号`""`括起来, 不可以用方括号拼接

如果某个字符串已经包含了单引号, 那么输入的时候要把这个单引号变成两个单引号`''`



---

#### 数据类型转换

有这么些数据类型: 







---

---

### 数据输入输出

---

#### 读入数据


1.  `load filename`, 将`filename.mat`的变量导入

2.  MATLAB中可以直接导入`.txt`文件, 可选择导入模式(?). 用`textread()`也可以

3.  读取`.xlsx`工作表: `xlsread()`或`readtable()` (没用过)

4.  `imread()`, 从tif文件中读取数据

#### 导出数据

1. `save filename dataname`, 将变量`dataname`存入`filename.mat`的文件中
2. `xlswrite()`, 导出到Excel表
3. 写到geotiff
4. 还可以导出到和R交互的格式



---

### 下标&逻辑表达式

1. `a=b(x,y)`可以将`b`矩阵的第`x`行`y`列的数据取出来给`a`. **注意取下标时用圆括号()而不是方括号[]!!!** 由于MATLAB中的变量都是一个$n(n\ge1)$维的数组, 因此对于一个$n$维数组, 就需要$n$个下标, 每个下标表示在这个维度上要取出哪些数据. 注意, `x`和`y`都是向量, 它们常用这些方法来创建: 
   1. `start:step:end`
      1. `end`表示最后一行/列. `end-1`表示倒数第二行/列, 以此类推
      2. 如果就填一个`:`, 那表示取出这个维度上的所有数据
   2. 逻辑表达式. 
      1. 如果对矩阵进行逻辑运算, 那么只能矩阵和数进行, 或者两个相同大小的矩阵之间进行. 返回一个逻辑类型的矩阵 (这个矩阵的大小和参与运算的矩阵一致)
      2. 数组可以用逻辑值作为下标. 如`v1(v1>4)`返回`v1`中所有大于4的元素的子向量. 其中`v1>4`返回的是一个下标值的向量. 也可以这么写:`v2(v1>4)`,即把`v1`中大于4的元素的位置记下来, 再在`v2`中找到这些元素
2. 注意MATLAB中下标从1开始而不是0



`isnan()`

`isempty()`

`find()`, https://ww2.mathworks.cn/help/matlab/ref/find.html





---

### 绘图

注意, 这里指的是画出来展示的图, 不是导出数据. 

##### plot()

函数绘图. 逻辑和Python中的`matplotlib`很像. 例: `plot(x, y, ‘o-r’)`. `loglog()`函数用来画两个坐标轴都是对数的图象, 其他用法和`plot()`一致. 注意MATLAB中参数有相对严格的位置要求,否则很容易报错. 另外,传递参数要用逗号分隔,如: `loglog(lambdaHa,sHa,'rs','MarkerSize',8)`, 而不是像Python中用等号



#### Axes对象

##### axes()

创立一个坐标区(官方翻译: “笛卡尔坐标区”), 并指定它在图窗中的位置

返回一个`Axes`对象

有关`axes()`函数, 参见[MATLAB文档](https://ww2.mathworks.cn/help/matlab/ref/axes.html)

有关`Axes`属性, 参见[MATLAB文档](https://ww2.mathworks.cn/help/matlab/ref/matlab.graphics.axis.axes-properties.html)

通常为: `ax=axes('position',[left bottom width height]);`

- 注意, `position`不是必要的初始化参数, 可以先无参初始化, 再在之后设置, 不过一般习惯性地在创建的时候设置

##### position属性

`[left bottom width height]`, 数值介于01之间

其他属性, 参见文档



其他

[创建包含多个 *x* 轴和 *y* 轴的图](https://ww2.mathworks.cn/help/matlab/creating_plots/graph-with-multiple-x-axes-and-y-axes.html)

[创建包含双 *y* 轴的图](https://ww2.mathworks.cn/help/matlab/creating_plots/plotting-with-two-y-axes.html)

`gca`: 当前坐标区

`gcf`: 当前图窗

极坐标: `polaraxes()`和`PolarAxes`对象

`set`



#### pcolor()“伪彩图”

返回一个`Surface`对象

有关`Surface`属性, 见[MATLAB文档](https://ww2.mathworks.cn/help/matlab/ref/matlab.graphics.primitive.surface-properties.html)

通常为: `s=pcolor(AxeObj,lon,lat,data);`

- 后三个变量大小须一致
- `lon`, `lat`的创建见下方`meshgrid()`

如果不设置颜色的话, 好像会按照一个默认方式上色(?)

##### meshgrid()

创建格网信息. 通常为`[lon,lat]=meshgrid(left_lon+lonRes/2:lonRes:right_lon-lonRes/2,bottom_lat+latRes/2:latRes:top_lat-latRes/2)`

- `left_lon`: 最左侧经度. `right_lon`, `bottom_lat`, `top_lat`同理
- `lonRes`: 栅格经向空间分辨率(单位: Deg), `latRes`同理
- 起始和结束要偏移半个栅格, 是为了对准栅格的中心点
- `lon`和`lat`是和图像有相同大小的矩阵. `lon`存放着每个栅格(中心点)的经度, (因此, 同一列的数据相同), `lat`同理

##### 去除网格线

`set(s,'linestyle','none');`

- 注意, 如果不去除的话, 图像会因为格网太密而一团黑
- 这一步通常是必要的. 其他属性设置, 参看文档





#### colorbar

参见[MATLAB文档](https://ww2.mathworks.cn/help/matlab/ref/colormap.html)

有空再来补充……









`hold on`

`close all`, 关闭所有图窗





---

### 自定义函数

格式如下: 

```matlab
function [outputList]=functionName(parametersList)
	% do something
end
```

可以随时用`return`终止函数运行. 注意MATLAB里的`return`后面不能跟其他东西, 他只能强行结束函数并给出返回值 (官方说法: “将控制权交还给调用脚本或函数”), 因此你得先`output=<somethingYouWant>`, 再写`return`. 也可以不写`return`, 函数运行结束的时候会“自带”return

当然, 也可以在函数一开始就把`output`指定为缺省值, 避免出现无法返回的情况

如果只有一个输出, 不需要外面那个方括号

可以没有输出, 直接`function functionName(parametersList)`即可

---

### 捕捉异常

基本语法

```matlab
try
	% try block
catch
	% catch block
end
```

- 注意, MATLAB中不允许多个`catch`, 没有`finally`
- 如果不清楚错误的具体类型, `catch`后面可以不填, 这样只要出错就会执行`catch block`
- 如果要应对多种具体的错误, 请参见[MATLAB文档](https://ww2.mathworks.cn/help/matlab/ref/try.html)

---

### 并行计算/分布式计算

需要`Parallel Computing Toolbox™`

参见

https://ww2.mathworks.cn/solutions/parallel-computing.html

https://ww2.mathworks.cn/products/parallel-computing.html

#### parfor

- `parfor`仅限于前一次计算结果对后一次没有影响的情况. 这一点看似容易, 但是在写代码的时候要格外留心
- `parfor`不能嵌套
- 使用的时候, 可以先写一个自定义函数, 然后把这个函数放进`parfor`中运行



---

### 拟合&回归

#### polyfit()

具体参见[MATLAB文档](https://ww2.mathworks.cn/help/matlab/ref/polyfit.html)



---

### 其他(未归类/筐) 以下为施工区



`newline`, 用于字符串输出的时候分行



注意matlab中二维下标转化为一维的时候是先列后行的

`disp()`



###### 和C语言联合编程



