## Cake Cutting

### Definition
蛋糕有不同的部分，奶油，蛋糕，水果
每个人喜欢的部分不一样
对同一个蛋糕不同部位有不同的权重
整个蛋糕归一化后，每个人的权重就变成了一个定义域[0,1]的函数
一个人认为他实际分到的是该“喜好函数“在被分到的区域的积分
同时，每个人的喜好函数在0-1的积分也是1，也要归一化
如果一个人兴趣广，对于其中某一部分的喜好程度就不如特别喜欢那一部分的人对同一部分的评分。

### Concepts

**Proportional** An allocation is proportional if each agent values their share as at least $1/n$

**Envy-free** An allocation, if no agent envies another one $V_i(A_i)\geq v_i(A_j)$ for all $i,j$

**Equitable** if all agents have the same value

**Pareto Optimal** no other allocation $(B_1,..B_n)$ such that $V_i(B_i)\geq V_i(A_i)$for each i and 

### Relationship
complete and envy-free -> proportional allocation

### Existence
fair solutions not always exist
can we find them efficiently
- proportional yes, with algorithm
- envy-freeness + completeness yes, but $O(n^{n^{n^n}})$ with n people

### Algorithm

#### Cut and choose
不够公平，因为每个人认为的均等不一样，有不同的喜好。
![[Pasted image 20230628164616.png]]

#### Robertson-Webb 计算模型
![[Pasted image 20230628164854.png]]

#### Dubins-Spanier Moving knife Protocol
从左到右连续移动刀，不断检测每个人的满足度，当有人认为到左边的部分价值1/n时，他拿走那块然后离开。最后一个人拿剩下的。
![[Pasted image 20230628165805.png]]
这个算法满足 **proportional**
推导法each remaining agent values the remaining cake at least $(n-i)/n$
像是贪心的算法，$O(n^2)$

#### Even-Paz Protocol
**Proportionality** $O(nlogn)$
![[Pasted image 20230628170533.png]]
二分查找，一直分下去，直到大于等于1/n
每个人尽可能去“爱好密度”高的一部分

#### Envy-free allocation
Selfridge-Conway protocol




