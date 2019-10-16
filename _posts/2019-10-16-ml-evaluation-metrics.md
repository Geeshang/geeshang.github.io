# Evaluation Metrics

## Regression

<!-- prettier-ignore -->
| 评价指标 | 名称 | 计算式 | 特点 |
| ------ | -----| ----- | --- |
| RMSE   | Root-mean-squared Error | $\sqrt{\frac{\sum_{i=1}^{n}(y_{i} - \hat{y}_{i})^{2}}{n}}$ | 较为常用 |
| MAPE   | Mean-absolute-percentage Error | $\frac{100\%}{n} \sum_{i=1}^{n} \left \| \frac{y_{i} - \hat{y}_{i}}{y_{i}} \right \|$ | 相对于RMSE，减少了极值的影响|
| WMAPE  | Weighted-mean-absolute-percentage Error | $100\% \frac{\sum_{i=1}^{n} \left\| y_{i} - \hat{y}_{i} \right\|}{\sum_{i=1}^{n} y_{i}}$ |  消除了MAPE的除零问题，侧重$y_{i}$取值较大的样本。|
| NWRMSLE | Normalized-weighted-root-mean-squared-logarithmic Error |  |This metric is suitable when predicting values across a large range of orders of magnitudes. It avoids penalizing large differences in prediction when both the predicted and the true number are large: predicting 5 when the true value is 50 is penalized more than predicting 500 when the true value is 545. |

Note:
$y_{i}是样本i的真实值$，$\hat{y}_{i}是样本i的预测值$， $n$代表样本总数。

## Classification

## 参考：

[Mean_absolute_percentage_error](https://en.wikipedia.org/wiki/Mean_absolute_percentage_error)

[measuring-forecast-accuracy](https://forecastsolutions.co.uk/measuring-forecast-accuracy.htm)
