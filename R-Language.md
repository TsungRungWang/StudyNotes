# R语言学习笔记

快捷键: `Alt` + `-`即可打出`<-`

###### 包的管理

`install.packages('packagename')`安装包. 也可以在网上搜索这个包, 然后在RStudio中手动安装. 

清华镜像站: https://mirrors.tuna.tsinghua.edu.cn/CRAN/src/contrib/. 或者在Bing中搜索"CRAN <包名称>". 

`library('packagename')`导入包. 类似于Python中的`import <modulemane>`.  

`installed.packages()`查看已经安装的包

`.libPaths()`查看目前安装的包的位置, `.libPaths(<dir>)`, 设置安装包的路径. 注意, 这种设置在退出R后就会失效. 

### R中的数据结构

#### 向量Vector

向量就是一个一维数组. 向量中的元素必须是同一类型 (数值, 字符, 逻辑值)

R语言中的逻辑值: `TRUE`, `FALSE`

###### 构建一个向量

`a=c(1,2,3,4)`. `c()`为R中创建向量的语法. 

###### 对向量进行索引

向量的索引从`1`开始. 索引值是一个数值列表, 用**方括号**括起来, 返回的是在索引位置上的元素的列表

`a=c(6,3,8,4)`
`a[1]`返回`6`
`a[2:3]`返回`3 8`. 这个方法类似matlab中的操作. 不过不能指定步长, 即无`start:end:step`这种写法. 
`a[c(4,2)]`返回`4 3`


也可以用逻辑表达式作为索引

`a=c(1,5,2,7,4,6,2)`
`a<3`返回`TRUE FALSE  TRUE FALSE FALSE FALSE  TRUE`
如果用`a<3`作为索引, 则可以返回`a`中所有小于`3`的元素. 

找到向量中等于某个值的元素: `which()`

`a=c(1,5,2,7,4,6,2)`
`which(a==2)`返回`3 7`
对比: `a==2`返回`FALSE FALSE  TRUE FALSE FALSE FALSE  TRUE`

找向量中的最大/最小值: `which.max()`, `which.min()`, 返回第一个最大/最小值的索引

###### 删除向量中元素

用负索引. (注意与Python不同的是, Python中的负索引是用来倒序索引的)

`a=c(8,2,5,6,7,4)`
`a[-2]`返回`8 5 6 7 4`. 注意此时`a`本身并没有发生变化, 除非`a=a[-2]`. 
`a[-2:-4]`返回`8 7 4`. 一次删除多个元素. 

###### 生成随机向量

设置随机函数的种子: `set.seed(250)`. `250`为种子编号. 注意种子被设定后只能用一次. 

`runif(3,min=0,max=100)`, 生成包含`3`个元素的向量, 元素在`min`和`max`间均匀分布. 

其他分布: `rnorm()`, `rexp()`, `rbinom()`, etc

###### 取整

`floor(a)`, 向下取整

`ceiling(a)`, 向上取整

`round(a,4)`, 保留`4`位小数

#### 矩阵Matrix

matrix矩阵是一个二维数组. 它能存储的数据类型和向量是一样的. 

###### 创建一个矩阵

`x=matrix(1:20,5,4,TRUE)`, 参数分别为数据, 行数, 列数, 是否按行填充. 默认是按列填充的. 

###### 对矩阵进行索引

基本思路为`[a,b]`, 即查找第`a`行`b`列的元素. 如果`a`, `b`不填则为该行/列的全部元素. 注意`a`, `b`本身就是向量, 因此也可以用`start:end`或`c()`方式生成. 如果`a`, `b`均含有不止一个元素, 则切出来的是一个二维子矩阵. 

也可以只用一个向量对矩阵进行索引, 此时矩阵按列首尾相连为一个向量. 

###### 给矩阵的行/列命名

`rownames(x) <- rowname`. `rowname`为字符型向量, 它的长度必须和`x`的行数相等. 对列命名用`colnames()`

命名后仍然可以用位置来进行索引

#### 数组Array

即多维矩阵. 

###### 创建数组

方法如下
```R
dim1=c('A1','A2')
dim2=c('B1','B2','B3','B4')
dim3=c('C1','C2','C3')
dim4=c('D1','D2')
z=array(1:48,c(2,4,3,2),list(dim1,dim2,dim3,dim4))
```
`array()`的参数依次是数据, 每个维度的大小, 每个维度的名称. 

数据的填充顺序: 先固定除第一个维度以外的其他维度, 对第一个维度依次填充, 再填充第二个维度, 以此类推. 

#### Data Frame数据框

类似矩阵, 不过它的列可以放不同类型的元素. 相当于把多个 (相同长度的) 向量并排放在一起. 类似一个带标题的Excel表, 每一列的数据必须是同一种类型. 

