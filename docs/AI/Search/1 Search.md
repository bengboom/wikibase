## Search Problems
Search problems are models, mathematical description
- state space
- successor function (action, costs) cost = distance
- a start (initial) state and a goal test
A **solution** is a sequence of actions (a plan) which transforms the start state to a goal state
![[Pasted image 20230920231815.png]]
![[Pasted image 20230920231836.png]]
### Search for?
Assumptions about the world
Planning Algorithms
Identification problems
## Search
### Search Trees
- Expand out potential plans (tree nodes)
- Maintain a fringe (探索集) of partial plans under consideration
- Try to expand as few tree nodes as possible
- Notice: Lots of repeated structure in the search tree!
### General Tree Search
![[Pasted image 20230920233559.png]]
![[Pasted image 20230920233830.png]]
idea: Fringe Expansion Exploration strategy
### Search Algorithm Properties
- Completeness (完备性): Guaranteed to find a solution if one exists?
- Cost optimality (代价最优性): Guaranteed to find the least cost path?
- Time complexity (时间复杂性)
- Space complexity (空间复杂性)
### Depth-First Search
Strategy: expand a deepest node first
Implementation: Fringe is a LIFO stack
If m is finite, takes time $O(b^m)$, space $O(bm)$, prevent cycle so complete(m could be infinite), not optimal, but the leftmost solution.
### Depth-Limited Search
为了避免深度优先搜索陷入无限路径
Space complexity $O(b\cdot m )$
limited depth: $l$
time complexity $O(b^l)$
### Breadth-First Search
$s$ : depth of the shallowest solution
time complexity $O(b^s)$
complete, optimal if costs are all 1
### Iterative Deepening Search
combine DFS BFS
存在解时，时间复杂度为$O(b^s )$；不存在时为$O(b^m )$
- Run a DFS with depth limit 1.  If no solution…
- Run a DFS with depth limit 2.  If no solution…
- Run a DFS with depth limit 3.  …..
最大深度不断加深的多次DFS搜索
Isn’t that wastefully redundant?Generally, most work happens in the lowest level searched, so not so bad!
### Cost sensitive search
BFS没找到最低代价的路线
**-> Uniform-cost search**
Strategy: expand a cheapest node first. Fringe is a priority queue (优先队列)
(优先级: cumulative cost) Dijkstra?
### Uniform cost search
Processes all nodes with cost less than current cheapest solution!
If that solution costs C* and arcs cost at least e , then the “effective depth” is roughly C*/e
Takes time O(bC*/e) (exponential in effective depth)
Space $O(b^{C^*/\epsilon})$
Complete. Optimal. A* Algorithm
**fringe strategies: DFS、BFS、UCS都是一样的，只是每次选择节点的策略不同（fringe不同）**
## Informed Search
### Heuristics（启发式）
A heuristic is:
- A function that estimates **how close a state is to a goal**
- Designed for a particular search problem
- Examples: Manhattan distance, Euclidean distance for pathing
### Greedy（贪心）
Strategy: expand a node that you think is closest to a goal state
Heuristic: estimate of distance to nearest goal for each state
### A* Search
Combine: Uniform-cost search *UCS* and Greedy
- Uniform-cost orders by **path cost**, or backward cost  g(n)
- Greedy orders by **goal proximity**, or forward cost  h(n)
- A* Search orders by the sum: f(n) = g(n) + h(n)
- ![[Pasted image 20230923132337.png]]
stop when **Dequeue** the goal, not when enqueue the goal

**Admissibility** 可容许性
- Inadmissible (pessimistic) heuristics break optimality by trapping good plans on the fringe
- Admissible (optimistic) heuristics slow down bad plans but never outweigh true costs
- A heuristic h is admissible (optimistic) if $$0 \leq h(n) \leq h^*(n)$$where $h^*(n)$ is the true cost to a nearest goal

**Proof of the optimality of A* Search***
![[Pasted image 20230923132942.png]]![[Pasted image 20230923133004.png]]![[Pasted image 20230923133056.png]]![[Pasted image 20230923133108.png]]

**Consistency** 一致性：如果对于每个节点n以及由动作a生成的n的每个后继节点n′有以下条件，则启发式函数h(n)是一致的。一致的启发式函数都是可容许的（Admissible），但反过来不成立。因此，使用一致性启发式函数的A* 搜索都是代价最优的。
![[Pasted image 20230923133230.png]]

**Visualization** 可视化方法：绘制**等值线 Contour**
A* 扩展从初始状态可以到达并且路径上的每个节点都满足$f(n)<C^∗$的所有节点。这些节点被称为必然扩展节点（Surely Expanded Node）
A* 可能会在选出目标节点之前，扩展某些恰好在目标等值线（ $f(n)=C^∗$  ）上的节点
A* 不扩展$f(n)>C^∗$的节点
![[Pasted image 20230923133828.png]]

### Weighted A* Search
对启发式函数加权
cost function $$f(n) \leq g(n) + W \times h(n)$$![[Pasted image 20230923134051.png]]
- Most of the work in solving hard search problems optimally is in coming up with admissible heuristics (可容许的启发式函数)
- Often, admissible heuristics are solutions to relaxed problems, where new actions are available
- Inadmissible heuristics are often useful too: 虽然丧失了最优性，但可能搜索速度更快
![[Pasted image 20230923172658.png]]
### Graph Search
Idea: Never expand a state twice
- Tree search + set of expanded states (“closed set”)
- Expand the search tree node-by-node, but…
- Before expanding a node, check to make sure its state has never been expanded before
- If not new, skip it;  if new, add to closed set
Important: Store the closed set as a set, not a list
Completeness?![[Pasted image 20230923172823.png]]
Optimality?![[Pasted image 20230923172827.png]]

