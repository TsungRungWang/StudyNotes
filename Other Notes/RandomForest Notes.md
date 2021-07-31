# RandomForest Notes

特征重要性评价标准

%IncMSE 是 increase in MSE。就是对每一个变量 比如 X1 随机赋值, 如果 X1重要的话, 预测的误差会增大,所以 误差的增加就等同于准确性的减少,所以MeanDecreaseAccuracy 是一个概念的.

IncNodePurity 也是一样, 如果是回归的话, node purity 其实就是 RSS（残差平方和residual sum of squares） 的减少, node purity 增加就等同于 Gini 指数的减少,也就是节点里的数据或 class 都一样, 也就是 Mean Decrease Gini。

---

%IncMSE是Increased in mean squared error (%)，直译为增长的错误率平方均值，即去除该变量后，对目标预测的准确度下降的低(低?)，可理解为对目标变量预测准确的贡献度。IncNodePurity是Increased node purity，是另一种评估的方法。这里我们只关注%IncMSE就够了。
————————————————
版权声明：本文为CSDN博主「刘永鑫Adam」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/woodcorpse/java/article/details/80153357

---

打印RandomForest的结果: 

```R
Call:
 randomForest(formula = EF ~ ., data = traindata, mtry = 4, ntree = 600,      importance = TRUE, na.action = na.omit) 
               Type of random forest: regression#分析类型
                     Number of trees: 600#ntree
No. of variables tried at each split: 4#mtry

          Mean of squared residuals: 0.5961805#平均残差平方
                    % Var explained: 44.82#解析率
```

