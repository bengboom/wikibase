## 20230403

### 时钟Clock
**两类时钟**
CLK高电平有效 CLK_L低电平有效
也有以上升沿、下降沿有效的电路
![[Pasted image 20230624221854.png]]
**理想同步**
认为“授时中心”给每个元件的CKL信号完全对齐，但实际不是，造成clock skew

### 单双稳态电路
memory 1 bit SRAM 静态存储器 需要6个晶体管（两个反相器四个+输入输出开关管两个（?)  
#### 双稳态电路
Bistable Multivibrator![[Pasted image 20230624222132.png]]
![[Pasted image 20230624222141.png]]
#### 单稳态电路 Monostable Multivibrator
门铃  响一段时间自己又停下![[Pasted image 20230624224754.png]]
> 后面所学的知识解答前面的问题真是一个奇妙的过程，工创1的超声波模块就需要一个10us以上的触发时间，这就是时间约束。工作一段时间自行短路的电路原理也在这里得到解释。

**准稳态电路**
Astable Multivibrator 反复在两种电路之间切换，像振荡器![[Pasted image 20230624224813.png]]
用开关电路可以做出方波振荡电路 质量很差（质量因子**Q**很差）

### Latches and flip-flops “store one bit”
Latch : 不依赖时钟变化 watching inputs continuously, changes any time 需要信号“总是”稳定
Flip-flip：变化依赖时钟 根据时钟信号采样 用的更多 要求在时钟上升沿下降沿上信号稳定即可

##### R-S Latch 锁存器
Reset and set 复位和置位
Cross-coupled inverter pair

set 1 reset 0 不能出现11 电路合法，状态非法（such condition tries to set and reset the latch at the same time)![[Pasted image 20230624231057.png]]

$RS、Q \rightarrow Q_{n+1}$ Karnaugh 得到特征方程，状态转移关系（像FSM） 

对于高电平有效的 -> implement with NAND(或非转换)![[Pasted image 20230624231253.png]]
$S \cdot R = 0$ 约束
$R_{n+1}=S+R'Q_n$特征
对于低电平有效的：相加为1，特征SR取反。
会存在因为***输入信号稳定时间不够***而造成的不确定态*Unknown State*，造成*timing violation*
-> 时间约束 -> 用*propagation delay* 约束 触发时间
**Clocked Latch** 在前面再加两个NAND，CLK触发时，SR才有效，但这还不是Flip-flop

#### D Latch and D flip-flop
D:Delay
![[Pasted image 20230624231622.png]]
Latch:$Q_{n+1}=D(CLK=1)$  和clocked latch 有点像，加一个反相器可构成
D latch 仍然需要触发时间约束（$t_{setup},t_{hold}$ ）


D flip-flop 由两个 D latch 组成

book3.2.3  解释的不错，检测rising edge，用两个D-latch![[Pasted image 20230624231637.png]]
不存在不确定态，*因为输入只有一个，另一个是他的反，这样Q和Q_N永远互补*
除了D和CLK之外，还有两个输入端![[Pasted image 20230624231700.png]]
PR_L 预置位（不常用） CLR_ clear 异步复位端


## 20230403 作业
阅读p109-113 3.2.7
比较D_latch和transistor level latch的原理和优劣
D_latch: data and clock, D latch 22 FF46, gate level, too much
Transistor level latch:对电源电压的要求比较高。减少晶体管数量，在modern commercial chip中使用。Floating output node, No buffer.->with buffer。巧妙地使用了书Section1.7.7的transmission gate.D-latch12,FF20
transmission gate: 利用pmos和nmos，把输入传给输出。

D_latch的CLK要经过一个反相器，带来延迟，不希望有这些延迟，还有线延迟等等
而transmission gate就不会受这样的延迟影响

## 20230410
D_flip flop 可以去掉选择电路，master两个，存D和他的反，给slave更新两个值
![[Pasted image 20230417083822.png]]
其实是三个R-S  latch
这样做可以节省晶体管

### Scan flip flop 
![[Pasted image 20230624231744.png]]
TE TI 利于集成电路的检查
可以检查前级输出，通过给TE施加高电平，接管后续电路的输出，有调试（断电）集成电路的作用，类似于c语言的debug，设置断点功能


### J-K flip-flop
不变或翻转![[Pasted image 20230624232429.png]]![[Pasted image 20230624232506.png]]

### 各种FF特征方程

| Device type             | Characteristic Equation |
| ----------------------- | ----------------------- |
| S-R latch               | Q*=S+R’.Q               |
| D latch                 | Q*=D                    |
| D flip-flop             | Q*=D                    |
| D flip-flop with enable | Q*=EN.D+EN’.Q           |
| J-K flip-flop           | Q*=J.Q’+K’.Q            |
| T flip-flop             | Q*=Q’                   |

