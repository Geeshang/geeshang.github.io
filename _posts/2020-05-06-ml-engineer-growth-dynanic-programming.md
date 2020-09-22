# Dynamic Programming

- 研究起源：对多阶段决策过程(multi-stage decision process)的最优化的研究
- 应用场景：初始决策确定后，后面的决策可保证最优的决策过程

> 递归加 memory 和预计算刷表只是实现上的两种技巧，其本质是一样的，递归加 memory 更接近本质，刷表则是一种优化
>
> > 动态规划本质是不是就是递归算法，再加上记忆功能呢？ - sin1080 的回答 - 知乎 https://www.zhihu.com/question/323076638/answer/673995021

## 难点

- 递推关系不好得到
- Bottom Up 解法，for i j index 初始位置不一定是从 0 开始，需要确定实际的 Bottom 是哪，从而确定 i, j index 的初始结束位置

## 考虑的步骤

1. 先得到递推关系

   - 选择什么量作为规模变量？比原问题小一点的问题是什么？小问题如何解决大一点的原问题的？
   - 从多步决策角度思考，当前步骤有哪几种选择，怎么计算得到最优选择？认为不管当前如何选择，前置与后续步骤的决策过程无需考虑，都可以拿到对应的最优解
   - 伪代码表达出递推关系

2. 将递推表达式，转换为带 memory 的递归代码

   - 将递推关系转换为递归代码
   - 考虑递归出入口
   - 考虑异常输入

3. 将递归解法转换为 Bottom up 解法， 进而优化时间复杂度大 O 的常数：

   - 该小问题如何用一个 state 定义表示？该 state 定义为什么，才能包含足够的信息且不冗余？
   - 小问题到规模大一点的问题 state 如何变化转移？

4. Bottom Up 解法考虑如何用变量替代表格存储，进而优化空间复杂度

## 参考资料

- DP 发明者 Richard Bellman 对 DP 较为系统的介绍 [The Theory of Dynamic Programming](https://www.rand.org/content/dam/rand/pubs/papers/2008/P550.pdf)

- Leetcode DP 套路总结 [From good to great. How to approach most of DP problems.](https://leetcode.com/problems/house-robber/discuss/156523/From-good-to-great.-How-to-approach-most-of-DP-problems.)
