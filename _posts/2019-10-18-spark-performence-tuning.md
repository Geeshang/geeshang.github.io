# Spark 性能调优

## Shuffle 读写特别慢

### 可能的原因：

1.  启用了 spark.shuffle.service.enabled，而 shuffle service 作为外部服务，如果负载过高，会大大降低 shuffle 读写的效率

    解决方法：设置 spark.shuffle.service.enabled 为 false，相应的把 dynamicAllocation 设置为 false

## Debug Spark

如果 yarn deploy-mode cluster 模式 log 错误日志无法定位问题，换成 deploy-mode = client 试一试