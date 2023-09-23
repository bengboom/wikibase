Architectural State : Program Counter and 16 registers
some microarchitecture add non-architectural state to simplify logic or improve performance

Microarchitecture **Datapath** and **Control**

## Performance Analysis
[[Chapter 1 Introduction]]
$$Execution Time = instructions \frac{cycles}{instruction} \frac{seconds}{cycle} $$

## Single Cycle Processor
[[Chapter 5 CPU]]
一条指令一个周期，指令周期长
B BTA= (Sign-extended immediate <<2 )+ (PC+4)
BL (Link) PC => LR
![[Screenshot 2023-05-25 at 11.29.43.png]]
![[Screenshot 2023-05-25 at 11.31.21.png]]
![[Screenshot 2023-05-25 at 11.31.41.png]]
单周期处理器的critical path
$T_c = t_{pcq\_PC} + t_{mem} + t_{dec} + \max(t_{mux} + t_{RFread}, t_{sext} + t_{mux}) + t_{ALU} + t_{mem} + t_{mux} + t_{RFsetup}$


## Multicycle Processor
一条指令多个周期，频率可更高
### Cycle 1 Fetch Instruction

### Cycle 2 Read Register & Extend Immediate

### Cycle 3 Get Address (ALU)

### Cycle 4 Read Data from Memory

### Cycle 5 Write Data Back to Register File

### Cycle 6 Increment PC (PC<=PC+4) (与Cycle1同时)

### Cycle 7 Update R15 （与Cycle2同时)


**IR RF Execute DM RF**
![[Pasted image 20230529100034.png]]

![[Pasted image 20230529100038.png]]

**不同指令有不同的Clock per instruction CPI**
B: 3 cycles
数据、存储 4cycles
LDR 5 cycles

冯诺依曼结构 指令数据共享Instruction

控制逻辑变成了有限状态机，以适应按照时钟周期转换输出
![[Screenshot 2023-05-31 at 09.23.21.png]]
![[Screenshot 2023-05-31 at 09.36.55.png]]


Performance 
增加了寄存器，减少单个时钟周期长度。可能比single cycle 更慢， 虽然最大时钟频率高了，但单指令CPI更多，导致对于**特定程序**消耗时间更长。

对于多周期处理器，最短时钟周期为
$T_c = t_{pcq} + 2t_{mux} + \max(t_{ALU} + t_{mux}, t_{mem}) + t_{setup}$

## Pipelined Processor
[[Chapter 6 Instruction Pipeline]]
design a pipelined processor by subdividing the single-cycle processor into five pipeline stages.
**Fetch, Decode, Execute, Memory, and Writeback**

Forward CutSet 割集：把Datapath分割为多个小集合，向前传播![[Screenshot 2023-06-05 at 09.35.38.png]]

还需解决冲突问题。
首先引入转发。![[Screenshot 2023-06-05 at 10.04.25.png]]
![[Screenshot 2023-06-05 at 10.04.43.png]]
在满足地址相等的情况就进行转发。
转发不能解决所有数据问题。 
有的时候下一条指令需要数据的时间早于此条指令产生的时间
于是可以让下一条指令在计算前的状态停一下![[Screenshot 2023-06-05 at 09.53.15.png]]
但这样下下条指令会和下一条指令起冲突 阻塞 stall
![[Screenshot 2023-06-05 at 09.53.31.png]]
所以还需要给Hazard Unit 引入Stall 处理单元。给寄存器组EN信号，是否阻塞寄存器更新。
转发 阻塞可以解决全部Data Instruction数据冲突包括Load Store
对于控制冲突：要执行分支跳转时，分支跳转后的指令已经进入流水线，要冲刷flush这些指令
阻塞 冲刷 转发都有独立的Hazard Unit控制
![[Screenshot 2023-06-05 at 09.59.46.png]]
最终的结构
![[Screenshot 2023-06-05 at 09.37.17.png]]
需满足周期要求
![[Screenshot 2023-06-05 at 09.38.03.png]]
为什么Decode Writeback 是两倍时间？因为寄存器前半周期写后半周期读，也就是说周期需要大于读/写操作的两倍

> 老师出考研题目特别灵活，学生出考场怀疑做错考场了，学校怕出考试事故，把老师出的题目拿给清华东南审核，结论是都在考纲之内，但是很灵活，老师很得意

## Advanced Microarchitecture
> 课程设计每年都有人用，往届有的组用了还真拿第一

### Deep Pipeline
10-20个 stages, more stages
limited by: hazard, sequencing overhead, power, cost

### Branch Prediction
思想：预测的很好，不需要在跳转的时候停下来，就会更快
准确率已超90%
- 静态预测
	- 例如向前跳的总是跳转
- 动态预测
	- 占小部分
还可以把for loop展开，不写循环，一条一条写，代码长度更长，减少跳转

### Superscalar
设计思想：一次读两条指令，同时做，同时写回。
和多核处理器不一样，内存只有一个，寄存器只有一个，计算单元有两套，读取写入口数增加
hazard不一定要处理，只要汇编程序写得好，做一些约束

### 乱序执行
这也能硬卷？

### SIMD
Single instruction multiple data
两个十六位的数，ALU32位，一次算两个结果出来！效率高很多

### Multithreading and multiprocessors
Word processor: thread for typing, spell checking, printing
Multiprocessors: multiple cores on a single chip
- homogeneous
- heterogeneous big.LITTLE
- clusters, memory in every core
> 老师的组要做这个，有兴趣的欢迎了解

往届最多放了四个核