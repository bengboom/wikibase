MDP Offline 计算完之后得到决策
RL Online 一边试错一边进化
$\hat{T}$ 状态转移概率 一个模型

强化学习：通过与环境的交互学习，不需要知道环境的动态（即状态转换概率）。Assume a MDP ，$<State, Action, TransProb, Reward>$, looking for $\pi(s)$, 不知道转换概率和奖励，要真正去了才能感受到奖励。

## 分类
### model based?
- Model-Based 
	- 更简单
	- 与MDP接近
	- Learn an approximate model based on experiences
	- Policy/Value
	- Process
		- step1: learn empirical MDP model
			- count outcome $s'$ for each $s,a$
			- normalize, get $\hat T (s,a,s')$
			- Discover $R(s,a,s')$ when experience
		- step 2 : solve MDP
- Model-Free
	- 通过采样的方式
	- Passive
		- Online!
		- Direct Evaluation
			- compute state value under $\pi$
			- **Average** observed values according to $\pi$
				- ![[Screenshot 2023-12-07 at 20.24.09.png]]
			- Pros
				- easy to understand, do not require T,R
			- Cons
				- waste info about state connection
				- time consuming
			- TDL *Temporal Differential Learning*
				- ![[Pasted image 20231207202536.png]]
				- Exponential Moving Average, make recent samples more important
					- ![[Pasted image 20231207202554.png]]
		- Indirect Evaluation
	- Active
		- Q-learning
### on policy?
- On-policy
	- 这种方法中，学习和决策是基于当前策略执行的。换句话说，它使用当前的策略来决定下一步的动作，并基于这些动作来更新策略本身。
	    - **示例算法**：SARSA（State-Action-Reward-State-Action）是一个典型的On-policy算法。在SARSA中，学习过程依赖于基于当前策略采取的行动。
- Off-policy
	- 这种方法允许策略的学习和评估与用于生成行为的策略分离。这意味着，即使是从其他策略中获得的数据，也可以用来改进当前的决策策略。
	- **示例算法**：Q学习是一个经典的Off-policy算法。在Q学习中，策略的更新是基于最大化未来奖励的行动，而不一定是遵循当前策略的行动。

### value/policy based?
- Value-based vs Policy-based
![[Pasted image 20231206214018.png]]


## Q-learning
 Value based, Off policy
它通过样本来迭代更新状态-动作对的价值（Q值）。
  $$
  Q_{k+1}(s, a) \leftarrow \sum_{s'} T(s, a, s') \left[ R(s, a, s') + \gamma \max_{a'} Q_k(s', a') \right]
  $$
其中：
- $Q_{k+1}(s, a)$ 是状态 $s$ 下动作 $a$ 在第 $k+1$ 轮迭代的Q值。
- $T(s, a, s')$ 是转移概率，表示在状态 $s$ 执行动作 $a$ 后转移到状态 $s'$ 的概率。
- $R(s, a, s')$ 是即时奖励。
- $\gamma$ 是折扣因子，表示未来奖励的当前价值。
- $\max_{a'} Q_k(s', a')$ 是下一个状态 $s'$ 所有可能动作 $a'$ 的最大Q值。

- **学习过程**：
  在实际应用中，由于不直接知道转移概率和奖励函数，我们通过观察到的样本来更新Q值：
  $$
  Q(s, a) \leftarrow (1 - \alpha) Q(s, a) + \alpha \left[ \text{sample} \right]
  $$
  样本 $\text{sample}$ 的计算公式为：

  $$
  \text{sample} = R(s, a, s') + \gamma \max_{a'} Q(s', a')
  $$

  - $\alpha$ 是学习率，用于控制新旧信息的权重。
  - 这种更新方法也被称为不再是策略评估，因为它不依赖于固定策略下的价值估计，而是尝试估计在任何可能策略下的预期回报。


### Exploit vs. Explore
开发与探索的取舍
开发：做出当前信息下的最佳决定
探索：做以前从来没有做过的事情，期望获得最高的回报

### Explore

#### Greedy
- Simplest--Random
	- $\epsilon$-greedy
		- every step, random
			- With (small) probability e, act randomly
			- With (large) probability 1-e, act on current policy
	- However, we should lower $\epsilon$ over time, or using exploration functions

#### Exploration Functions
收敛更快

- 探索函数通过提供一个乐观的状态-动作对值估计，鼓励智能体探索较少访问的状态。
- 公式为：
  $$
  f(u, n) = u + \frac{k}{n}
  $$
  其中：
  - `u` 表示状态-动作对的当前估计值。
  - `n` 表示该状态-动作对被访问的次数。
  - `k` 是一个常数，用于调整乐观估计的程度。

##### 带探索函数的Q学习更新规则
- **常规Q值更新**：
  - 不考虑探索的标准更新规则：
    $$Q(s, a) \leftarrow R(s, a, s') + \gamma \max_{a'} Q(s', a')$$
  其中：
  - $Q(s, a)$ 是当前状态 $s$ 下动作 $a$ 的Q值。
  - $R(s, a, s')$ 是在状态 $s$ 下执行动作 $a$ 并转移到新状态 $s'$ 后获得的即时奖励。
  - $\gamma$ 是未来奖励的折扣因子。
  - $\max_{a'} Q(s', a')$ 是下一个状态 $s'$ 所有可能动作 $a'$ 的最大Q值。

- **带探索的修改Q值更新**：
  - 结合探索函数来平衡探索与利用
    $$
    Q(s, a) \leftarrow R(s, a, s') + \gamma \max_{a'} f(Q(s', a'), N(s', a'))
    $$
  其中：
  - $f(Q(s', a'), N(s', a'))$ 是将探索函数应用于下一个状态-动作对 $(s', a')$ 的Q值和访问次数 $N(s', a')$。
  - **这个修改后的规则通过探索函数为少访问的状态-动作对分配更高的值，从而鼓励智能体更频繁地选择这些对。**

### Regret
越小越好，在学习最优策略的过程中也会犯错
Regret: total mistake cost: difference between expected rewards and optimal rewards.
要求比optimal learning更高

### Generalizing
- Learn about some small number of training states from experience
- Generalize that experience to new, similar situations
- This is **a fundamental idea in machine learning**

### Approximate Q-learning (GPT4)
用feature描述状态
Using a feature representation, we can write a q function (or value function) for any state using a few weights:  

由于状态-动作空间可能非常庞大，直接计算和存储每个状态-动作对的Q值变得不切实际。近似Q学习通过特征函数来表示Q值，从而减少需要估计的参数数量。
- **Q值的近似表示**：
  近似Q值可以表示为特征向量和权重向量的乘积：
  $$
  Q(s, a; \theta) \approx \theta^T \cdot \phi(s, a)
  $$
  其中：
  - $\theta$ 是权重向量。
  - $\phi(s, a)$ 是特征向量，它描述了状态 $s$ 和动作 $a$ 的特征。
- **更新规则**：
  权重向量 $\theta$ 的更新依赖于预测误差，通常采用梯度下降法来进行更新：
  $$
  \theta_{k+1} = \theta_k + \alpha \cdot \left[ r + \gamma \max_{a'} Q(s', a'; \theta_k) - Q(s, a; \theta_k) \right] \cdot \nabla_{\theta} Q(s, a; \theta_k)
  $$
  其中：
  - $\alpha$ 是学习率。
  - $r$ 是即时奖励。
  - $\gamma$ 是折扣因子。
  - $\nabla_{\theta} Q(s, a; \theta_k)$ 是关于权重的Q值的梯度。
#### Optimization
用回归的方法去拟合上面的函数
Least Squares