是R语言中最常见的数据结构

###### 创建Data Frame

- `data.frame(a,b,c,...)`, 参数为多个向量. 生成的Data Frame的列名即为用来生成它的向量名. 
- 创建空的数据框: 例: `data <- data.frame(ID= character(), age= numeric(), stringsAsFactors=FALSE)`

###### 对Data Frame进行索引

注意, 如果只有一个作为索引的向量, 

-   如果这个索引向量只含有一个元素, 
    -   `x[2]`取出来的是只包含第二列的子Data Frame; 
    -   而`x[,2]`取出来的, 
        -   如果该列是数值型或逻辑值型, 取出的是第二列的向量, 
        -   如果该列是字符型元素, 取出的是`factor`型变量. 
-   如果这个索引向量包含两个以上的元素, 那无论是`a[1:2]`还是`a[,1:2]`取出的都是子Data Frame

`数据框名$列名`, 用来取到那一列. 

`attach()`, 可以把一个Data Frame的列名设置为可以检索到的矢量. 用`detach()`取消这个设定. 注意, 这样操作后"生成"的变量并不会在变量窗口显示, 但的确可以检索到. 



#### List

R中最复杂的数据集. 不受数据类型和数据长度限制, 它可以让你把多个变量放在同一个变量的名下. 说白了就是个把变量放在一起的粗暴操作. 

###### 创建list

`list(a,b,c,...)`, 参数可以为向量, 矩阵, 数组, Data Frame或者其他list. 

###### 对list的索引

假如我用`mylist=list(a,b,c)`创建了一个包含三个矩阵的list, 那么`mylist[2]`返回的结果是只包含一个元素 (矩阵`b`) 的子list, 它仍然是list (即, 和`list(b)`的返回值一致). 如果要得到`b`这个矩阵, 要用`mylist[[2]]`

#### 1.6. 因子Factor

用来处理离散型变量

`levels()`用来查看具体包含哪些水平. `nlevels()`用来查看有几个水平. 



### 2. 工作目录

注意R语言中路径用的是正斜杠`/`. 

#### 设定工作目录

`setwd(<path>)`. 工作空间设定后, 打开文件只需要它的相对路径. 

注意, 如果已经设定了一个工作路径, 再需要改变工作路径, 也可以用相对路径. 比如, D盘下有一个文件夹`A`, 后者下面又有一个文件夹`B`, 那么, `setwd(D:/A)`, 把工作路径设定到`D:/A/`, 如果此时想把工作路径设定到`D:/A/B/`, 只需要 `setwd(./B)`, 当然用`D:/A/B`也是可以的

#### 查看当前工作目录

`getwd()`







### 3. 读取文件

`read.csv()`会读取标题栏为列名, 即返回的是一个Data Frame, 而`read.table()`会把标题栏当作第一行数据. 

设定工作目录后, 可以在RStudio右下侧`Files`窗口查看该路径底下的文件, 并可以"**左键**->Import Dataset…". 该方法实际上是调用了`read_excel()`函数









### 4. 绘图

分幅: `par()`. 先执行这个语句, 然后画的图就会依次出现在上面

> R中的par()函数可以将绘图区分割成规则的几个部分。
>
> 多图环境用参数mfrow或参数mfcol来设定，如：
>
> par(mfrow=c(3,2))
>
> 则是在同一绘图区中绘制3行2列共6个图形，而且**是先按行绘制**，即绘制完第1行的2个图形后，再绘制第2行的2个图形，最后是第3行的2个图形。同理，
>
> par(mfcol=c(3,2))
>
> 也是绘制3行2列共6个图形，与上面不同的是，**先按列绘制**。即先绘制完第1列的3个图形，再绘制第2列的3个图形。
>
> par设定的绘图参数直至退出前都会有效，即使是在某个函数中使用par()设定的参数，也会影响全局的效果，所以如果在绘图中需要恢复到初始状态，可以设置临时变量保存初始环境。在准备恢复时再使用par(临时变量)的形式恢复到初始状态。

关闭绘图设备: `dev.off()`

`qqnorm()`, see`stats::qqplot`

`qqline()`

`line()`

#### `plot(x,y)`

画x-y散点图. 如果是`plot(x)`, 就是画index-value的散点图. 

#### `hist()`

画直方图. 

- `breaks=1000`, 设置画出来的直方图有多少根柱子. 
- `freq=TRUE`则y轴为frequency, 否则为density

#### `plot(density())`

画密度函数图. 

#### `boxplot()`

画箱图. 里面可以有1\~多个变量, 每个变量都会画出一个箱图. 

#### `qqplot(a,b)`

