# 时序预测综述

## Amazon DeepAR

### DeepAR 的核心特点

- 基于 RNN 模型，拟合一个概率分布

  1. 选定一个概率分布，如高斯分布(Gaussian distribution)、负二项分布(Negative binomial distribution)等
  2. 用 RNN 网络的输出，来拟合它的概率密度函数(Probability density function)或概率质量函数(probability mass function)的参数
  3. 然后用拟合好的概率分布来产生预测值

  拟合概率分布，相当于限制模型解空间？

- 输入数据组织上具有 AR(Auto Regressive)的特点

  当前 step 的输入，整合了之前几个 step 的 target，与 Auto Regressive 的概念具有类似性，所以命名为 DeepAR

- 为了应对 target 量级差别较大情况, 提出一种 scale 概率分布参数的方法

## Facebook Prophet

### Prophet 的核心特点

## 参考资料

- [Prophet vs DeepAR: Forecasting Food Demand](https://towardsdatascience.com/prophet-vs-deepar-forecasting-food-demand-2fdebfb8d282)
- [Failing Fast with DeepAR Neural Networks for Time-Series](https://medium.com/slalom-technology/failing-fast-with-deepar-neural-networks-for-time-series-ef442bf03567)
- [DeepAR Presentation](https://www.slideshare.net/CyrusMoazamiVahid/deep-ar-presentation)
  [DeepAR Presentation-知乎](https://zhuanlan.zhihu.com/p/61896500)

- [3 facts about time series forecasting that surprise experienced machine learning practitioners.](https://towardsdatascience.com/3-facts-about-time-series-forecasting-that-surprise-experienced-machine-learning-practitioners-69c18ee89387)

  - 特征随时间发生变化，预测前需要重新训练
  - 时序数据较少，划分 test 导致可用 train 数据太少，模型选择多用相对选择法如 AIC、BIC 等
  - 区间预测、不确定性预测与点预测一样重要
