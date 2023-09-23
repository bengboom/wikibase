## 20230320

### 组合逻辑
- 组合逻辑：输出取决于当前输入，与历史无关。 (不正确：no feedback 有的有反馈的电路也是组合逻辑)教材上：无环路cyclic paths，信号最多经过path一次。可当做黑盒，用真值表描述。
- 时序逻辑sequential logic circuit：与过于状态有关

### Basic components and operations
NOT AND OR与或非 True/False
取补$Y=AB,\overline Y = \overline A + \overline B$ exchange与或
对偶$Y=AB+CD, Y_{Dual} = (A+B)(C+D)$  Dual 与或全互换，01全互换，输入不变

推泡泡：and or前/后的泡泡，交换时and,or也要换。
**其实是DeMorgan's Law**
![[Pasted image 20230624205611.png]]
Equivalent 真值表相等
Complement 补

### Axioms
![[Pasted image 20230624205511.png]]
五条基本引理及其对偶描述了基本逻辑，延伸出了***很多***定理(像离散数学的一堆)
**定理的灵活使用在做题中很重要，比如Expanded form**

异或真有趣。

好像可以用矩阵运算得到电路设计（每个元件的输入输出关系矩阵相乘）

对于逻辑的标准表达式
sum of products  Minterm SOP 电路：与或结构 最小项
product of sums  Maxterm POS
$\Sigma$ and $\Pi$ Nomenclature and its corollary

$f’(A,B,C) = Σ 2,3,5,6 = Π 0,1,4,7$

canonical form 标准型 主析取范式 主合取范式
minimal Boolean equation 最简形式 不一定是minterm maxterm
>Discrete Math

## 20230322

#### Karnaugh Graph
don't care condition -- using X to replace
margin: Gray Code -- 1bit different from neighbor 要注意格雷码的编码顺序 00 01 11 10 ......
数码管优化：1.卡诺图优化。2.多个管共用一些结果 实现电路简化。3.利用don't care再化简
对于更多变量(>4)的卡诺图，卡诺图可以采用更高维？yes.
如果卡诺图中单个margin超过三个变量的话，可以圈在一起的就不一定相邻了。
五变量：四变量卡诺图画两张（双层）立体。

Each circle should be as large as possible
哪怕重合 也要更大，越大越好。.

A circle may wrap around the edges of the K-map
单变量/双变量在一个margin里，可否跨越边界的情况不同（有时不能有时可以，以“有无共同点“为准）

course problem, answered by ChatGPT
```markdown
F = ((X+Y’)Z)+(X’Y’Z’) =(X+Y’+X’)(X+Y+Y’)(X+Y’+Z’)(Z+X’)(Z+Y)(Z+Z’) how this was made

To simplify the expression F = ((X+Y')Z)+(X'Y'Z'), we can first distribute the terms:

((X+Y')Z) + (X'Y'Z') = XZ + Y'Z + X'Y'Z'

Next, we can use the following Boolean identities:

-   X + X' = 1 (complement)
-   X + 1 = 1 (domination)
-   X + 0 = X (identity)
-   X + X = X (idempotent)
-   XY + X'Z = X(Y+Z) (distributive)
-   XYZ + XY'Z + X'YZ + X'Y'Z = XY(Z+Z') + X'Y(Z+Z') = (X+X')YZ + (Y+Y')X'Z (distributive)

Using these identities, we can simplify F as follows:

F = ((X+Y')Z) + (X'Y'Z') = XZ + Y'Z + X'Y'Z' (distributive) 
  = XZ + Y'Z + X'Y'ZZ' + X'Y'Z'Z' (adding ZZ' and Z'Z) 
  = XZ + Y'Z + X'Y'Z(Z+Z') + X'Y'Z'(Z+Z') (factoring out Z+Z') 
  = XZ + Y'Z + X'Y(Z+Z')Z + X'Y'(Z+Z')Z' (distributive) 
  = XZ + Y'Z + X'YZZ + X'YZ'Z + X'Y'ZZ' + X'Y'Z'Z' (distributive) 
  = XZ + Y'Z + XYZ + XY'Z + X'YZ + X'Y'Z' (using XYZ + XY'Z + X'YZ = (X+X')YZ + (Y+Y')X'Z) 
  = (X+Y'+X')(X+Y+Y')(X+Y'+Z')(Z+X')(Z+Y)(Z+Z') (using the distributive property to expand the terms)

Therefore, F can be expressed as (X+Y'+X')(X+Y+Y')(X+Y'+Z')(Z+X')(Z+Y)(Z+Z'), which is in the form of a product of sums.
```

