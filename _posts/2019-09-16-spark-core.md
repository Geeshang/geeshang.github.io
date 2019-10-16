# Spark内核原理解析

不光能运用工具，同时也要了解工具的原理，从而才能更好的运用工具，了解其局限性，知道它适合做什么、不适合做什么，并且能改造工具，使其更能适应自己的工作，一方面是从通用性改像专用性改造，另一方面是添加个性化的需求，这是我想要了解Spark内核原理的动机。

工作以来，Spark是我最用到的工具，主要用来做机器学习算法的预处理工作，然后连接第三方机器学习库比如XGBoost、LightGBM，来做机器学习算法。

## Spark on YARN 部署

一般公司内常用的集群工具是YARN，而又会有内部团队，根据Spark开源版本进行了个性化定制，包括一些功能扩展与BUG的修复。下面主要讨论Spark on YARN部署维护相关问题。

- 问题：Spark部署需要在集群每个节点安装相关库吗？
- 问题：部署维护一个Spark集群，需要的工作有哪些？

部署一个Spark集群很简单，对于YARN管理的集群来说，只需要在一台机器上有一个可用的Spark JAR包文件，启动Spark的机器，配置好YARN相关的环境变量即可，不需要所有的集群节点安装与Spark相关的东西，Spark本身与Hadoop技术栈并没有什么特别的关系，同样可以部署在Kubernetes和Mesos管理的集群上。不过由于Hive作为事实上的数据仓库标准，将Spark部署在YARN上也是很自然的事，很方便来使用HIVE作为一切工作的数据源。从Hive表中读取数据，处理后将数据存入Hive表中。

## Spark接口

- Spark开发接口如何选择？写SQL、还是用DataFrame API、还是RDD API?

从开发应用程序，使用Spark的角度，我们有三种选择：

1. SQL
2. DataFrame API (Python、Scala、Java)
3. RDD API (Scala、Java)

而RDD底层的API接口，在Spark升级到2.0之后，就不推荐写了，主要原因是用起来繁琐、不方便，另一个原因是，写出的性能很可能不如DataFrame API，通过DataFrame API写出的代码，Spark内部可以做很多优化，可以消除一些常见的性能问题。

下面的主要介绍Spark SQL以及Spark DataFrame API中的Python接口，也就是PySpark的相关原理，Python接口和Scala、Java接口底层共享一套执行引擎，运行效率没有什么差别。在实际代码中混合SQL与DataFrame两种写法是很常见的，可以结合两者的优点。Spark支持的SQL中没有循环，因而不是图灵完备的语言，表达能力有限。

## 接口概述

- 问题：Spark SQL与PySpark DataFrame API共用一套底层引擎，这两种接口与底层引擎是怎么联系起来的？

### Spark SQL

- 问题：Spark SQL从SQL语句到实际执行，中间经历了什么阶段？

### PySpark DataFrame API

- 问题：Spark SQL从SQL语句到实际执行，中间经历了什么阶段？

## Machine Learning Lib

- 问题：ML库是怎么设计的，通用框架是哪些？
- 问题：ML库的局限性有哪些？为什么集成树要使用第三方库如XGBoost、LightGBM而不使用内置库中的TreeBoosting?

参考资料：

- Spark SQL内核剖析，朱峰等。
- Spark: The Definitive Guide, Bill Chambers et al.
- [Spark SQL: Relational Data Processing in Spark](http://people.csail.mit.edu/matei/papers/2015/sigmod_spark_sql.pdf). Michael Armbrust, Reynold S. Xin, Cheng Lian, Yin Huai, Davies Liu, Joseph K. Bradley, Xiangrui Meng, Tomer Kaftan, Michael J. Franklin, Ali Ghodsi, Matei Zaharia. SIGMOD 2015. June 2015.
- [Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing.](http://people.csail.mit.edu/matei/papers/2012/nsdi_spark.pdf) Matei Zaharia, Mosharaf Chowdhury, Tathagata Das, Ankur Dave, Justin Ma, Murphy McCauley, Michael J. Franklin, Scott Shenker, Ion Stoica. NSDI 2012. April 2012. Best Paper Award.
- [MLlib: Machine Learning in Apache Spark.](http://www.jmlr.org/papers/volume17/15-237/15-237.pdf) Xiangrui Meng, Joseph Bradley, Burak Yavuz, Evan Sparks, Shivaram Venkataraman, Davies Liu, Jeremy Freeman, DB Tsai, Manish Amde, Sean Owen, Doris Xin, Reynold Xin, Michael J. Franklin, Reza Zadeh, Matei Zaharia, and Ameet Talwalkar. Journal of Machine Learning Research (JMLR). 2016.
