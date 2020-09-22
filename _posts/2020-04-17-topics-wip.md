# Topics-Working In Progress

## CK 导数工作反思总结

- 遗留下来的表结构可能存在坑点，需要详细阅读遗留文档
- 梳理堡垒机备用开发测试环境，防止主用开发测试环境 DAS 平台挂掉，导致的任务无法推进的情况
- 测试阶段，设置足够的 logger 给出必要信息
- 有异常情况处理时，需要单独进行校验
- 结合 Notebook，cache 中间数据，加快 debug 速度
- JOIN 语句明确最左边 main key，必要时需独立生成 main key 表
- 字段中存在 null 值，where 语句比较大小过滤时，注意 null 会不被包含进去
- python assert 跨行括号应该只包含 string 语句

## CI、CD 怎么做的？有什么益处？适用条件是什么？

- 适用条件:
  - 对团队整体：需求不明确、不稳定，需要交付来获取反馈，或者？
  - 对团队成员：各个成员之间的工作耦合性强的，多需要依赖彼此工作的
- 益处：
  - 对团队整体来说？
  - 对协调团队内成员来说：彼此耦合性强的工作，提前发现问题，避免各自整体工作完成后不能合并，进而有大规模修改
- 怎么做：拆分大模块，将小模块走完整体流程

## 对个人来说，难以理解、难以解决的问题有什么共性？

## 如何进行任务优先级拆分

- 按照使用高频程度，高频热点先学习
- 按照抽象层级，先了解概况，再了解细节

## 数据存储方式选择 CSV、HDF5

- hdf5, row based
  HDF5 读写遇到了不稳定的情况，有时候会失败。原因肯能是，依赖的 Python 包版本问题？或者写 HDF5 大量开线程，文件符耗尽？应对方案：用 CSV 替换

## 高内聚、低耦合

### 破坏高内聚的来源

- 过度追求复用性

## 代码可读性

### Variable Name

命名原则: 尽量具体、有意义、容易理解、明确无歧义；缩写按惯例来，名字长一点没事

- Loop 变量: 短代码 i、k、j，长代码用有意义的词
- Status 变量：不用 flag，用实际意义词，如 data_ready、report_type
- Bool 变量：加修饰词如，done、found、success、ok、ready、available
- Temp 变量：尽量用有意义的词替代 tmp、temp 这种命名

### 代码可读性差的来源

- 引入了新技术，团队成员不熟悉，导致可读性变差

### 参考资料：

