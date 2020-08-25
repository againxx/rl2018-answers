# Exercises of  Chapter 08
## 8.2
#### Ex8.1 The nonplanning method looks particularly poor in Figure 8.3 because it is a one-step method; a method using multi-step bootstrapping would do better. Do you think one of the multi-step bootstrapping methods from Chapter 7 could do as well as the Dyna method? Explain why or why not.
解: 还是没有Dyna快, n-step方法虽然一次可以更新n个状态的Q值, 但是Dyna算法可以利用所有过去的经验不断将更新后的Q值回溯更新到更之前的状态,
所以一幕结束时得到更新的状态数量远多于n-step方法, 所以在环境模型准确时, Dyna更优一些.
