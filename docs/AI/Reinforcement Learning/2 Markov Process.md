**强化学习的基础、本质**

1900s 马尔科夫链 数学
1950s 马尔科夫决策过程MDP经济学 管理学

## Basic Concepts
MDP, Markov decision process, also called Markov Chain

MRP, Markov Reward Process, a Markov chain with values
**MRP is a tuple $<S,P,R,\gamma>$** 没有明确决策过程（无action）

- R is reward function $R_s=E[R_{t+1}| S_t=s]$
- $\gamma$ is discount factor 衰减因子 $\gamma \in [0,1]$

MP $<S,P>$
**MDP $<S,A,P,R,\gamma>$**
- **组成部分：** MDP由状态（States）、动作（Actions）、转移概率（Transition Probabilities）、和奖励（Rewards）组成。
- **马尔可夫性质：** MDP假设“马尔可夫性质”，即下一个状态只依赖于当前状态和动作，而与之前的历史状态或动作无关。

![[Pasted image 20231206204202.png]]
## Reward Function
Reward 总体目标函数 The agent's **sole objective**: maximize **the total reward** it receives over the long run

### 累计收益 Long Time Return Reward
![[Screenshot 2023-10-10 at 20.36.21.png]]
### Discount
为什么要discount？
数学上适应、防止无穷的环状现象
- It’s reasonable to maximize the sum of rewards
- It’s also reasonable to prefer rewards now to rewards later
- One solution: **values of rewards decay exponentially**
- Smaller $\gamma$ means smaller “horizon” – shorter term focus

## Student MRP Value Function
Value Function
state value function $v(s)$ of an MRP is the expected return starting from state $s$
$v(s)=E[G_t|S_t=s]$
- a prediction of future reward
- evaluate the goodness/badness of states

### Reward vs. Value
Reward：signal that is good in an **immediate** sense
Value: signal that is good in the **long run**
状态的奖励值可以立刻得到，而value可以计算这个状态的更global的收益
- We seek actions that bring about states of **highest value, not highest reward,** because these actions obtain the greatest amount of reward for us **over the long run**

## Bellman Equation

### Definition
The Bellman Equation provides a recursive decomposition for the value function of a policy. For any MRP or MDP, it expresses the fact that the value of the starting state is the expected return for the first step plus the discounted value of the subsequent state.
累积未来奖励 \( G_t \)，从时间步 \( t \) 开始，给出如下：
$$
G_t = \sum_{k=0}^{\infty} \gamma^k R_{t+k+1}
$$

对于MRP，状态价值函数 v(s)  定义为从状态  s 开始的期望回报：
$$
v(s) = \mathbb{E}[G_t | S_t = s]
$$

对于MDP，策略 $\pi$ 下的状态价值函数 $v_{\pi}(s)$ 和动作价值函数 \( q_{\pi}(s, a) \) 定义如下：
$$
v_{\pi}(s) = \mathbb{E}_{\pi}[R_{t+1} + \gamma v_{\pi}(S_{t+1}) | S_t = s]
$$
$$
q_{\pi}(s, a) = \mathbb{E}_{\pi}[R_{t+1} + \gamma q_{\pi}(S_{t+1}, A_{t+1}) | S_t = s, A_t = a]
$$

