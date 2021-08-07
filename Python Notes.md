# Python学习笔记

## 启动Python

Linux: `python3`

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



## 参考教程&资料

[官方文档](https://docs.python.org/3/)

[廖雪峰的Python教程](https://www.liaoxuefeng.com/wiki/1016959663602400)

菜鸟教程: [Python基础教程](https://www.runoob.com/python/python-tutorial.html)

在线文档: [Problem Solving with Algorithms and Data Structures using Python](https://runestone.academy/runestone/books/published/pythonds/index.html)

数据结构于算法可视化: [VisualGo](https://visualgo.net/en)



## pip&模块

`pip`和`pip3`是啥关系(?

> 来自 [Wybxc](https://github.com/Wybxc) 的回答：
> 因为 linux 某些系统预装了 python2，这时候 `pip` 命令指的是 python2 的 pip，用 `pip3` 可以强制指定为 python3 版本的 pip。
> 如果不用 `pip3`，用 `python3 -m pip` 也是一样的。

`pip list` 查看已安装列表

`pip list --outdate` 查看可升级的包

`pip install --upgrade <pkgName>` 升级某个包

`pip install <pkgName>` 安装包

> [Python pip 安装与使用](https://www.runoob.com/w3cnote/python-pip-install-usage.html)

- 如果报错`ValueError: check_hostname requires server_hostname`, 关梯子试试



关于创建虚拟环境, 避免包版本冲突: [Virtual Environments and Packages](https://docs.python.org/3/tutorial/venv.html)



注意, Linux下不同用户的库是不一样的, 

自带的库位于`/usr/lib/python2.7/dist-packages` or `/usr/lib/python3/dist-packages` or `/usr/lib/python3.8`

通过`pip`安装的库位于`~/.local/lib/python3.x/site-packages`







## 基本运算

| 运算符         |                         |
| -------------- | ----------------------- |
| `//`           | 整除(地板除)            |
| `%`            | 求余数                  |
| `/`            | "真"除法, 的到小数      |
| `divmod(a, b)` | 商和余数(返回一个tuple) |
| `m ** n`       | 乘方                    |
| `abs(n)`       | 绝对值                  |
|                |                         |



## 输入输出

`input()`  输入. 注意输入默认为字符串



## 逻辑运算





当前位置: https://www.liaoxuefeng.com/wiki/1016959663602400/1017075323632896



## 内置数据结构

[Built-in Types](https://docs.python.org/3/library/stdtypes.html)

### 字符串



### List



### Dict



### Set





### 



### 