即百分位-百分位图, 用来验证两组数据是不是属于同一种分布. `qqnorm(a)`, 用来检验`a`是否符合正态分布. `qqline(a)`, 给`qqnorm()`图加上理论趋势线. 

### 5. R中的v控制流

#### 5.1. 循环

###### `for`

```R
for(i in 1:10){
	# do something
}
```

`in`后面的内容只要是一个可迭代对象即可 (目前我还不清楚是必须要向量, 还是矩阵或数组均可)

###### `while`

```R
i=0
while(i<10){
	# do something
}
```

#### 5.2 判断

###### `if`-`else if`-`else`

```R
if(a==1){
    # do something
}
else if(a==2){
    # do something else
}
else{
    # do something plus else
}
```

###### `switch`

```R
switch(condition,
       judge1=returnvalue1,
       judge2=returnvalue2
)
```

`condition`进入`switch`结构后会依次与`judge1`, `judge2`, ...进行对比, 若与哪个相等, 便返回等号右边的`returnvalue`. 

#### 5.3 自定义函数

```R
myfunction=function(a,b,c,d,...){
    # do something
    return(# something)
}
```

`return`后面的括号必须要写, 因为这也是个函数. 



### 6. 常用指令

清除某个变量: `rm(<变量名>)`

查看数据的若干方式

`summary()`

`str()`

`head()`













### 6. 常见函数

#### `cbind()`

横向合并多个矩阵/数据框. 类似地, `rbind()`用来纵向合并矩阵(是否能合并数据框不清楚). 

#### `scale()`

将数据中心化(减去均值使得均值为0)和标准化(除以方差使得方差为1). 

#### `subset()`

根据条件查询. 

-   第一个变量为待筛选的变量, 
-   第二个变量为逻辑表达式, 即筛选条件, 即只会返回满足条件的行, 
-   `select=`后填入向量索引或者列名, 表示只返回这些列, 负数索引表示不返回这些列. 

#### `ncol()`/`nrow()`

计算列数/行数. 

#### `colnames()`

给数据框赋予新的列名. 如: `colnames(data)[23:32]<-c("BD","Clay","SOC","pH","tmp","prec","PET","PPE","Nrate","EF")`. 

#### `dplyr::select()`

按照列名得到数据框的子数据框. 第一个变量是数据框, 后面为若干个变量, 依次为列名. 

#### `apply()`

用来批量操作. 减少循环的使用. 

-   第一个变量为待处理的数据, 
-   第二个变量: `1`: 基于行操作; `2`:基于列操作; `c(1,2)`对行列都进行操作, 
-   第三个变量为要用到的函数. 可以写临时函数形如: `function(parameter)<return value>`, 也可以用`sum()`, `var()`等内置函数. 

`apply()`函数是一个家族, 还有很多类似的函数. 

#### `paste()`

用来连接若干个字符串. 如果输入了列表, 会得到该列表每一个元素进行连接操作后的新列表, 这时`collaspse`用来将这个列表连接成一个单一字符串. 

#### `sink()`

用来使R输出到文件. 

```R
sink(<文件路径>) #此时这个文件被创建出来了
print(<something>) #此时数据并没有被写进文件, 它们还在内存里
sink() #把内存里的数据写入文件
```

这样`print()`的内容就会被写进文件, 而不是被打印在命令行里. 

#### `Cairo`

是一个用来画图的函数. 

以`CairoPNG()`为例: 用法和`sink()`类似. 执行`Cairo()`语句后, 之后执行的`plot()`等绘图命令便会把图画在图片文件上. 最后`dev.off()`输出图像. 

#### `table()`

用来统计出现的频数. 

#### `names()`

用来获取数据中所有变量的名字. 

`names(A)<-names(B)`即把`B`中的名字赋给`A`. 

#### `lm()`

线性拟合. 

`seq()`

生成一个均匀数列, 如`seq(1,20,30)`, 生成1起始, 20结束, 共30项的等差数列

### 7. 与离散分布有关的函数

|            | 包名             | 函数名     | 参数 |
| ---------- | ---------------- | ---------- | ---- |
| 泊松分布   | `Poisson`        | `pois`     |      |
| 二项分布   | `Binomial`       | `binom`    |      |
| 多项分布   | `Multinom`       | `multinom` |      |
| 几何分布   | `Geometric`      | `geom`     |      |
| 超几何分布 | `Hypergeometric` | `hyper`    |      |
| 负二项分布 | `NegBinomial`    | `nbinom`   |      |

前缀:

- `d`: 密度函数, 根据$x$给出该点分布函数的密度
- `p`: 分布函数, 根据$x$给出该点左侧的累积密度(左侧所围面积之和)
- `q`: 分位数函数, 根据累计密度, 给出$x$
- `r`: 随机函数, 生成$n$个特定分布的随机数

