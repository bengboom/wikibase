# Probability

## Key Words

1. Experiment : 实验 （丢五次硬币）
2. Event : ***E*** 事件 实验的可能结果 （三次正面朝上) 样本空间子集
3. Sample Space : ***S*** 样本空间 实验的所有可能结果的集合  需要是exhaustive，全面的。
4. Finate Sample Space : 有限样本空间
$$S={s_1,s_2,...,s_n}$$
$$p_i>0 \ for\ i =1,2,...n$$
$$\sum_{i=1}^np_i=1$$
5. Simple Sample Spaces: 简单样本空间 classical Probability,每件事情概率相等,均为1/n
6. Outcome : ***s*** 样本空间中的一个元素
$$ s \in S , s \in E , E \subset S $$
7. Containment包含
    A is contained in B, B contains A
    $$A \subset B$$
    $$A \subset B,B\subset A \iff A=B$$
8. Empty Set
    $$\emptyset$$
9. Universal Set
    $$\Omega$$
10. Complement 补集
    $$A^c = \Omega \backslash A$$
    $$A^c = \{s \in \Omega | s \notin A \}$$
    $$(A^c)^c=A,(\emptyset)^c=\Omega $$
11. Finite and Infinite Sets
    $$\mathbb{N}=\{1,2,3,...\}$$
    $$\mathbb{Z}=\{...,-2,-1,0,1,2,...\}$$
    $$\mathbb{Q}=\{q\in\mathbb{R}|q=\frac{p}{q}\}$$
    $$\mathbb{R}$$
    $$\mathbb{C}$$
12. Countable and Uncountable Sets

    $\mathbb{N},\mathbb{Z},prime$ : countable , Infinite

    Uncountble: neither finite nor countable

    $\mathbb{R},\mathbb{C}$ : Uncountable

    可数与不可数是互斥的。
    既不是可数也不是有限的就是不可数。
13. Union of two Sets
    $$A \cup B = \{s \in \Omega | s \in A \ or \ s \in B \}$$
    $$ A \cup B = B \cup A$$
    $$ A \cup \emptyset = A$$
    $$ A \cup A = A$$
14. Intersection
    $$ A \cap B = \{s \in \Omega | s \in A \ and \ s \in B \}$$
    $$ A \cap B = B \cap A$$
    Intersection of many Sets
    $$ \cap_{i=1}^n A_i$$
15. Associative Property 结合律
    $$ A \cap (B \cap C) = (A \cap B) \cap C$$
    $$ A \cup (B \cup C) = (A \cup B) \cup C$$
16. De Morgen's Laws
    $$ (A \cup B)^C=A^C \cap B^C$$
    $$ (A\cap B)^C=A^C\cup B^C$$
17. Distributive Property 分配律
    $$ A\cup (B\cap C)=(A\cup B)\cap (B\cap C)$$
    $$ A\cap (B\cap C)=(A\cap B)\cap(A\cap C)$$
18. Disjoint/Mutuallt Exclusive 互斥的
    $$ A\cap B =\emptyset$$
19. Partitioning a Set
    For evert two sets A and B, $A\cap B$and$A\cap B^C$are Disjoint
    $$ A = (A\cap B)\cup(A\cap B^C)$$
    $$ B = (A\cap B)\cup(A^C\cap B)$$
20. Venn Diagrams

## Axioms of Probability

1. $$Pr(A)>0,Pr(\Omega)=1$$
2. $$disjoint : Pr(\cup_{i=1}^{\infty}A_i)=\Sigma_{i=1}^{\infty}Pr(A_i)$$
3. Bonferroni Inequality

    For all events $A_i$,$n>2$
    $$Pr(\cup_{i=1}^nA_i) \leq \Sigma_{i=1}^nPr(A_i)$$
    $$Pr(\cap_{i=1}^nA_i) \geq 1-\Sigma_{i=1}^nPr(A_i^C)$$
4. Multiplication Rule

   乘法原理，A-B三条路，B-C5条路，A-C15条路
5. Permutations 排列
    $$P_n^k=\frac{n!}{(n-k)!}$$
   Suppose that a set has $n$ elements.
   An experiment to select $k$ elements one at a time with out replacement.
   Each outcome consists of the $k$ elements in the order selected.
   Each such outcome is a permutation of n elements taken $k$ at a time.
6. Sampling with replacement 取球要放回
    $$P_n^k=n^k$$
7. Combinations 组合

    Consider a set with n elements. Each subset of size k
    chosen from this set is a combination of n elements
    taken k at a time.
    $$C_n^k=\frac{n!}{k!(n-k)!}$$

    Binomial Coefficient
    $$\binom{n}{k}=\frac{n!}{k!(n-k)!}$$

    Binomial Theorem
    $$ (x+y)^n=\Sigma_{k=0}^n(^n_k)x^ky^{n-k}$$

    Multinomial Coefficients
    $$ \left(^n _{n_1,n_2,...n_k}\right) = \frac{n!}{\Pi_{k=1}^n n_i!}$$
    连续选多个
8. unordered sampling with replacement
