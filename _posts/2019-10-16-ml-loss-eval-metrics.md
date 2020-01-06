# Loss and evaluation metric

## 构造 Loss Function 的方式

### 最大似然（MLE）

### 最大后验（MAP）

- L1线性回归（Lasso回归）：权重W为拉普拉斯分布
- L2线性回归（Ridge回归）：权重W为高斯先验分布

> 一个普通的线性回归函数，给参数 w 加上一个高斯的先验分布，并用最大后验来构造目标函数，那么，这就相当于给目标函数加上一个 L2 正则项，这时的线性回归，也叫做岭回归（Ridge regression），这时的 logistic regression，叫做。。呃，L2 regularized logistic regression（= =），如果我们给参数 w 加上一个拉普拉斯的先验分布，那么我们可以让 w 变得更加稀疏。
>
> 作者：dontbeatmycat
> 链接：https://www.zhihu.com/question/24900876/answer/46567811
> 来源：知乎
> 著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

## Loss Function

https://heartbeat.fritz.ai/5-regression-loss-functions-all-machine-learners-should-know-4fb140e9d4b0

https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3885826/

## Evaluation Metrics

### Regression

<!-- prettier-ignore -->
| 评价指标 | 名称 | 计算式 | 特点 |
| ------ | -----| ----- | --- |
| RMSE   | Root-mean-squared Error | $\sqrt{\frac{\sum_{i=1}^{n}(y_{i} - \hat{y}_{i})^{2}}{n}}$ | 回归问题首先考虑采用的评估指标，较为常用，真实值正态分布的预测问题。 |
| MAPE   | Mean-absolute-percentage Error | $\frac{100\%}{n} \sum_{i=1}^{n} \left \| \frac{y_{i} - \hat{y}_{i}}{y_{i}} \right \|$ | 1. 相对于RMSE，减少了极值的影响 2. 存在$y_{i}$为零时的除零问题，需要考虑处理策略。a. 剔除零值，该指标会忽略一部分误差 b. 零值加一，会引入一部分误差。3. 适用于”真实值零值较少“的预测问题，特别适用于”真实值大部分取值为10以上“的预测问题，不适用”真实值零值较多“的预测问题，比如长尾效应非常明显的电商商品销量预测。|
| WMAPE  | Weighted-mean-absolute-percentage Error | $100\% \frac{\sum_{i=1}^{n} \left\| y_{i} - \hat{y}_{i} \right\|}{\sum_{i=1}^{n} y_{i}}$ |  1. 消除了MAPE的除零问题，侧重衡量$y_{i}$取值较大的样本。2. 较为适用作为销量预测的评价指标，偏重衡量高销量的商品 |
| NWRMSLE | Normalized-weighted-root-mean-squared-logarithmic Error |  |This metric is suitable when predicting values across a large range of orders of magnitudes. It avoids penalizing large differences in prediction when both the predicted and the true number are large: predicting 5 when the true value is 50 is penalized more than predicting 500 when the true value is 545. |

Note:
$y_{i}是样本i的真实值$，$\hat{y}_{i}是样本i的预测值$， $n$代表样本总数。

### 如何选取回归问题的评价指标

原则：根据真实值的分布，选取。

1. 正态分布：RMSE
2. 偏向高销量-偏态分布：MAPE
3. 偏向低销量-偏态分布(长尾效应)：WMAPE

### Classification

## 参考

[Mean_absolute_percentage_error](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)

[measuring-forecast-accuracy](https://forecastsolutions.co.uk/measuring-forecast-accuracy.htm)
