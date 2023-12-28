> AIGC *AI Generated Content*
## 凸集合与凸函数
- **凸集合(Convex Sets)**: 集合中任意两点之间的线段仍在集合内，则该集合为凸集合。
  - **性质**: 对于任意的$x,y \in C$和$\theta \in [0,1]$，有$\theta x + (1-\theta)y \in C$。![[Pasted image 20231208231702.png]]

- **凸函数(Convex Functions)**: 定义在凸集合上的函数，对于任意$x,y \in C$和$\theta \in [0,1]$，满足$f(\theta x + (1-\theta)y) \leq \theta f(x) + (1-\theta)f(y)$，则$f$为凸函数。
- 凸函数在任何点处的切线都位于函数的下方

## 多个凸集的交集是凸集

- **概念**：多个凸集合的交集仍然是凸集合。
- **证明**：设有凸集合$C_1, ..., C_k$，它们的交集定义为$C = \bigcap_{i=1}^{k}C_i$。对于任意$x,y \in C$，且对于任意$\theta \in [0,1]$，必须证明$\theta x + (1-\theta)y \in C$，即这个点也在所有凸集合$C_i$中。
- **证明过程**：因为$x,y \in C$，则$x,y$同时属于所有的$C_i$。由于每个$C_i$都是凸集合，所以对于任意$i \in \{1, ..., k\}$，都有$\theta x + (1-\theta)y \in C_i$。因此，$\theta x + (1-\theta)y$也在它们的交集$C$中，所以$C$是凸集合。
- **应用**：在优化问题中使用多个凸集合作为约束，可以保证得到的解空间也是凸的，这在理论上和计算上都是有利的，因为凸问题通常更容易求解，并且局部最优解也是全局最优解。

## 线性约束
- **等式约束**: 形如$Ax = b$的约束。
- **多面体约束**: 形如$Ax \leq b$的约束。

## 最优性条件和判定方法
![[Pasted image 20231208232138.png]]
- **一阶必要条件**: 函数在最优点的梯度为零。
- **二阶必要条件**: 函数在最优点的Hessian矩阵半正定。

### 例子
- $f(x, y) = x + y$，Hessian矩阵$H = \begin{bmatrix}0 & 0\\0 & 0\end{bmatrix}$，线性函数。
- $f(x, y) = x^2 + y^2$，Hessian矩阵$H = \begin{bmatrix}2 & 0\\0 & 2\end{bmatrix}$，正定矩阵，凸函数。
- $f(x, y, z) = 2x^2 - xy + y^2 - 3z^2$，Hessian矩阵$H = \begin{bmatrix}4 & -1 & 0\\-1 & 2 & 0\\0 & 0 & -6\end{bmatrix}$，非全局凸函数。

## 凸优化问题
## 凸优化问题的标准形式
- 凸优化问题的目标是找到一个最小化目标函数的解，同时满足一系列约束条件。
  - **目标函数**: $\min f(x), x \in C$
  - **约束条件**: 约束集合$C$通常由等式和不等式约束组成。

- **标准问题形式**:
- $min f(x), x$
- $s.t. h_i(x) = 0, i = 1, ..., k$
- $g_i(x) \leq 0, i = 1, ..., l$

其中$h_i(x)$表示等式约束，$g_i(x)$表示不等式约束。
### 局部最优就是全局最优
- **全局最优性**: 凸优化问题中，任何局部最优解也是全局最优解。
- **全局最优解**: $f(x^*) \leq f(x)$，对于所有$x$在约束集合$C$内。
- **局部最优解的性质**:
- **足够局部最优解**：在$x^*$的$\delta$-邻域内（即所有$x$满足$\|x - x^*\| \leq \delta$），有$f(x^*) \leq f(x)$。
- ![[Pasted image 20231208231917.png]]
- **证明**: 凸优化问题满足，若$f(x^*) \leq f(x)$对所有$x \in C$成立，则$x^*$为全局最优解。

### 具体的优化任务

#### 线性回归 (Linear Regression, LR)
- **目标函数**: $f(x) = w^Tx + b$
- **损失函数**: $L = \frac{1}{2n} \sum_{i=1}^{n}(f(x_i) - y_i)^2$，其中$(x_i, y_i)$是训练样本。
- **优化问题**: $\min \frac{1}{2n} \sum_{i=1}^{n}(w^Tx_i + b - y_i)^2$
- **梯度**: $\frac{\partial^2L}{\partial w_i \partial w_j} = \frac{1}{n} \sum_{k=1}^{n}x_{i,k}x_{j,k}$
- **Hessian矩阵**: $H = \frac{1}{n}X^TX$，其中$X$为设计矩阵。
- **最优性条件**: $X^TX$为半正定矩阵，确保优化问题的凸性。

#### 岭回归 (Ridge Regression)
- **目标函数**: $\min_w \sum_{i=1}^{n}(w^Tx_i - y_i)^2 + \lambda ||w||^2$
- **正则化项**: $\lambda ||w||^2$用于防止过拟合。

#### 支持向量机 (Support Vector Machine, SVM)
- **目标函数**: $\min_w \frac{1}{2} ||w||^2 + C \sum_{i=1}^{l}\xi_i$
- **约束**: $y_i(w^Tx_i + b) \geq 1 - \xi_i$

#### 逻辑回归 (Logistic Regression)
- **目标函数**: $\min_w \frac{1}{2}w^Tw + C \sum_{i=1}^{l}\log(1 + e^{-y_iw^Tx_i})$


