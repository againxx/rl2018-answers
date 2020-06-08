# Exercises of  Chapter 06

## 6.1
#### Ex6.1 If V changes during the episode, then (6.6) only holds approximately; what would the difference be between the two sides? Let $V_t$ denote the array of state values used at time $t$ in the TD error (6.5) and in the TD update (6.2). Redo the derivation above to determine the additional amount that must be added to the sum of TD errors in order to equal the Monte Carlo error.
解: 更新$\delta_t$的表示为
$$ \delta_t \doteq R_{t+1} + \gamma V_t(S_{t+1}) - V_t(S_t) $$
之后
$$
\begin{aligned}
    G_t - V_t(S_t) &= R_{t+1} + \gamma G_{t+1} - V_t(S_t) + \gamma V_t(S_{t+1}) - \gamma V_t(S_{t+1}) - \gamma V_{t+1}(S_{t+1}) \\
                   &+ \gamma V_{t+1}(S_{t+1}) \\
                   &= \delta_t + \gamma(G_{t+1} - V_{t+1}(S_{t+1})) + \gamma(V_{t+1}(S_{t+1}) - V_t(S_{t+1})) \\
                   &= \delta_t + \gamma \delta_t + \gamma^2 (G_{t+2} - V_{t+2}(S_{t+2})) + \gamma(V_{t+1}(S_{t+1}) - V_t(S_{t+1})) \\
                   &+ \gamma^2 (V_{t+2}(S_{t+2}) - V_{t+1}(S_{t+2})) \\
                   &= \sum_{k=t}^{T-1} \gamma^{k-t} \delta_k + \sum_{k=t}^{T-2} \gamma^{k-t+1}(V_{k+1}(S_{k+1}) - V_{k}(S_{k+1}))
\end{aligned}
$$
注意, 对于TD(0), 若$S_{k+1} = S_k$ 则 $V_{k+1}(S_{k+1}) \neq V_k(S_{k+1})$, 否则$V_{k+1}(S_{k+1}) = V_k(S_{k+1})$
