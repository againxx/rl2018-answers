# Exercises of  Chapter 08
## 8.2
#### Ex8.1 The nonplanning method looks particularly poor in Figure 8.3 because it is a one-step method; a method using multi-step bootstrapping would do better. Do you think one of the multi-step bootstrapping methods from Chapter 7 could do as well as the Dyna method? Explain why or why not.
解: 还是没有Dyna快, n-step方法虽然一次可以更新n个状态的Q值, 但是Dyna算法可以利用所有过去的经验不断将更新后的Q值回溯更新到更之前的状态,
所以一幕结束时得到更新的状态数量远多于n-step方法, 所以在环境模型准确时, Dyna更优一些.

## 8.3
#### Ex8.2 Why did the Dyna agent with exloration bonus, Dyna-Q+, perform better in the first phase as well as in the second phase of the blocing and shortcut experiments?
解: 由于具有额外的exploration bonus, Dyna-Q+可以更快的发现最优策略. 所以再第一阶段, Dyna-Q+更早开始利用最优策略, 从而获得更高的cumulative reward.
当3000步之后环境发生变化时, Dyna-Q+同样能更早的发现新捷径, 从而表现更优.

#### Ex8.3 Careful inspection of Figure 8.5 reveals that the difference between Dyna-Q+ and Dyna-Q narrowed slightly over the first part of the experiment. What is the reason for this?
解: 在Dyna-Q发现最优策略后, Dyna-Q+仍然会因为exploration bonus而时不时的探索非最优策略, 从而浪费了一些步数, 使得双方之间的差距缩小.

#### Ex8.5 How might the tabular Dyna-Q algorithm shown on page 164 be modified to handle stochastic environment? How might this modification perform poorly on changing environments such as considered in this section? How could the algorithm be modified to handle stochastic environments and changing environments?
解:
* 对每一对动作状态二元组(S, A), 维护一个状态转移数组, 用不同状态转移出现的频率来建模环境的动态转移概率(相当于维护一个分布模型)
* 更新时由Q-Learning 采样更新变为期望更新
* 变换的环境可能只是转移概率发生了改变, 需要花很长时间才能反应在模型之中(搜集到足够多的新的转移采样数据之后)
* 改进方法是增加一个exploration bonus, 类似UCB的设计$\kappa \frac{\sqrt{\tau}}{\sqrt{n}}$, 其中$n$是访问过的总次数
* 类似答案中的方法, 单纯使用距离上次访问过的时间应该是不妥的, 因为在随机环境中, 需要采样足够多的次数才能对转移概率有相对准确的描述.
不能仅仅因为最近访问过一次状态动作二元组, 就降低其exploration bonus