## Adversarial Search
对抗搜索 博弈搜索 Game Search
- 在一个竞争环境（Competitive Environment）中，两个或两个以上的智能体（Agents）之间通过竞争实现相反的利益，一方最大化这个利益，另外一方最小化这个利益。
- ![[Pasted image 20230923172939.png]]

### Types of Games
- Deterministic (确定性的) or stochastic (随机性的)?
- One, two, or more players?
- Zero sum?
- 零和游戏：对一方有利的东西将对另一方同等程度有害，不存在“双赢”结果
- Perfect information (states完全可被观察)?
#### Deterministic Games
- state S (start at $s_0$)
- Players: P={1...N} (usually take turns)
- Actions: A (may depend on player / state)
- Transition Function: $S \times A \rightarrow S$
- Terminal Test: $S \rightarrow \{t, f\}$
- Terminal Utilities: $S \times P \rightarrow R$
- Solution: A policy: $S\rightarrow A$
#### Zero-Sum Games
Agent have opposite utilities(values on outcomes)
Adversarial, pure competition
双人零和博弈：确定性、双人、轮流、完美信息(perfect information)。移动(move) 作为“动作”(action)；局面(position) 作为“状态”(state)![[Pasted image 20230924105553.png]]
#### General Games
Agents have independent utilities
Cooperation, indifference, competition, and more are all possible

### Adversarial Games Trees
![[Pasted image 20230924112917.png]]
### Minimax Search 
极小化极大搜索
- a state-space search tree
- Player alternate turns
- Compute each node’s **minimax value: the best achievable utility against a rational (optimal) adversary**

对简单的博弈问题，可生成整个博弈树，找到必胜的策略

对于复杂的博弈问题，不可能生成整个搜索树。一种可行的方法是用当前正在考察的节点生成一棵部分博弈树，并利用代价函数f(n)对叶节点进行估值。
![[Pasted image 20230924113138.png]]
![[Pasted image 20230924113211.png]]

![[Pasted image 20230924113248.png]]

这种都是Turn based games，暂不涉及decision simultaneously的游戏

**Efficiency** like DFS Time $O(b^m)$ Space $O(bm)$

### Pruning
剪枝 在Minimax算法中可减少被搜索的节点数，即在保证得到与原Minimax算法同样的搜索结果时，剪去了不影响最终结果的搜索分枝
#### Alpha-Beta Pruning
General configuration (MIN version)
- We’re computing the MIN-VALUE at some node n
- We’re looping over n’s children
- n’s estimate of the children’s min is dropping
- Who cares about n’s value?  MAX
- Let a be the best value that MAX can get at any choice point along the current path from the root
- If n becomes worse than a, MAX will avoid it, so we can stop considering n’s other children (it’s already bad enough that it won’t be played)
MAX version is symmetric
![[Pasted image 20230924114125.png]]

**Properties**
- Values of intermediate nodes might be wrong
	- Important: children of the root may have the wrong value
	- So the most naïve version won’t let you do action selection
- Good child ordering improves effectiveness of pruning
- With “perfect ordering”:
	- Time complexity drops to $O(b^{m/2})$ vs. minimax $O(b^m)$
	- Doubles solvable depth!

> This is a simple example of meta-reasoning (computing about what to compute)

### Evaluation Function
cannot search to leaves in realistic condition
-> Depth-limited search
Guarantee of optimal play is gone
method -> **Evaluation Functions**
![[Pasted image 20230924120528.png]]
Deeper search => better play (usually)

### Synergies 协同作用
- Alpha-Beta : amount of pruning depends on expansion ordering
	- Evaluation function can provide guidance to expand most promising nodes first (which later makes it more likely there is already a good alternative on the path to the root)
- Alpha-Beta:  (similar for roles of min-max swapped)
	- Value at a min-node will only keep going down
	- Once value of min-node lower than better option for max along path to root, can prune
	- Hence: IF evaluation function provides upper-bound on value at min-node, and upper-bound already lower than better option for max along path to root  
	- THEN can prune
## Constraint Satisfaction Problems
约束满足问题 CSP
- A special subset of search problems
- State $X_i$ ,value function from domain $D$
- Goal test: set of constraints specifying allowable combinations of values for subsets of variables
示例：Map Coloring 地图染色，、下棋要满足游戏规则、数独Sudoku
### Backtracking Search 回溯搜索
basic uniform algorithm for solving CSPs
- One Variable at a time
- Check constraints as you go
- DFS Search with these two improvements
- **Backtracking = DFS + variable-ordering + fail-on-violation**
![[Pasted image 20230924121738.png]]

more improvements
- Filtering: Keep track of domains for unassigned variables and cross off bad options
- Forward checking: Cross off values that violate a constraint when added to the existing assignment
- Forward checking propagates information from assigned to unassigned variables, but doesn't provide early detection for all failures
- Enforcing consistency of arcs pointing to each new assignment![[Pasted image 20230924122628.png]]
- Runtime: $O(n^2d^3)$, can be reduced to $O(n^2d^2)$
- … but detecting all possible future problems is NP-hard
