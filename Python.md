# Python学习笔记

Any problem? just `help()`!



## 参考教程&资料

[官方主页](https://www.python.org/)

[官方文档](https://docs.python.org/3/)

[廖雪峰的Python教程](https://www.liaoxuefeng.com/wiki/1016959663602400)

- 目前进度: 

菜鸟教程: [Python基础教程](https://www.runoob.com/python3/python3-tutorial.html)

- [NumPy教程](https://www.runoob.com/numpy/numpy-tutorial.html)
- [SciPy教程](https://www.runoob.com/scipy/scipy-tutorial.html)
- [Matplotlib教程](https://www.runoob.com/matplotlib/matplotlib-tutorial.html)

在线文档: [Problem Solving with Algorithms and Data Structures using Python](https://runestone.academy/runestone/books/published/pythonds/index.html)

数据结构于算法可视化: [VisualGo](https://visualgo.net/en)



## 启动Python

Linux: `python3`

> Linux系统也可以安装`python-is-python3`(`sudo apt install python-is-python3`), 之后就可以用`python`替代`python3`

Windows: `python`



> 启动后出现`>>>`提示符

退出python: `exit()`



## 运行

Linux: `python3 <filename>.py`

> 或者, 先在文件第一行加上: `#!/usr/bin/env python3`
>
> 再运行`chmod a+x <filename>.py`
>
> 就可以直接运行`.py`文件了

Windows: `python <filename>.py`



```python
if __name__ == '__main__':
    main()
```



## Python on Linux

`os.system('<command>')` 运行命令, 并将其打印在屏幕上

`os.popen('<command>')` 运行命令, 并将输出作为一个对象返回(因此你可以对其执行后续操作)



`sys`

`sys.argv`



shebang

`#!/usr/local/bin/python`

`#!/usr/bin/env python`







## pip&模块

`pip`和`pip3`是啥关系(?

`pip list` 查看已安装列表

`pip list --outdate` 查看可升级的包

`pip install --upgrade <pkgName>` 升级某个包

`pip install <pkgName>` 安装包

> [Python pip 安装与使用](https://www.runoob.com/w3cnote/python-pip-install-usage.html)

- 如果报错`ValueError: check_hostname requires server_hostname`, 关梯子试试



关于创建虚拟环境, 避免包版本冲突: [Virtual Environments and Packages](https://docs.python.org/3/tutorial/venv.html)



注意, Linux下不同用户的库是不一样的, 

- 自带的库位于`/usr/lib/python2.7/dist-packages` or `/usr/lib/python3/dist-packages` or `/usr/lib/python3.8`
- 通过`pip`安装的库位于`~/.local/lib/python3.x/site-packages`



## 语言规范





## 基本运算

| 运算符         |                         |
| -------------- | ----------------------- |
| `//`           | 整除(地板除)            |
| `%`            | 求余数                  |
| `/`            | "真"除法, 得到小数      |
| `divmod(a, b)` | 商和余数(返回一个tuple) |
| `m ** n`       | 乘方                    |
| `abs(n)`       | 绝对值                  |
|                |                         |



## 输入输出

`input()`  输入. 注意输入默认为字符串





## 数据类型

[Built-in Types](https://docs.python.org/3/library/stdtypes.html)

查询数据类型: `type()`或者`isinstance(var, type)`

- type()不会认为子类是一种父类类型
- isinstance()会认为子类是一种父类类型

Python3中, `bool`是`int`的子类; `True`和`False`可以和数字相加, `True==1`, `False==0`会返回`True`, 但可以通过`is`来判断类型(?)

判断父子类关系: `issubclass()`



### str

切片和索引

索引从0开始，左闭右开区间



检查子串

### range

这是一个数据类型吗(?)

### list

```python
[]
[1]
[1, 2, 3, 4]
```



`append(element)`, 向末尾增加元素

`insert(position, elememt)`, 把元素插到指定位置上

`pop(position)`, 删除指定位置的元素

`pop()`, 删除末尾的元素



### tuple

```python
()
(1,)
(1, 2, 3, 4)
```



空tuple是`()`, 但是只有一个元素的tuple是`(element,)`



### dict





### set





## 高级特性

### slice

`[start:end:step]`

注意是`[,)`左闭右开区间

`[:]`复制一份





### iteration

可迭代(iterable): 可以作用于for循环`for ... in`

注意`...`位置可以是多个元素(tuple)

判断是否可迭代: 

````python
from collections.abc import Iterable
isinstance('abc', Iterable)
````



### List Comprehension

```python
[x * x for x in range(1, 11) if x % 2 == 0] # if作筛选条件
[m + n for m in 'ABC' for n in 'XYZ'] # 两层循环
[x if x % 2 == 0 else -x for x in range(1, 11)] # if-else作二选一条件
```



### generator

不是预先算好, 而是用到一个算一个, 减少内存消耗

把List Comprehension的`[]`改为`()`即可

或者用`yield`, 可以实现复杂的生成器



### iterator

生成器都是`Iterator`对象, 但`list`, `dict`, `str`虽然是`Iterable`, 却不是`Iterator`

把`list`, `dict`, `str`等`Iterable`变成`Iterator`可以使用`iter()`函数





## 自定义函数

### 参数

#### 位置参数

#### 默认参数 (positional argument)

有多个默认参数的时候, 可以在调用的时候指明参数的名称. 这样可以让前面的默认参数使用默认值, 而指定后面的默认参数. 

默认参数必须指向不变对象! 否则在多次调用函数的时候, 默认参数也会发生变化. 

#### 可变参数

函数定义的时候在参数前加`*`(一般叫`*argv`), 这样传进去的任意数量(包括0个)参数会自动组装成一个tuple

如果在函数外已经有一个list或者tuple, 想把它作为一个可变参数传进函数, 在调用的时候在前面加`*`

#### 关键字参数

一般写成`**kw`, 允许在调用的时候传进去任意个(包括0个)含参数名的参数, 它们会自动组装成一个dict

同理, 如果在函数外部已经有一个dict, 想把它作为关键字参数传进函数, 在调用的时候前面加`**`

#### 命名关键字参数

限制关键字参数的名字

在分隔符`*`(没有可变参数的情况)或者可变参数(如`*argv`)后面写出关键字的名字. 注意传入关键字参数的时候必须传入参数名. 可以有缺省值

注意如果没有缺省值的情况下, 命名关键字参数必须被传进去, 不能省略, 否则算缺参数. 

#### 参数定义的顺序

1. 位置参数
2. 默认参数
3. 可变参数`*`
4. 命名关键字参数
5. 关键字参数`**`

对于任意函数, 都可以通过类似`func(*args, **kw)`的形式调用它, 无论它的参数是如何定义的

#### 类型注解(type hints)

```python
def func(x: int, y: int) -> int:
```



注意Python解释器并不会因为注释而做额外的检查

但是IDE会针对这些注释而做语法高亮



## 面向对象

成员变量必须指定初始值, 或者用类型注解明确类型, 否则会报错`not defined`

注意list类型的变量如果只是用类型注解, 那么后续无法用下标访问. (这部分我还没完全搞懂, 是用类型注解就默认把它当作0长度的list了吗)



### 实例初始化 (构造函数)

`__init__(self, ...)` 初始化的时候是可以传值进去的

注意Python没有函数重载



### 创建一个实例

`varName = className()`



成员变量访问权限: 

`_`开头: 可以从外部访问但你最好不要这么干

`__`开头: 解释器会避免从外部访问



## 报错&异常处理

`raise TypeError('xxx')`





## 标准库

### 日期和时间





## 其他

删除对象: `del varname, ...`





## 读写文本文件

打开文件

`f = open(<path>)`

打开的文件最后一定要关闭! 

`f.close()`



读取文件

`f.read()` 读取文件为一个单独的字符串 (其中夹杂着换行符)

`f.readline()` 读取一行 (包括行尾换行符)

`f.readlines()` 读取全部文件为一个list, 其中每一个文件为一行 (包括换行符)



去除换行符

`s.strip()` 这个函数如果不指定去除什么, 就是把开头和结尾的空白符号全部去掉





---

Python有变量访问范围的问题吗

