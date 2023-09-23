[本课程PPT是老师抄的南大2013Fall的](https://cs.nju.edu.cn/swang/CompArchOrg_13F/index.htm)

## 发展历程
### 第一代：真空管计算机 (1946 - 1957)
- 运用了真空管（电子管Vacuum Tube）技术
- 代表：ENIAC计算机
- 提出了冯·诺依曼结构的主要思想，包括存储程序，指令和数据的区别等。

### 第二代：晶体管计算机 (1950年代后期～1960年代初期)
- 逻辑元件采用晶体管，内存由磁芯构成，外存为磁鼓与磁带
- 特点：变址，浮点运算，多路存储器，I/O处理机，中央交换结构(非总线结构)
- 软件：使用高级语言，提供了系统软件
- 代表机种：IBM 7094 (scientific)、1401 (business)和 DEC PDP-1

### 第三代：中小规模集成电路(SSI/MSI)计算机 (1960年代中期～1970年代中期)
- 逻辑元件与主存储器均由集成电路（IC）实现
- 特点：微程序控制，Cache，虚拟存储器，流水线等
- 代表机种：IBM 360和DEC PDP-8 提出总线
- 分为巨型机(Supercomputer)：Cray-1、大型机(Mainframe)：IBM360系列提出兼容机、小型机(Minicomputer)：DEC PDP-8

### 第四代：大规模/超大规模计算机LSI/VLSI/ULSI (1970年代初期～至今)
- 微处理器和半导体存储器技术发展迅猛，微型计算机出现
- 特点：共享存储器，分布式存储器及大规模并行处理系统

## 基本结构
ALU 寄存器等
- 运算器（数据运算）：ALU、GPRs、标志寄存器等
- 存储器（数据存储）：存储阵列、地址译码器、读写控制电路
- 总线（数据传送）：数据线(MDR)、地址线(MAR)和控制线
- 控制器（控制）：对指令译码生成控制信号

冯诺依曼：运算器 存储器 控制器 输入设备 输出设备

CPI *cycles per instruction*
IPC *instructions per cycle*

MIPS：指令数
MFLOPS：浮点运算次数

## 性能评价
一般用执行时间来测量
CPU时间分为用户CPU时间和系统CPU时间
为了执行用户程序 需要操作系统提供支持
还有其他时间：等待IO等等

系统性能和CPU性能区别
本章主要讨论CPU性能

- 响应时间（response time）指从作业提交开始到作业完成所用的时间
- 吞吐率（ throughput）指单位时间内所完成的工作量
- 指令执行速度（MIPS、MFLOPS/TFLOPS/PFLOPS）
- 基准程序（ Benchmark） 用来评测计算机的性能
	- SPEC *System Performance Evaluation Committee*
- 阿姆达尔定律（Amdahl’s law）

算时间 提升

Amdahl定律
$p=1/(t/n +1-t)$
t:改进部分时间 n:改进部分改进倍数 p:整体改进倍数

存在$n\rightarrow \infty$ 的情况 

冯诺依曼结构在第一代计算机出现，他参与了研制。
主存就是内存，磁盘就是硬盘。
高级语言通过编译转换为汇编。