## 20230327
### Combinational Building Blocks
synthesis = translation + optimization + mapping
> 华为已经解决14nm以上EDA工具卡脖子问题。
> 现在的mapping已经在使用神经网络、生成对抗网络。以前是线性、非线性规划。

Truth table  --mapping--> Look up table (LUT)
Karnaugh方法一般最多能到6个变量，多了用“表格法”，对于计算机运算更方便。

###### multiplexer MUX
位宽$s=upper(log_2 n)$
**多路选择器，最后只有一个输出**($n ->1$)，还有EN使能，根据S决定输出哪个。设计时有些管道fan in/out太大，需要加驱动（延时器/两个反相器）
可以将多个mutiplexer级联构建更大规模的多路选择器。两个多路选择器，地址高端给两个EN，只启动一个（需要一个高电平启动，一个低电平启动的multiplexer)，最后只有一个输出。两个输出取Or后得到最后的输出。![[Pasted image 20230624215445.png]]
multiplexer with tristate buffers
4bit MUX需要三个2bit MUX($Y=D_0 \overline S + D_1 S$)

n+1变量的多路选择器 另一种形式 输出用多的那个来表示，接到输入端。

###### demultiplexer
a combinational logic circuit with an input line, $2^n$ output lines and n select lines.
![[Pasted image 20230624215824.png]]
![[Pasted image 20230624215913.png]]

###### decoder
与multiplexer不同，[Differences](https://www.tutorialspoint.com/difference-between-multiplexer-and-decoder#:~:text=A%20multiplexer%20select%20one%20of,into%20a%20corresponding%20output%20signal.&text=The%20main%20function%20of%20a,transmit%20the%20data%20and%20signal.),有多个($2^n$)输出。根据($n$)输入，**在其对应的端口输出一个高电平，其他均输出低电平。**![[Pasted image 20230624220147.png]]
EN使能：分高电平使能和低电平使能。
Decoder is a special case of a demultiplexer **without the input line.**
**Decoder 只解码，Demultiplexer 将输入输出到对应的编码(存在输入输出关系)**

单个decoder多个使能端口：为了级联。

为什么Decoder通常选择低电平有效：悬空高阻态很可能被判断为逻辑1，可能被误判。用低电平可以确保没有问题。 

>我要说考试要考的xxx，大家才有兴趣听 74x138![[Pasted image 20230624220107.png]]

###### encoder
decoder的反函数。![[Pasted image 20230624220239.png]]
priority encoder 只看最高的一位。输出最高位的位号。![[Pasted image 20230624220257.png]]

### Timing
#### Transition Time
path, critical path, path balance
![[Pasted image 20230624220646.png]]
只讨论gate delay， 不考虑line delay 时
sy: Select delay
dy: data delay
不同输入端的变化导致的delay不同，分别计算。

#### Propagation Delay & contamination delay
![[Pasted image 20230624220718.png]]
>The _propagation delay_ $t_{pd}$ is the maximum time from when an input changes until the output or outputs reach their final value. The _contamination delay_ $t_{cd}$ is the minimum time from when an input changes until any output starts to change its value. $t_{pd}$ and $t_{cd}$ may be different for many reasons, including **different rising and falling delays**.

propagation是最大，contamination是最小
contamination污染 怎么理解？从这个时刻开始到prop delay中间的信号会变化 所以是污染的

Line Delay: 90%, Gate Delay 10%

#### Glitches (static hazard)
Consume more power, by switching more frequently. 但不带来任何功能
- static-1 hazard 前后两种状态都应该是1，但中间出现了短暂的0![[Pasted image 20230624221033.png]]
- static-0 hazard 前后都是0但中间出现短暂的1![[Pasted image 20230624221038.png]]
可以由不同输入delay不同导 致，一个变得快一个变得慢。
可以在Karnaugh图中可以增加一些冗余项以去掉Glitch，对于第一个static-1 hazard![[Pasted image 20230624221052.png]]

#### Timing Hazard *危害*
- Race 在组合电路中信号经由不同的途径达到某一会合点, 在时间先后上影响输出
- Hazard 由于竞争而引起电路输出发生信号错误的现象 表现为毛刺Glitches
- 有竞争Race不一定存在冒险hazard 有冒险一定存在竞争

dynamic hazards
*The possibility of an output changing more than once as the result of a single input.*
由门电路速度不同导致
![[Pasted image 20230624221352.png]]
要设计hazard-free circuit