MRP中的状态价值函数的贝尔曼方程是：
$$
v(s) = R_s + \gamma \sum_{s' \in \mathcal{S}} P_{ss'} v(s')
$$

### for MRPs
没有行动的概念，状态的转移仅由当前状态和转移概率决定。因此，MRP中的贝尔曼方程只需考虑立即奖励和下一个状态的预期价值。
- The value of state $s$ can be decomposed into two parts
- Immediate reward $R_{t+1}$
- Discounted value of successor state $\gamma v\left(S_{t+1}\right)$
$$
\begin{aligned}
v(s) & =\mathbb{E}\left[G_t \mid S_t=s\right] \\
& =\mathbb{E}\left[R_{t+1}+\gamma R_{t+2}+\gamma^2 R_{t+3}+\ldots \mid S_t=s\right] \\
& =\mathbb{E}\left[R_{t+1}+\gamma\left(R_{t+2}+\gamma R_{t+3}+\ldots\right)\left|S_t=\right| s\right] \\
& =\mathbb{E}\left[R_{t+1}+\gamma G_{t+1} \mid S_t=s\right] \\
& =\mathbb{E}\left[R_{t+1}+\gamma v\left(S_{t+1}\right) \mid S_t=s\right] \\
v(s) & =\mathcal{R}_s+\gamma \sum_{s^{\prime} \in \mathcal{S}} \mathcal{P}_{s s^{\prime}} v\left(s^{\prime}\right)
\end{aligned}
$$
### For MDPs
系统引入了智能体的决策行为。智能体可以选择不同的行动，每个行动都可能导致不同的后续状态和奖励。因此，MDP中的贝尔曼方程需要包含智能体的策略，策略定义了在给定状态下选择各种可能行动的概率，这导致了**状态价值函数V和动作价值函数Q的引入。**
- The value function $V$ for MDPs includes the decision-making process by considering the best action to take.
	- V table
		- ![[Pasted image 20231206212558.png]]
- The action-value function $Q$ relates to the value of taking an action $a$ in a state $s$ under a policy $π$.
	- Q table
		- ![[Pasted image 20231206212708.png]]
$$\begin{aligned}
v_{\pi}(s) &= \mathbb{E}_{\pi}[G_t | S_t = s] \\
&= \mathbb{E}_{\pi}[R_{t+1} + \gamma v_{\pi}(S_{t+1}) | S_t = s] \\
q_{\pi}(s, a) &= \mathbb{E}[R_{t+1} + \gamma q_{\pi}(S_{t+1}, A_{t+1}) | S_t = s, A_t = a]
\end{aligned}
$$
### Complexity
在状态很多的时候 路径无限多
$$\begin{align*}
v &= R + \gamma Pv \\
(I - \gamma P)v &= R \\
v &= (I - \gamma P)^{-1}R
\end{align*}
$$
- Computational complexity is $O(n^3)$ for n states
- Direct solution only possible for small MRPs
- There are many **iterative methods** for large MRPs
	- Dynamic programming
	- Monte-Carlo evaluation
	- Temporal-Difference learning

## Policy
Policy: 环境的感知到行动的映射
policy $\pi$ is a distribution over actions given states
Policy **Fully defines** the behavior of an agent
$$ \pi(a|s)=P[A_t=a|S_s=s]$$
- Deterministic Policy
	- $a = \pi(s)$
- Stochastic policy
	- $\pi(a|s)=P[A_t=a|S_s=s]$

### Optimal Policy
An optimal policy, denoted as $\pi^*$, is a policy that yields the **highest expected return** from all states, regardless of the initial state. 
Formally, an optimal policy **maximizes the state-value function** for all states.
$$
\pi^* = \arg\max_\pi V_\pi(s) \quad \text{for all } s \in \mathcal{S}
$$
⁡argmax 是一个数学术语，它表示“使得给定表达式取得最大值的参数”
This policy leads to the optimal value functions, defined as:
$$
V^*(s) = \max_\pi V_\pi(s) \quad \text{for all } s \in \mathcal{S}
$$
$$
Q^*(s, a) = \max_\pi Q_\pi(s, a) \quad \text{for all } s \in \mathcal{S}, a \in \mathcal{A}
$$

### Optimality
### Define a Partial Ordering Over Policies

We define a partial ordering over policies where a policy \( \pi \) is considered better than or equal to policy \( \pi' \) if the expected return from \( \pi \) is greater than or equal to \( \pi' \) for all states. Formally:

$$
\pi \geq \pi' \text{ if } V_\pi(s) \geq V_{\pi'}(s), \forall s
$$

### Theorem
For any Markov Decision Process:
- There exists an optimal policy $\pi^*$ that is better than or equal to all other policies, denoted as:
$$
\pi^* \geq \pi, \forall \pi
$$
- All optimal policies achieve the optimal value function, represented as:
$$
V_{\pi^*}(s) = V^*(s)
$$
- All optimal policies achieve the optimal action-value function, represented as:
$$
Q_{\pi^*}(s, a) = Q^*(s, a)
$$
## Iteration Methods
Iteration methods are used to **find the optimal policy** by iteratively improving the estimate of the value functions.
### Value Iteration
Value iteration is a method of computing the optimal policy by iteratively updating the value function:
$$
V_{k+1}(s) = \max_a \sum_{s'} P(s' | s, a) [R(s, a, s') + \gamma V_k(s')]
$$
**start with final rewards** and work backwards
并发，所有状态同时开始迭代，同步更新
**Problems**
- slow $O(S^2A)$ per iteration
- The “max” at each state rarely changes
- policy converges long before the values
### Policy Iteration
Two main steps 
![[Pasted image 20231206213707.png]]
1. **Policy Evaluation**:
   - For a given policy $\pi$, the value function is calculated until it converges by using the Bellman expectation equation:$$V^\pi(s) = \sum_a \pi(a|s) \sum_{s'} P(s' | s, a) [R(s, a, s') + \gamma V^\pi(s')]$$
2. **Policy Improvement**:
   - The policy is then improved by acting greedily with respect to the current value function:$$   \pi'(s) = \arg\max_a \sum_{s'} P(s' | s, a) [R(s, a, s') + \gamma V^\pi(s')]$$
### Comparison
both compute the optimal
Policy Iteration can converge (much) faster under some conditions
Value Iteration: 只更新值，策略只是隐式更新，不跟踪policy
Policy Iteration：时刻利用已学到的值进行决策，更快是因为每次更新只会考虑一种目前已学到的最优的action，而不是所有action