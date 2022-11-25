# MATLAB学习笔记

### Get Started

#### help me!

帮助文档: `help something`, 注意不是`help(something)`

也可以在键入函数的左括号后暂停, 比如`funcName(`, 此时编辑器会弹出窗口提示参数名称 

`help`是简明文档. 如果要打开详细文档的话使用`doc something`

或者直接上[官网](https://ww2.mathworks.cn/help/)查文档

MATLAB中有很多操作都和命令行的操作类似



#### 基本运算

1. 矩阵运算
  - 相同大小的矩阵可以在对应位置进行加减乘除运算. 注意乘除需使用`.*`&`./`(点乘&点除), 否则变成矩阵乘法. 还有点乘方`.^`
  - 若某个操作可以对单个数进行运算, 那么将这个操作用于矩阵, 则该矩阵中所有元素都会分别经历一次该运算
2. 逻辑运算
  - 非运算是`~` (不是`!`). 逻辑`logical`类型: `True`为`1`, `False`为`0`
3. MATLAB中有内置了许多数据和函数, 比如`pi`, `sin`, `abs`, `sqrt`, 等, 可以直接拿来用; 不像Python中必须`import math`. 使用的时候要注意避开这些关键字
4. MATLAB中没有`//`整除这种用法, 整除用`idivide()`*, 但是个人感觉这个函数并不好用, 不如直接除再取整*
    1. `unit8()`, 转化为`uint8`整型数据

5. MATLAB中**没有**`+=`这种用法, **没有**`++`这种用法



#### 基本语法

1. MATLAB变量名必须以严格的字母开头, 数字或下划线都不行
2. 注释用`%`, 快捷键`Ctrl + R`; 取消注释`Ctrl + T`. 取余用`mod(a, b)`
3. `clear`清除所有变量, `clear varname`清除某个变量; `clc`清空命令行窗口
4. `whos varname`查看某个变量信息, `whos`查看所有变量信息
5. MATLAB内置虚数. 虚数单位用`i`, `j`均可. 虚数是`Complex`类
6. MATLAB中`if`, `for`等语句没有强制缩进 (但最好用缩进对齐, 美观一些), 且必须用`end`来结束. `if`, `else`等语句后面都没有冒号



#### for循环

```matlab
for i=1:10
	% do something
end
```

注意, 对于一个二维数组, for循环是枚举每一列(而不是枚举每一行)



#### 基本统计函数

- `sum()`
- `mean()`

##### 参数

- `'omitnan'`



#### 路径

和Linux里一样啦

1. 获取当前工作路径`pwd`

> 打开MATLAB后第一件事看当前路径

2. 设定工作路径`cd(path)`或者`cd path`
  - 注意, 工作路径设定之后, 打开文件只需要相对路径
3. `.` 当前文件夹
4. `..` 上级文件夹





### 数据结构

#### array矩阵

0. MATLAB中所有变量都是一个$n(n\ge1)$维矩阵

1.  用方括号`[]`创建矩阵. 用`,`或空格分行, 用`;`分列

2.  `'`: 转置符

3.  `start:step:end`, 创建一个向量(二维矩阵). 类似Python的列表推导式, 不过注意, MATLAB中`end`为实心 (而Python中为空心)

4.  `linspace`, 格式`linspace(first, last, number_of_elements)`用来创建指定首尾和数量的数组

5.  `rand(x)`, 用来创建$x\times x$的随机矩阵. `rand(a,b)`用来创建$a\times b$的随机矩阵
- `zeros()`, 创建全0矩阵, 传入变量和`rand()`一致
- `ones()`, 创建全1矩阵
- ~~如果要创建一个全`NaN`矩阵, 可以先用上述方法创建一个矩阵, 再将其全部置`NaN`~~
- 错! 直接用`nan()`就行
6.  `repmat()`, 用来叠置矩阵. 详见文档



`squeeze()`, 将一个多维, 但是只有一个维度上的大小大于一的矩阵/数组变为一维向量



`sort()`, 排序, 默认升序



`flipud()`



---

#### struct结构体





---

#### 元胞数组cell

创建空的元胞数组: `cell()`

- `()`切出来的是**子元胞数组**, 仍是一个元胞数组
- `{}`切出来的是元胞数组中的**内容**. 如果花括号下标的范围不止一个元胞, 那么这些内容会被逐次返回

char和string类型都能作为元胞数组的内容, 但是赋值的时候要注意数据类型的匹配. 即, 如果赋值号左边是元胞数组的**内容**, 那么可以直接赋值, 注意, 元胞数组也可以作为内容被赋值给元胞数组的一个元胞; 如果等号右边是一个元胞数组, 那么它应为和左边相同大小的元胞数组(?)

*上面这段我在说什么???*

***总之可以层层套娃***

```matlab
>> testCell={12,23,34;45,56,67;78,89,90}

testCell =

  3×3 cell array

    {[12]}    {[23]}    {[34]}
    {[45]}    {[56]}    {[67]}
    {[78]}    {[89]}    {[90]}

>> testCell(3)

ans =

  1×1 cell array

    {[78]}

>> testCell(2,2)

ans =

  1×1 cell array

    {[56]}

>> testCell{3}

ans =

    78
    
>> testCell{2,2}

ans =

    56
    
>> testCell(2,1)='char'
Conversion to cell from char is not possible.

>> testCell(2,1)={'char'}

testCell =

  3×3 cell array

    {[  12]}    {[23]}    {[34]}
    {'char'}    {[56]}    {[67]}
    {[  78]}    {[89]}    {[90]}

>> testCell{2,2}='char2'

testCell =

  3×3 cell array

    {[  12]}    {[   23]}    {[34]}
    {'char'}    {'char2'}    {[67]}
    {[  78]}    {[   89]}    {[90]}
    
>> testCell{2,3}={'char3'}

testCell =

  3×3 cell array

    {[  12]}    {[   23]}    {[    34]}
    {'char'}    {'char2'}    {1×1 cell}
    {[  78]}    {[   89]}    {[    90]}
    
% OK, but content of cell would be another cell
```



---

#### 字符数组char和字符串string

注意MATLAB中单引号和双引号括起来的字符串是不一样的, 单引号的字符串是`char`类型, 而双引号的是`string`类型

> 有点类似于C++中C风格的字符数组和STL string的关系

- 字符数组用单引号`''`括起来, 可以用`[char1 char2]`的方式进行拼接
- 字符串用双引号`""`括起来, 不可以用方括号拼接

如果某个字符串已经包含了引号, 那么输入的时候要把这个引号变成两个引号`''`或`""`





#### [table表](https://ww2.mathworks.cn/help/matlab/tables.html?lang=en)

最重要的feature: table可以根据列名(Variable Name)访问数据

`tableName.varName`

注意, 单元格内容如果是字符串, 存储形式不是单独的字符串, 而是一个$1\times1$的cell(?这里没写清是`''`字符串还是`""`字符串



注意, table有圆括号`()`和花括号`{}`两种访问方式

[访问表中的数据Access Data in Tables](https://ww2.mathworks.cn/help/matlab/matlab_prog/access-data-in-a-table.html?lang=en)

- `()`切出来的是子table, 无论是$1\times1, 1\times n,n\times1,n\times n$, 结果都是一个table
- `{}`的情况比较复杂. 首先, 数字和字符串不能被切进同一个结果, 否则会报错
    - 全是字符串的情况下, 无论是$1\times1, 1\times n,n\times1,n\times n$, 结果都是一个cell array
    - 全是数字的情况下, 无论是$1\times1, 1\times n,n\times1,n\times n$, 结果都是一个数组
        - 如果其中含有复数, 则其他实数也会被转化为虚部为0的复数
    - 其他类型的数据没有试过. 不过按我的理解, `{}`切出来的数据必须都是同一数据类型, 或者是经过转换能成为同一数据类型的. 



`readtable`, 读取xls, xlsx, csv等表格

- `'ReadVariableName',true`: 读取首行为列名
- `'ReadRowName',true`: 读取首列为行名
- `'VariableNamingRule','preserve'`: 遇到[行列名为数字, 含有句点`.`]等情况(原始列名不是MATLAB合法变量名)的时候不对行列名做改动

有的时候不需要前两个参数就会自动读取行列名称, 有时候不行. 还不知道什么时候可以什么时候不行, 每次用的时候先试一下就好~



转置一个表

`rows2vars(table)`

- `VariableNameSource'`, 转置后的表的列名用转置前的哪一列
- `'VariableNamingRule'`, 一般为`'preserve'`, 否则列名会按缺省方式命名, 而不是用行名或者某一列的列名



表的列名

`table.Properties.VariableNames`

`table.Properties.VariableDescription`

- 如果读取table的时候因为没有设置`'VariableNamingRule','preserve'`而出现了警告, 则原本的列名会被存储到这里



表的行名

`table.Properties.RowNames`



设置行列名的时候直接对`table.Properties.VariableNames`赋值即可



### 数据输入输出

#### 读入数据


1.  `load filename`, 将`filename.mat`的变量导入

2.  MATLAB中可以直接导入`.txt`文件, 可选择导入模式(?). 用`textread()`也可以

3.  读取`.xlsx`工作表: `xlsread()`或`readtable()` (可以, 这玩意挺好用的)(以及要写一下`table`数据类型)

4.  `imread()`, 从tif文件中读取数据



#### 导出数据

1. `save filename dataname`, 将变量`dataname`存入`filename.mat`的文件中
2. `xlswrite()`, 导出到Excel表
3. 写到geotiff
4. 还可以导出到和R交互的格式



---

### 下标&逻辑表达式

1. `a = b(x,y)`可以将`b`矩阵的第`x`行`y`列的数据取出来给`a`. **注意取下标时用圆括号()而不是方括号[]!!!** 由于MATLAB中的变量都是一个$n(n\ge1)$维的数组, 因此对于一个$n$维数组, 就需要$n$个下标, 每个下标表示在这个维度上要取出哪些数据. 注意, `x`和`y`都是向量, 它们常用这些方法来创建: 
   1. `start:step:end`
      1. `end`表示最后一行/列. `end-1`表示倒数第二行/列, 以此类推
      2. 如果就填一个`:`, 那表示取出这个维度上的所有数据
   2. 逻辑表达式. 
      1. 如果对矩阵进行逻辑运算, 那么只能矩阵和数进行, 或者两个相同大小的矩阵之间进行. 返回一个逻辑类型的矩阵 (这个矩阵的大小和参与运算的矩阵一致)
      2. 数组可以用逻辑值作为下标. 如`v1(v1>4)`返回`v1`中所有大于4的元素的子向量. 其中`v1>4`返回的是一个下标值的向量. 也可以这么写:`v2(v1>4)`,即把`v1`中大于4的元素的位置记下来, 再在`v2`中找到这些元素
2. 注意MATLAB中下标从1开始而不是0
3. 注意, 如果只用一个下标取二维数组的值, 是从第一列开始, 而不是从第一行开始



`isnan()`

`isempty()`

`find()`, https://ww2.mathworks.cn/help/matlab/ref/find.html





---

### 绘图

注意, 这里指的是画出来展示的图, 不是导出数据. 

##### plot()

函数绘图. 逻辑和Python中的`matplotlib`很像. 例: `plot(x, y, ‘o-r’)`. `loglog()`函数用来画两个坐标轴都是对数的图象, 其他用法和`plot()`一致. 注意MATLAB中参数有相对严格的位置要求,否则很容易报错. 另外,传递参数要用逗号分隔,如: `loglog(lambdaHa,sHa,'rs','MarkerSize',8)`, 而不是像Python中用等号



#### 设置标题

`title('title')`

- `'FontName'`, 设置字体



#### 设置坐标轴标签

`xlabel('labelContent')`. y轴同理



#### 设置坐标轴刻度值

这个意思是在哪里出现刻度

`xticks([array])`

`xticks('auto')`, 设置回默认值

`xticks([])`, 不要刻度值



#### 设置坐标轴刻度标签

`xticklabels(cell)`

![img](https://ww2.mathworks.cn/help/matlab/ref/xticks_ticksvslabels.png)

###### 刻度和标签

xticks设置刻度. 这个是设置刻度在哪的

https://ww2.mathworks.cn/help/matlab/ref/xticks.html

指定刻度和标签:

https://ww2.mathworks.cn/help/matlab/creating_plots/change-tick-marks-and-tick-labels-of-graph-1.html?searchHighlight=%E5%9D%90%E6%A0%87%E8%BD%B4%E5%88%BB%E5%BA%A6&s_tid=srchtitle_%25E5%259D%2590%25E6%25A0%2587%25E8%25BD%25B4%25E5%2588%25BB%25E5%25BA%25A6_1

坐标轴范围xlim

https://ww2.mathworks.cn/help/matlab/ref/xlim.html









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

`gca`: 当前坐标区, **g**et **c**urrent **a**xis

`gcf`: 当前图窗, **g**et **c**urrent **f**igure

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



#### 导出图片

`print('filename','-djpeg','-r600')`





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





### 捕捉异常

基本语法

```matlab
try
	% try block
catch ME
	% catch block
end
```

- 注意, MATLAB中不允许多个`catch`, 没有`finally`
- 错误信息会存在`ME`中, 它是一个`MException`类型的对象
- 可以在后续`catch block`中用`switch`处理不同类型的错误
- 如果要应对多种具体的错误, 请参见[MATLAB文档](https://ww2.mathworks.cn/help/matlab/ref/try.html)





### 并行计算/分布式计算

需要`Parallel Computing Toolbox™`

参见

https://ww2.mathworks.cn/solutions/parallel-computing.html

https://ww2.mathworks.cn/products/parallel-computing.html

#### parfor

- `parfor`仅限于前一次计算结果对后一次没有影响的情况, 即各个运算之间互相独立. 这一点看似容易, 但是在写代码的时候要格外留心
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







###### parallelcoords

https://ww2.mathworks.cn/help/stats/parallelcoords.html

用来画多维数据. 比如说同一观测条件下有若干平行观测值





时间和日期

https://ww2.mathworks.cn/help/matlab/date-and-time-operations.html

看这个









`full()`和`sparse()`, 稀疏矩阵和???的转换





## 10/22/2022新增

`isequal()`, 判断`array`类型的数据是否相等(大小相同&各个值均相同). 注意输入的范围包含`numeric`和`char`

可以有超过2个输入, 此时它们全部相等时结果为`true`

注意, 判断不同类型的`numeric`数据时, 比如`uint8`和`double`, 只要数学上的值相等, 结果就为`true`

还有一个类似的`isequaln()`, 它会把`NaN`值视作相等, 而`isequal()`把`NaN`值视作不等



`isobject()`, 判断是否为`object`. 注意`string`类型的也是`object`



字符串判断相等

两个`char`类型的字符串用`==`判断相等时, 它们的长度必须相同, 否则会报错. 得到的结果是它们长度大小的`logical`矩阵

`string`和`char`, `string`和`string`之间用`==`判断相等时不需要长度相同. 得到的结果是`1x1`的`logical`



`strcmp`无上述应用限制. 此外可以进行多重比较, 将字符串矩阵和一个字符串进行比较, 返回字符串矩阵大小的`logical`矩阵. 或者是两个大小相同的字符串矩阵进行比较, 返回它们对应位置的字符串是否相等的`logical`矩阵

字符串矩阵`string array`(用`[]`括起)或`cell array`(用`{}`括起)

创建`string array`时只要内容不全是`character vector`就行(至少有一个`string scarlar`)



`fieldnames()`, 给出`object`的`properties`. `properties()`函数似乎有相同功能

判断是否是`object`: `isobject`

注意, `string`类型同样是`object`, 但是对其用`fieldnames()`会报错, 而用`properties()`不会



`numel()`返回元素总数量. 等价于`prod(size(X))`

`size()`分别返回各个维度的大小

`length()`最大维度的大小. 等价于`max(size(X))`

`prod()`连乘

`sum()`累加

`ndims()`返回维度数量



`mod()`和`rem()`取余数. 被除数和除数符号相同时无差异, 符号不同时, `mod()`结果的符号和**除数**相同, 而`rem()`结果的符号和**被除数**相同



`idivide()`非常不好用, 它要求被除数必须是整型, 值为整数的浮点型也不行



`class()`给出类型名

`isa()`判断是否为某一类型



[Functions that Read and Write Geospatial Data](https://ww2.mathworks.cn/help/map/functions-that-read-and-write-geospatial-data.html)



---

## 11/1/2022新增

`mat2gray`, 将数据线性变换到`[0,1]`区间上

`xcorr`

`corrcoef`



`interp2()`, 用来插值(在二维平面上), 尤其是实现ArcGIS中提取至点的功能



如何在一张图上使用两套colormap? 做法: 创建两个坐标区`axes`

`ax1=axes;`

然后后续作图时第一个参数要指定坐标区

再将两个坐标区连接起来

`linkaxes([ax1,ax2])`

再将第二个坐标区的坐标轴隐藏起来

`ax2.Visible='off';ax2.XTick=[];ax2.YTick=[];`

[参考页面](https://ww2.mathworks.cn/matlabcentral/answers/194554-how-can-i-use-and-display-two-different-colormaps-on-the-same-figure)



如何为colorbar添加标签

本质上colorbar是一个很扁的绘图区, 因此只需要在创建colorbar的时候指定其句柄

`cb=colorbar;`

再为其添加纵坐标标签

`ylabel(cb,...)`



`figure`, 创建一个新的图窗. 同理, 可以在创建时指定其句柄

`f=figure;`

[Figure Properties](https://ww2.mathworks.cn/help/matlab/ref/matlab.ui.figureappd-properties.html?lang=en)



`subplot(...)`, 也是创建一个axes, 同理, 也可在创建时指定其句柄



如何在坐标轴标签/图像标题文字中换行? 用`cell array`或`string array`

`title（{'1st line','2nd line'}）`



计算相关

`xcorr()`, [互相关](https://ww2.mathworks.cn/help/matlab/ref/xcorr.html)

`corrcoef()`, [相关系数](https://ww2.mathworks.cn/help/matlab/ref/corrcoef.html), 注意除了算R值还能算P值及其上下界(95%置信区间). P值越小相关性越显著

(但是我还没搞清两者计算出来的结果有啥关系, 我只知道是做了一个线性变换后的结果, 好像和期望值有关)



自定义colormap

用`linspace`

或者可以用`colormapeditor`

必须是三列, 高度随意

`rgbplot`, 绘制指定colormap的RGB颜色强度(用来查看colormap的属性)



`clim()`, 设置colormap范围



画图时如何不显示`NaN`数据? 把`NaN`位置设置为0透明度

`imagesc(data,'AlphaData',~isnan(data))`



判断是否在多边形内部: `inpolygon`. 此外还可以判断是否在多边形上



设置在图窗中显示的范围

`xlim()` and `ylim()`

这个的作用是, 你可以用一块较大的区域的数据画图, 再用上述语句zoom in到你想要的范围, 而无需将原始数据裁切到你需要的范围



关于decimal year

decimal year是以闰年的1月1日中午12点为零点



`decyear`

`datestr`

`datevec`

`leapyear`



关于时间和日期的一些处理函数

`datenum` *not recommended*

`datetime`





---

`pbaspect` 设置坐标轴长度比例



`addpath`, 让你能调用另一个文件夹(非当前文件夹和MATLABPATH)中的函数

被`addpath`的文件夹在“Current Folder”中会显示成不透明

被添加的路径在关掉MATLAB就被清理了, 不会永久地添加到MATLABPATH中

但是这个方法不能帮你读取数据

与之对应的是`rmpath`

`genpath`, 生成这个路径下的所有子路径



`path`, 打印出当前所有MATLABPATH(*MATLAB's current search path*)

被`addpath`新加入的path会被列在最前面



`switch`

```matlab
switch variable
	case value1
		% do something 
	case value2
		% do something else
	otherwise
		% a backup
end
```

注意, 与C不同, MATLAB中执行了一个case, 便不会向下继续执行其他case, 无需break, 而且也无法在switch语句中使用break

[Document](https://ww2.mathworks.cn/help/matlab/ref/switch.html)





**[`geographicGrid`](https://www.mathworks.com/help/releases/R2022b/map/ref/map.rasterref.geographiccellsreference.geographicgrid.html)**

**[`ndgrid`](https://www.mathworks.com/help/releases/R2022b/matlab/ref/ndgrid.html)** 



`griddata(X,Y,Z,Xq,Yq)`: 用来空间重采样

前三个是原数据的位置和值, 后两个是待重采样的位置. 得到的结果为`Zq`



`circshift`

循环位移



`normalize`



`detrend`一键detrend



`plotprofile`





---

## 11/23/2022新增

`quiver`, 用来画箭头

注意`scatter`画散点图的时候可以指定散点颜色, 大小和形式. 颜色的映射用`colormap`指定

`clearvar -except keep...`清理除了`keep`之外的变量

`diff`差分. 注意他可以计算高阶差分

`tiledlayout`

`sgtitle`给许多subplot添加总标题

`maxk`计算前k个最大值(和它的下标)

`xline`, 在指定`x`值位置画竖线. `yline`同理

`findpeaks`, 需要`Signal Processing Toolbox`, 用来找local maxima的(还不清楚具体怎么用)



`islocalmax`, 判断是不是局部最大值. `islocalmin`同理

`cumsum`, 累加



MATLAB自带`mad`函数, 但是要指定是mean还是median absolute deviation. 画Error bar的时候或许能用得上



`nanmendian`, 相当于`median(~,'omitnan')`



`azimuth`,算两点之间的方位角. 不过注意这个针对的是在真实的球体上, 所以有时候结果不一定是正西南西北/东南东北



MATLAB里面是可以有连续函数的. 函数求解的一些方法: 

- https://ww2.mathworks.cn/help/matlab/ref/fzero.html
- https://ww2.mathworks.cn/help/matlab/ref/fminsearch.html

查找单变量函数在指定区间内的最小值: https://ww2.mathworks.cn/help/matlab/ref/fminbnd.html





---

