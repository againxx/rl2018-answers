# Exercises of  Chapter 10
## 10.2
#### Ex10.1 We have not explicitly considered or given pseudocode for any Monte Carlo methods or in this chapter. What would they be like? Why is it reasonable not to give pseudocode for them? How would they perform on the Mountain Car task?
解:
* 对于MC方法, 每一幕直到结束时才开始更新, 回报由$G_{t:t+n}$变为$G_t$
* 在高山行车问题中, 由于一幕可能需要很多步才能完成, MC方法学习效率很低, 甚至有可能完全没有机会学习(第一幕一直没有结束)

#### Ex10.2 Give pseudocode for semi-gradient one-step Expected Sarsa for control.
解: n步期望Sarsa (公式7.7)
$$
G_{t:t+n} = \sum_{k=t}^{t+n-1} \gamma^{k-t} R_{k+1} + \gamma^n \sum_a \pi(a|S_{t+n})\hat{q}(S_{t+n}, a, w_{t+n-1})
$$

#### Ex10.3 Why do the results shown in Figure 10.4 have higher standard errors at large n than at small n?
解: n越大, 在等待学习的n步之中, 就越有可能收集到各种极端的经验, 从而增加方差

## 10.3
#### Ex10.4 Give pseudocode for a differential version of semi-gradient Q-learning.
解:
1. 通过$\hat{q}(S,.,w)$选取$A$ (如通过$\epsilon$-贪心策略)
2. 采取动作$A$, 观察$R, S'$
3. $\delta \leftarrow R - \bar{R} + \max\limits_a \hat{q}(S', a, w) - \hat{q}(S, A, w)$
4. $\bar{R} \leftarrow \bar{R} + \beta\delta$
5. $w \leftarrow w + \alpha\delta \nabla\hat{q}(S, A, w)$
6. $S \leftarrow S'$

#### Ex10.5 What equations are needed (beyond 10.10) to specify the differential version of TD(0)?
解: 需要半梯度更新公式
$$ w_{t+1} = w_t + \alpha \delta_t \nabla \hat{v}(S_t, w_t)$$
其中,
$$ \delta_t = R_{t+1} - \bar{R}_{t+1} + \hat{v}(S_{t+1}, w_t) - \hat{v}(S_t, w_t) $$

#### Ex10.6 Consider a Markov reward process consisting of a ring of three states A, B, and C, with state transitions going deterministically around the ring. A reward of 1 is received upon arrival in A and otherwise the reward is 0. What are the differential values of the three states?
解: 平均收益$\bar{R} = \frac{1}{3}$, 根据差分价值函数的Bellman Equation
$$
\left\{
\begin{aligned}
    V(A) &= 0 - \frac{1}{3} + V(B) \\
    V(B) &= 0 - \frac{1}{3} + V(C) \\
    V(C) &= 1 - \frac{1}{3} + V(A)
\end{aligned}
\right.
$$

注意到, 上述方程组线性相关, 没有唯一解, 重新考察$V(A)$的定义
$$ V(A) = \sum_t (a_t - \bar{R}) $$

其中$a_t = \delta\left\{t + 1 \equiv 0 (mod 3)\right\}$, 这个定义并不收敛, 所以加上折扣项
$$ V(A; \gamma) = \sum_t \gamma^t (a_t - \bar{R}) $$

则
$$ \lim_{\gamma \to 1} V(A; \gamma) = V(A) $$

下面对$V(A; \gamma)$进行变形
$$
\begin{aligned}
    V(A; \gamma) &= -\frac{1}{3} - \frac{1}{3} \gamma + \frac{2}{3} \gamma^2 + \sum_{t=3} \gamma^t (a_t - \bar{R}) \\
                 &= -\frac{1}{3} - \frac{1}{3} \gamma + \frac{2}{3} \gamma^2 + \gamma^3 \sum_{t=0} \gamma^t (a_t - \bar{R}) \\
                 &= -\frac{1}{3} - \frac{1}{3} \gamma + \frac{2}{3} \gamma^2 + \gamma^3 V(A; \gamma) \\
\end{aligned}
$$
$$
\begin{aligned}
    &\implies V(A; \gamma) = -\frac{1 + \gamma - 2\gamma^2}{3(1 - \gamma^3)} = -\frac{2\gamma + 1}{3(\gamma^2 + \gamma + 1)} \\
    &\implies \lim_{\gamma \to 1} V(A; \gamma) = \lim_{\gamma \to 1} -\frac{2\gamma + 1}{3(\gamma^2 + \gamma + 1)} = -\frac{1}{3}
\end{aligned}
$$

得到$V(A) = -\frac{1}{3}$, 从而$V(B) = 0, V(C) = \frac{1}{3}$

#### Ex10.7 Suppose there is an MDP that under any policy produces the deterministic sequence of rewards 1, 0, 1, 0, 1, 0, . . . going on forever.
Technically, this is not allowed because it violates ergodicity; there is no stationary limiting distribution $\mu_\pi$
and the limit (10.7) does not exist. Nevertheless, the average reward (10.6) is well defined; What is it?
Now consider two states in this MDP. From A, the reward sequence is exactly as described above, starting with a 1, whereas, from B,
the reward sequence starts with a 0 and then continues with 1, 0, 1, 0, . . . The differential return (10.9) is not well defined
for this case as the limit does not exist. To repair this, one could alternately define the value of a state as
$$
v_\pi(s) = \lim_{\gamma \to 1} \lim_{h \to \infty} \sum_{t=0}^h \gamma^t(\mathbb{E}_\pi [R_{t+1} | S_0 = s] - r(\pi)).
$$
Under this definition, what are the values of states A and B?

解: 首先说明 (10.7) 的极限确实不存在
$$
\lim_{t \to \infty} \mathbb{E}[R_t | S_0, A_{0:t-1} \sim \pi] = \lim_{t \to \infty}\left\{ 1\ \text{or}\ 0 \right\} \quad \text{doesn't exist}
$$
而
$$
r(\pi) = \lim_{h \to \infty} \frac{1}{h} \sum_{t=1}^h \mathbb{E} [R_t | S_0, A_{0:t-1} \sim \pi]
= \lim_{t \to \infty} \left\{ \frac{1}{2} \ \text{or} \ \frac{h+1}{2h} \right\} = \frac{1}{2}
$$

下面计算A和B的状态价值
$$
\begin{aligned}
    &v_\pi(A;\gamma) = \frac{1}{2} + \gamma v_\pi(B;\gamma) \\
    &v_\pi(B;\gamma) = -\frac{1}{2} + \gamma v_\pi(A;\gamma) \\
    &\implies v_\pi(A;\gamma) = \frac{1}{2} (1 - \gamma) + \gamma^2 v_\pi(A;\gamma)  \\
    &\implies v_\pi(A;\gamma) = \frac{1}{2(1 + \gamma)}  \\
    &\implies v_\pi(A) = \lim_{\gamma \to 1} v_\pi(A;\gamma) = \frac{1}{4} \\
    &\implies v_\pi(B) = \lim_{\gamma \to 1} (-\frac{1}{2} + \gamma v_\pi(A;\gamma)) = -\frac{1}{2} + v_\pi(A) =  -\frac{1}{4} 
\end{aligned}
$$

#### Ex10.8 The pseudocode in the box on page 251 updates $\bar{R}_{t+1}$ using $\delta_t$ as an error rather than simply $R_{t+1} - \bar{R}_{t+1}$. Both errors work, but using $\delta_t$ is better. To see why, consider the ring MRP of three states from Exercise 10.6. The estimate of the average reward should tend towards its true value of $\frac{1}{3}$. Suppose it was already there and was held stuck there. What would the sequence of $R_{t+1} − \bar{R}_{t+1}$ errors be? What would the sequence of $\delta_t$ errors be (using (10.10))? Which error sequence would produce a more stable estimate of the average reward if the estimate were allowed to change in response to the errors? Why?

解: 从A开始的误差$R_{t+1} - \bar{R}_{t+1}$
$$ -\frac{1}{3}, -\frac{1}{3}, \frac{2}{3}, -\frac{1}{3}, -\frac{1}{3}, \frac{2}{3}, \cdots $$

从A开始的误差$\delta_t$
$$ 0, 0, 0, 0, 0, 0, \cdots $$

很明显使用$\delta_t$会产生更稳定的估计

#### Ex10.9 In the differential semi-gradient n-step Sarsa algorithm, the step-size parameter on the average reward, $\beta$, needs to be quite small so that $\bar{R}$ becomes a good long-term estimate of the average reward. Unfortunately, $\bar{R}$ will then be biased by its initial value for many steps, which may make learning inefficient. Alternatively, one could use a sample average of the observed rewards for $\bar{R}$. That would initially adapt rapidly but in the long run would also adapt slowly. As the policy slowly changed, $\bar{R}$ would also change; the potential for such long-term non-stationarity makes sample-average methods ill-suited. In fact, the step-size parameter on the average reward is a perfect place to use the unbiased constant-step-size trick from Exercise 2.7. Describe the specific changes needed to the boxed algorithm for differential semi-gradient n-step Sarsa to use this trick.
解: 增加一个变量$u$, 初始化其为0, 每一次迭代令
$$ u \leftarrow u + \beta (1 - u) $$
$$ \bar{R} \leftarrow \bar{R} + \frac{\beta}{u} \delta $$
