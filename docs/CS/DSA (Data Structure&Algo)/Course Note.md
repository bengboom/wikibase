# Data Structure 数据结构

## 上课笔记

### 20220906

0. #### Big O , big theta, big omega

   | | |
   |---|---|
   |$\Omega$| lower bound|
   |$\Theta$| average|
   |O| upper bound|

1. #### 不同时间复杂度计算机一秒内处理的最大n值

    |O|n|
    |---|---|
    |O(1)|5*10^8|
    |O(n)|10^8|
    |O(nlogn)|10^6|
    |O(n^2)|5000|
    |O(n^3)|300|
    |O(2^n)|25|
    |O(n!)|11|

### 20220913

1. #### Master Theorem 主定理

    $$T(n) = aT(\frac{n}{b}) + f(n)$$

    设$a\geq1,b>1$，$f(n)$是定义在正整数集上的递归函数，满足下列条件之一：

    1. 对于某常数$ϵ>0$，有$f(n)=O(n^{\log_ba-ϵ})$,

        则$T(n)=Θ(n^{\log_ba})$

    2. 若$f(n)=Θ(n^{\log_ba})$,

        则$T(n)=Θ(n^{\log_ba}\log n)$

    3. 对于某常数$ϵ>0$，有$f(n)=Ω(n^{\log_ba+ϵ})$,且对于当$c<1$,$n$足够大时，有$af(n/b)\leq cf(n)$,

        则$T(n)=Θ(f(n))$

    >算法和数据结构,是程序设计的核心关键环节,对算法性能起决定性作用

2. #### Fibnacci 的矩阵快速幂解法

    $$\begin{aligned}&{\left[\begin{array}{l}
    F_2 \\ F_1 \end{array}\right]=\left[\begin{array}{l} 1 \\ 1 \end{array}\right]\left[\begin{array}{l}
    F_{n+2} \\
    F_{n+1} \end{array}\right]=\left[\begin{array}{ll} 1 & 1 \\
    1 & 0 \end{array}\right]\left[\begin{array}{l}F_{n+1} \\
    F_n
    \end{array}\right]} \\
    &{\left[\begin{array}{l}
    F_n \\
    F_{n-1}
    \end{array}\right]=\left[\begin{array}{ll}
    1 & 1 \\
    1 & 0
    \end{array}\right]\left[\begin{array}{l}
    F_{n-1} \\
    F_{n-2}
    \end{array}\right]=\left[\begin{array}{ll}
    1 & 1 \\
    1 & 0
    \end{array}\right]^2\left[\begin{array}{l}
    F_{n-2} \\
    F_{n-3}
    \end{array}\right]=\ldots=\left[\begin{array}{ll}
    1 & 1 \\
    1 & 0
    \end{array}\right]^{n-2}\left[\begin{array}{l}
    F_2 \\
    F_1
    \end{array}\right]}
    \end{aligned}
    $$

3. #### 线性表

    顺序结构：随机访问O(1)原因：地址=首地址+n*sizeof(type) 得到

    链式结构：

### 20220920

1. #### test