- 复用与可读性矛盾，重复与依赖 [Redundancy vs dependencies: which is worse?](http://yosefk.com/blog/redundancy-vs-dependencies-which-is-worse.html)

## Pro Python, Best practices

### pathlib vs os.path

- pathlib 更简洁

  pathlib 的 path 是一个对象，内部可以封装许多状态，使得接口更简洁；而 os.path 的 path 是一个字符串，封装许多层函数时，显得有点臃肿

  ```python
  import os.path

  BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))
  TEMPLATES_DIR = os.path.join(BASE_DIR, 'templates')

  # VS

  from pathlib import Path

  BASE_DIR = Path(__file__).resolve().parent.parent
  TEMPLATES_DIR = BASE_DIR.joinpath('templates')
  ```

- pathlib 更方便

  pathlib 将 path 主题的功能集成在一起，更方便使用；而 os.path 很多时候需要结合其他 package 来实现功能

  ```python
  import os
  import os.path

  os.makedirs(os.path.join('src', '__pypackages__'), exist_ok=True)
  os.rename('.editorconfig', os.path.join('src', '.editorconfig'))

  # VS

  from pathlib import Path

  Path('src/__pypackages__').mkdir(parents=True, exist_ok=True)
  Path('.editorconfig').rename('src/.editorconfig')
  ```

  ```python
  from glob import glob

  top_level_csv_files = glob('*.csv')
  all_csv_files = glob('**/*.csv', recursive=True)

  # VS

  from pathlib import Path

  top_level_csv_files = Path.cwd().glob('*.csv')
  all_csv_files = Path.cwd().rglob('*.csv')
  ```

- os.path 的好处

  - 代码更容易被看懂。

    os.path 的使用的多，大家普遍熟悉用法，而 pathlib 从 Python 3.6 开始才可以说是完善，使用的人较少，代码中用的较少。

#### 参考资料

介绍 pathlib vs os.path： [Why you should be using pathlib](https://treyhunner.com/2018/12/why-you-should-be-using-pathlib/)

### Python 3.6 版本以上的好东西

- f string
- pathlib.Path working with built-in open

## 反向传播算法(Backpropagation)

BP 算法算是链式求导法则的具体实现，计算时避免了许多重复计算，有一定的动态规划算法思想在里面

- 反向传播算法与符合函数求导法则（链式法则, Chain rule）是一回事吗？
- 反向传播与梯度下降有什么联系？
- 反向传播与动态规划算法(Dynamic programing)有什么联系？

参考资料：

- 维基百科对反向传播算法的介绍 [Backpropagation](https://en.wikipedia.org/wiki/Backpropagation)

- 从拉格朗日成乘数法角度看待反向传播算法 [Backprop is not just the chain rule](https://timvieira.github.io/blog/post/2017/08/18/backprop-is-not-just-the-chain-rule/)

- Quora Ian Goodfellow 的回答 [Is backpropagation in neural networks the same concept as the chain rule?](https://www.quora.com/Is-backpropagation-in-neural-networks-the-same-concept-as-the-chain-rule)

## 梯度下降法与 EM 算法

- 梯度下降与 EM 算法有什么关系？

### 参考资料：

- EM 算法的维基百科条目 [Expectation–maximization algorithm](https://en.wikipedia.org/wiki/Expectation–maximization_algorithm)

- 梯度下降与 EM 算法的关系 [梯度下降和 EM 算法：系出同源，一脉相承](https://spaces.ac.cn/archives/4277)

- 理解 EM 算法 [如何感性地理解 EM 算法？](https://www.jianshu.com/p/1121509ac1dc)

- 条件随机场 [An Introduction to Conditional Random Fields](https://arxiv.org/pdf/1011.4088.pdf)

## 如何提高沟通表达能力

### 通过多写作来提升语言组织能力

有了新的想法，脑子里想地时候心潮澎湃，写出来说出来，没味道了，感觉平平淡淡，为什么？能不能将心潮澎湃的感觉写出来，说出来？

- 考虑背景与目的
- 罗列要点
- 加入连接词、连接语句
- 用浅显易懂词替换相关句子

- 与人沟通前，首先捋清楚此次沟通的目的
- 注意气氛，向安全方向引导

## 重要理念总结

- 事情分两种，一种是自己能够控制的，一种不能控制，把精力放在可控的事情上
  - 别人对自己的评价是他头脑中的事，难以被自己控制，做好自己能做的就行了
  - 大环境、大背景的东西，自己没法控制，认识到之后，顺势而为
- 设定目标后，需要进行维度分解；目标的实现与否，存在不可控的因素，但各个维度的可控性相对较高
- 怎么识别不切实际的目标？想到它有如果有很大的压力、无力感，这个目标可能是不切实际的。目前对我来说，在北京定居涉及的户口、面积大而且位置好的房子、车牌，就是这样的不切实际的目标， 如果必须在北京不奢求户口、车牌，买个小点的房子，还是可以的，另外也可以去二三线城市。
- 考虑自己当下实际情况，拥有的东西，再对未来做规划，避免不切实际。别人有别人的情况，自己有自己的情况
- 追求自己认可自己，而不是让别人认可自己
- 被别人讨厌不可怕，要有被别人讨厌的勇气
- 保持开放性，避免绝对
- 对别人的建议，按可信度进行加权后考虑
- 在不重要的事情上，不去反对别人
- 优先做不愿意做但不得不做的事情
- 不能在人前说的话，不在人后说。聪明深察而近于死者，好议人者也。博辩广大危其身者，发人之恶者也
- 多依靠主题进行主动学习，而不是被动学习