Register File 一组完成某种功能的寄存器，不是“文件”的意思，是一种结构的名字[WiKi](https://zh.wikipedia.org/wiki/%E5%AF%84%E5%AD%98%E5%99%A8%E5%A0%86)
OE output enable 输出使能 可以加在decoder上面

考试最难出成这样：给一个没见过的Flip-flop，用其设计电路
画图要规范，**连接要打点**，Not connect if cross without dot
最后画电路图 时序图

## 20230412
中英文对应

|触发器|锁存器|寄存器|
|---|---|---|
|触发到稳态| Latch|FF 检测CLK边沿|

#### 同步设计 synchronous design

Ring oscillator 三个反相器环形连接实现高低电平震荡
对于FF，输入Next state 输入current state，要下一个时钟沿才能输出变为输入
Racing condition竞争

同步电路定义：要么组合逻辑要么寄存器 每个环路必有寄存器 CLK均同步
Latch 是时序设计 不是同步电路 要FiFo
CLK经过buffer也不算同步 时钟沿/有效沿对齐才算 差距控制在极小范围内
减少clock skew 同时加驱动 走线长度尽量均匀*例如主线在总间穿过再延伸的Clock Tree* 
布好时钟线后再布其他元件 优先级最高 **满足setup hold公式约束**

降低耗电 门控时钟 gating clock **实现方法？[wiki](https://en.wikipedia.org/wiki/Clock_gating)
***为什么可以降低耗电啊 同时还增加延迟吧*** 

#### Finite State Machine 一种同步电路实现
- inputs outputs
- state register/memory
- combinational logic

##### Moore Mealy
moore machine 输出仅与current state有关，inputs改变next state logic
mealy machine 输出与current state & inputs 有关 (相比moore多一根从input到output的线)
在实现里面moore更好 相对更独立 容易计算寄存器之间的delay

##### Coding
FSM code scheme对有限状态机的状态进行编码的方式

| In Order        | Grey                              | Johnson                                      | One-Hot独热码             | Synthesis Tool Optimization |
| --------------- | --------------------------------- | -------------------------------------------- | ------------------------- | --------------------------- |
| 按顺序 容易理解 | 相邻两状态仅1bit不同 翻转小功耗小 | 相邻状态仅相差一位，首尾状态相邻Ring counter | 每次只有一个EN 关键路径短 | Auto                        |
各有各的好处

## 20230417
##### FSM state coding requirements
- minimal risk
	- unused code -> still identify -> regarded as initial/some safe state
	- 在卡诺图上体现为未用全为0状态(initial)
	- safest method, any code has its corresponding state
	- 故障导向安全
- minimal cost
	- 永不进入无效状态，视为don't cares
	- simpler logic -> minimal cost

设计同步电路的步骤

## 20230419
有限状态机需要满足；各个状态互异*Mutual exclusion*、完备*All inclusion*

#### 子状态机
可以将一个有限状态机拆分为多个有限状态机*Main Machine, multiple submachine*
未来的大型设计都会有多个有限状态机，红绿灯分为Mode FSM, Light FSM
多周期Multicycle不好做Pipeline

#### Timing Factors
**Dynamic Discipline** the input of a synchronous sequential circuit must be stable during the setup and hold aperture time around the clock edge.
**Setup time, Hold time, Clock skew, Sequential circuits**
$t_{setup}$ clk之前需要稳定的时间
$t_{hold}$ clk之后要保持的时间
$t_{pcq}$ 完成更新的时间 *Clock to Q Propagation delay -- maximum*
$t_{ccq}$ 开始更新的时间 *Clock to Q Contamination delay -- minimam*
clock skew 时钟线边沿对不同元件的时间差 时钟歪斜 物理世界不可能全部对齐
##### Setup time constraint
$T_{c}\geq t_{pcq}+t_{pd}+t_{setup} +t_{skew}$
理解是，数据要在一个周期内算完，传到下一个FIFO
![[Pasted image 20230420152014.png]]
##### Hold time constraint
$t_{ccq}+t_{cd}\geq t_{hold}+t_{skew}$
理解是，数据进入下一个FIFO，存入前信号必须一段时间不变（hold time)
传输时间必须大于这个要求
![[Pasted image 20230420152022.png]]

我们的实际会影响$t_{cd},t_{pd}$并决定$T_c$

#### Synchronizer failure and metastability
synchronizer同步器。外部信号输入的时间无法控制，需要syncer与内部时钟同步。
Synchronizer resolution time $t_r=t_{clk}-t_{comb}-t_{setup}$
通过两个FF实现reliable synchronizer design,再加上clk divider, skew sync.....
还有一种：通过级联多个同步器，每个出问题的概率是p，n个是$p^n$，如果输入变化快，要用这种而不是分频器。
![[Pasted image 20230419094033.png]]

#### Metastable Timing
![[Screenshot 2023-06-26 at 14.59.31.png]]
res>t -> Q metastable -> failure
$P(t_{res}>t)=\frac{T_0}{T_c} e^{-\frac{t}{\tau}}$, $t=T_c-t_{setup}$

$MTBF=\frac{1}{P(failure)/sec} =\frac{T_c e^{\frac{T_c-T_{setup}}{\tau}}}{NT_0}$
N: number of changes of D in 1s
指数增长，resolution time 越大越好，MTBF *Mean Time Between Failure*越大越好

#### Ripple (Asynchronous) Counter
分频器，计数器，using JK flip-flop *Binary Counter*
通过加组合电路，到某个数后给Clear端置1，可以除以一个不是$2^n$的值
FF的$\bar Q$接下级FF,实现减计数器 
![[Pasted image 20230419102809.png]]
**MSI counter**  *Medium Scale Integration*
带输入选择功能、功能选择（加还是减）
**RCO** *Ripple Carry Output* generates a pulse signal when the counter overflows from its maximum count value back to zero.

![[Pasted image 20230419103416.png]]


## 20230424
#### Shift Register 移位寄存器
![[Pasted image 20230625002644.png]]
convert parallel to serial, for data transfer between modules
import IO channel, can be configured to perform -\*/~ operations
also used in TCP/IP CRC check



