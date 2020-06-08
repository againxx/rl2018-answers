# Exercises of  Chapter 02
## 2.5
#### Ex2.4 If the step-size parameters, $\alpha_n$, are not constant, then the estimate $Q_n$ is a weighted average of previously received rewards with a weighting different from that given by (2.6). What is the weighting on each prior reward for the general case, analogous to (2.6), in terms of the sequence of step-size parameters?
解:
$$
\begin{aligned}
    Q_{n+1} &= Q_n + \alpha_n[R_n - Q_n] \\
            &= \alpha_n R_n + (1 - \alpha_n) Q_n \\
            &= \alpha_n R_n + (1 - \alpha_n)[\alpha_{n-1} R_{n-1} - (1 - \alpha_{n-1}) Q_{n-1}] \\
            &= \alpha_n R_n + (1 - \alpha_n) \alpha_{n-1} R_{n-1} + (1 - \alpha_n)(1 - \alpha_{n-1}) Q_{n-1} \\
            &= Q_1 \prod_{i=1}^n (1 - \alpha_i) + \sum_{i=1}^n \alpha_i \prod_{j=i+1}^n (1 - \alpha_j) R_i
\end{aligned}
$$

## 2.6
#### Ex2.6 The results shown in Figure 2.3 should be quite reliable because they are averages over 2000 individual, randomly chosen 10-armed bandit tasks. Why, then, are there oscillations and spikes in the early part of the curve for the optimistic method? In other words, what might make this method perform particularly better or worse, on average, on particular early steps?
解: 在step大于10次之后, agent已经完成了一轮所有action的探索, 会持续选择若干次最优策略, 表现出峰值.
之后由于最优策略的Q值下降, 而其他次优策略的Q值仍然接近5, 故agent转而探索次优策略.


#### Ex2.7 Unbiased Constant-Step-Trick: In most of this chapter we have used sample averages to estimate action values because sample averages do not produce the initial bias that constant step sizes do (see the analysis in (2.6)).  However, sample averages are not a completely satisfactory solution because they may perform poorly on non-stationary problems. Is it possible to avoid the bias of constant step sizes while retaining their advantages on non-stationary problems?
One way is to use a step size of
$$ \beta_n \doteq \alpha / \bar{o}_n $$
to process the *n*th reward for a particular action,
where $\alpha > 0$ is a conventional constant step size and $\bar{o}_n$ is a trace of one that start at $0$:
$$ \bar{o}_{n} = \bar{o}_{n-1} + \alpha(1 - \bar{o}_{n-1}), \text{for}\  n \geq 0, \text{with}\  \bar{o}_0 \doteq 0. $$
Carry out an analysis like that in (2.6) to show that $Q_n$ is an exponential recency-weighted average without initial bias.

解:
$$
\begin{aligned}
    Q_{n+1} &= Q_n + \beta_n[R_n - Q_n] \\
            &= \beta_n R_n + (1 - \beta_n) Q_n \\
            &= \beta_n R_n + (1 - \beta_n)[\beta_{n-1} R_{n-1} - (1 - \beta_{n-1}) Q_{n-1}] \\
            &= \beta_n R_n + (1 - \beta_n) \beta_{n-1} R_{n-1} + (1 - \beta_n)(1 - \beta_{n-1}) Q_{n-1} \\
            &= Q_1 \prod_{i=1}^n (1 - \beta_i) + \sum_{i=1}^n \beta_i \prod_{j=i+1}^n (1 - \beta_j) R_i
\end{aligned}
$$

注意到
$$ 1 - \beta_i = 1 - \frac{\alpha}{\bar{o}_i} = \frac{\bar{o}_i - \alpha}{\bar{o}_i} = (1 - \alpha) \frac{\bar{o}_{i-1}}{\bar{o}_i} $$
故
$$
\prod_{i=1}^n (1 - \beta_i) = (1 - \alpha)^n \frac{\bar{o}_{n-1}}{\bar{o}_n} \frac{\bar{o}_{n-2}}{\bar{o}_{n-1}} \cdots \frac{\bar{o}_0}{\bar{o}_1} = 0
$$

$$ \prod_{j=i+1}^n (1 - \beta_j) = (1 - \alpha)^{n-i} \frac{\bar{o}_{i}}{\bar{o}_n} $$

$$
Q_{n+1} = \sum_{i=1}^n \beta_i (1 - \alpha)^{n-i} \frac{\bar{o}_i}{\bar{o}_n} R_i = \beta_n \sum_{i=1}^n (1-\alpha)^{n-i} R_i
$$

到$\alpha < 1$时, $R_i$的系数明显是衰减的
