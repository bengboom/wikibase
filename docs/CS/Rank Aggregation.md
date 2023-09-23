## Voting Rules

#### Single-Winner Rules: Plurality
each voter names his favorite
highest votes win
if have equal highest score -> tie-breaking rule

### Plurality With Runoff
如果有并列第一，重新全体投票，选项只有并列的
second round vote

### Multi-Round Elections
每次淘汰最末一个，直到有人得到超过一半的票数

### Single transferable vote
Multi-round is more appealing but hard to implement
这个方法把多轮转换为单轮 voters对candidates排序
然后依据顺序，模拟multi-round.假设voter vote the available & favorite candidate.

### Condorcet vote 
*Hard to explain to voters*
**pair-wise winner** 比较两个candidate，一个人在一半以上的投票中排名都超过另一个candidate(more than half voters rank A above B)
**Condorcet winner** 一个人和任意的另一个人比，总是他赢
并不是所有的投票都能投出condorcet winner

#### Condorcet consistent
总是能选出condorcet winner 的投票机制
G: pairwise majority graph, any directed graph with no 2-cycles can arise as a pairwise majority graph.不存在长度为2的环的有向图
对于选不出Condorcet winner的情况
**Copeland** pair-wise win得一分 平得0.5 最后比较分数
**Maximin** score is votes he get in his *worst*() pairwise election, highest win. Winner's Maximin score >n/2, 其他所有人都少于n/2
**Dodgson** 

## Scoring rules
*easy to understand and implement*
*however, not Condorcet-consistent*
**election with m candidate, with scoring vector (s1,s2,...sm)**
**the calculation process seems to need to multiply vectors or matrices**
plurality:(1,0,...,0)
Borda (m-1,m-2,...,2,1,0)
K-approval(1,...1,0,...0), equivalent to: voter只有k票

## Axioms
### Desirable properties of voting rules
Anonymity 平等对待voters
Neutrality  平等对待candidates
Pareto efficiency 如果所有投票者都把a排b前，b不能赢

### Criteria for Single-Winner Elections
**Consistency**
two elections with disjoint sets of vectors, same set of candidates
一个人在两边都赢了，合起来也能赢，仅scoring rules满足
**Monotonicity**
c赢了，一些投票者把c票数移到更高位置，c依然赢（单调）
满足：Pluality copeland maximin borda
不满足：plurality with runoff stv dodgson

### **Winner? Ranking?**
有的方法能决定出赢家，但不能决定候选人顺序Ranking

**Kemeny Rule**
two votes, u v
$$d(u,v)= \# \{(A,B)| A_u>B,B_u>A \} $$
find a ranking minimize total distance $d$ of all votes

### Criteria for Ranking Rules
**Pareto efficiency**
所有人都认为A比B好，最后排名a也应该更靠前
**Monotonicity**
有人将某人的排名抬高，某人的最终排名不能降低
**IIA**
*Independence of irrelevant alternatives*
if a is ranked above b in the current election, and we permute the candidates in each vote without changing the relative order of a and b, then a should be ranked above b in the resulting election

**Dictatorship**
这种方法把上面三个都满足了,however not acceptable

**Arrow's Theorem**
如果一个voting rule 同时满足pareto efficient and IIA，且至少3 candidates
则他是dictatorship

**Gibbard-Satterthwaite Theorem**
至少三个候选人，不是dictatorship的voting rule，**则会有某个decisive voter存在** ![[Pasted image 20230623222613.png]] 如果这个人被操控，则可以影响投票的公平性

## Algorithmic Challenges

**Convert Voting Aggregation to Algorithm Neutrality**

### Complexity of Computing Results
single winner: poly-time
some others: NP-hard
Rankings:
by score: easy
Kemeny: NP-hard

### Complexity of Computing Manipulation
有人修改了自己的投票，计算结果需要的时间复杂度
compute a **manipulative** vote result, assume rule itself is poly-time computable
almost all: poly-time computable
STV: NP-hard

### Deal with Incomplete Ballots
如果一些投票的数据丢了，数据不完整怎么办
-> introduce possible winner -- a candidate who can win under some completion
**Necessary Winner** he wins under all completion
存在获胜概率，成概率论问题，NP-hard usually

## Domain Restrictions
因为太难算了，需要restict the preference domain
method:

### Single-Peaked Preferences
![[Pasted image 20230623172633.png]]
Transitivity
![[Pasted image 20230623224052.png]]
![[Pasted image 20230623224454.png]]

Circumventing Gibbard-Satterthwaite

Medium is Truthful