参数: 

- `lower.tail`, 如果`TRUE`(缺省), 则计算的是$P(X\le x)$的概率, 否则就是$P(X>x)$的概率. 



| 名称       | 英文名             | R对应的函数 | 参数           |
| ---------- | ------------------ | ----------- | -------------- |
| 泊松分布   | Poisson            | Pois        | n, lambda      |
| 二项分布   | binomiaminal       | Binom       | n, size,  prob |
| 多项分布   | multinominal       | multinom    | n,  size, prob |
| 几何分布   | geometric          | geom        | n, prob        |
| 超几何分布 | hypergeometric     | hyper       | n,  m, n, k    |
| 负二项分布 | negative  binomial | nbinom      | n, size,  prob |

### 8. 与连续分布有关的函数

|          | 包名 | 函数名 | 参数 |
| -------- | ---- | ------ | ---- |
| 均匀分布 |      |        |      |
| 指数分布 |      |        |      |
| 正态分布 |      |        |      |
|          |      |        |      |



| 名称          | 英文名       | R对应的函数 | 参数                    |
| ------------- | ------------ | ----------- | ----------------------- |
| 高斯分布      | gaussian     | norm        | n,  mean=0, sd=1        |
| 指数分布      | exponential  | exp         | n,  rate=1              |
| 伽玛分布(γ)   | gamma        | gamma       | n, shape,  scale=1      |
| 韦氏分布      | Weibull      | weibull     | n, shape,  scale=1      |
| 柯西分布      | Cauchy       | cauchy      | n, location=0,  scale=1 |
| β分布         | beta         | beta        | n, shape1,  shape2      |
| t分布         | Student's  t | t           | n, df                   |
| F分布         | F            | f           | n, df1,  df2            |
| 卡方分布      | chi-squared  | chisq       | n, df                   |
| Logistic 分布 | Logistic     | logis       | n, location=0,  scale=1 |
| 对数正态分布  | log-normal   | lnorm       | n, meanlog=0,  sdlog=1  |
| 均匀分布      | uniform      | unif        | n, min=0,  max=1        |



计算样本平均值: `mean()`

计算样本方差: `var()`

计算样本标准差: `sd()`

`mediam`



检验样本是否来自服从**正态**分布的总体

`shapiro.test()`

- 样本大小3~5000, 超过5000要用ks检验
- 检验统计量$W=...$
- $H_0$: 样本来自正态分布的总体. 即若$p<\alpha$, 说明样本来自非正态总体, 否则来自正态总体
- see: [Shapiro–Wilk test](https://en.wikipedia.org/wiki/Shapiro%E2%80%93Wilk_test)

Kolmogorov–Smirnov test: `ks.test()`

- 拟合优度检验
- 要指定待检验的分布, 以及分布的均值标准差, etc



判断方差齐性: `var.test()`

- 检验统计量$F=...$
- $H_0$: 两个正态分布的总体方差相同. **注意必须事先检验两个总体的正态性**
- see: [*F*-test of equality of variances](https://en.wikipedia.org/wiki/F-test_of_equality_of_variances)

画Q-Q图

判断方差齐性: `bartlett.test`



$p<\alpha$, 则可推翻原假设(选择备择假设)

需要查看有哪些具体的分布类型时, 用`?Distributions`





[李东风的主页](https://www.math.pku.edu.cn/teachers/lidf/)





*t*-检验

- *t*-检验一定只能用于**正态分布**的数据, **必须事先检验正态性**, 对于非正态分布的数据, 有其他非参数检验的替代方法(在下文以*斜体*标出)

三种*t*-检验: 

- 单样本*t*-检验`t.test()`. *单样本Wilcoxon检验`wilcox.test()`(秩和检验)*
- 差值服从正态分布: 成对样本Paired *t*-检验`t.test(paired=TURE)`. *配对Wilcoxon检验`wilcox.test(paired=TRUE)`*
- 两组数据均服从正态分布且方差齐性: Two Sample *t*-检验. *Mann-Whitney检验*
- 两组数据均服从正态分布但是**方差不齐**: Aspin-Welch检验(近似*t*-检验)
- see: [Wilcoxon signed-rank test](https://en.wikipedia.org/wiki/Wilcoxon_signed-rank_test), [Mann–Whitney *U* test](https://en.wikipedia.org/wiki/Mann%E2%80%93Whitney_U_test)











---



二项分布

- 例: 每个零件的次品率为$5\%$, 检测20个零件, 问抽到2个及以上次品的概率为`1-pbinom(2,20,0.05)=pbinom(2,20,0.05,lower.tail=FALSE)`. 该值即为$p$值



`sample`: 从样本中抽样