OS:**管理**软件与硬件的**系统软件**

Glossaries
> simple batch systems 简单批处理系统
> Uni-programming Multiprogramming 单道多道
> Time Sharing 分时
> **Concurrency 并发 单CPU构成的 CPU多核只是提高并发度
> Parallelism 并行 多CPU构成的**
## 现代操作系统四种基本观点
从外部看
- 用户环境User Interface
- 虚拟机器Virtual Machine操作系统为硬件扩充了功能，操作系统是硬件机器上的虚拟机器
从内部看
- 资源管理Resource Manager
- 作业组织Job Organizer这个概念PC一般没有，大型机有
## 需求
### 功能性需求
- 计算机用户需要的**用户指令** Interface
- 应用软件需要的 System Call **系统调用**，所有系统调用的集合叫做程序接口API *application programmable interface* 常用api: POSIX.1 WIN32 程序接口事实上定义了一台虚拟计算机
### 非功能性需求
- 性能 Performance
- 效率 Efficiency
e.g. maximize throughput, minimize response time 最大用户容纳量（承载能力）
- 公平性Fairness 对每个进程公平 （但是中断有优先级？）
- 可靠性 Reliability
- 安全性 Security
- 可伸缩性
- 可扩展性
- ......

Major Requirements:
- Interleave the execution of several processes to maximize processor utilization while providing reasonable response time
- Allocate resources to processes
- Support interprocess communication
## 依赖
对硬件平台有依赖，那么硬件需要一些功能
- Timer 计时器 需要分时
- I/O Interrupts
## 基本概念
#### Thread 
线程指程序的一次相对独立的运行过程
现代OS中，线程是**系统调度的最小单位**
> 为什么出现线程？
> 进程太大了，管理起来开销大，于是分解为更小的单位，线程更轻量
> 线程 有状态、上下文、local variables等等
> 大量资源是多个线程共享，能够访问上面**Process**的资源
> *All thread of a process share this*
> 轻量化后的优点：创建终止切换线程更快，进程内线程间通信without invoking kernel
#### Process
进程是系统**分配资源**的基本对象
现代OS中，进程是系统中**拥有资源的最小实体**
传统OS中，进程也是系统调度最小单位
**Thread Process**都是动态概念 需要创建删除调度通信等等
### Virtual Memory
虚拟存储，进程的逻辑地址空间，满足多作业同时驻留内存的需要，换入换出
### File
文件 命了名的字节流都可以是，不只是传统意义上的文件
Output
## 发展过程
- 串行处理 Serial Processing 没有OS
- 简单批处理系统 Simple Batch Systems
	- Monitors控制当前程序，负责调度一个个程序，当前程序完成回到Monitor再调度
	- 内存单道，外存层面是批处理
	- 单道程序设计 **Uni-Programming** Processor等待IO指令完成 inefficient
- 多道批处理系统
	- 多道程序设计 **Multiprogramming**
		- 允许多道程序同时**准备运行**，当一个程序在等待输入输出数据时，系统自动启动另一个程序运行，IO等待结束时，中断保护还原
		- maximize processor use 总耗时减少
		- 但是道数太多，等待时间不足以处理所有程序，就会卡
		- 实际效果根据作业性质有关  **可否识别程序性质，根据性质来调动得到最佳调度方案？**
			- heavy compute or heavy I/O
			- duration and memory
			- IO needed?(printer, hard disk...)
		- 时间分片 通过中断来切换
		- Difficulties
			- Improper synchronization 同步
				- 一段共享内存，A往里面放，B从里面取
				- A放的前提是有空间，B取的前提是有数据
				- B取完数据唤醒A去放，A放满唤醒B让B取，这就形成了同步
			- failed mutual exclusion 互斥
				- A正在放的时候B不能取，不然就脏了
				- 某个进程在读写的时候其他进程不能访问同一段空间
			- nondeterminate program operation
			- deadlocks 死锁
				- AB互相等，A等B，B等A，都在等，不运行了
	- 内存中存放多个作业，并发执行，**作业调度程序**负责作业调度
	- 需要硬件支撑：I/O中断 DMA...
	- 特点：多道性、调度性、无序性（一直在调度）、无交互能力（批处理系统没有，在封闭环境运行）
- 分时系统 Time Sharing
	- 满足更多用户需求 using multiprogramming to handle multiple **interactive交互** jobs
	- 处理器时间被多用户共享，多用户同时通过terminal访问系统
	- 特点：多路性 独立性 及时性 交互性
	- minimize response time 分时的快慢是人的心理决定的，人没觉得慢就行
- 实时系统
	- 系统能**即时**相应外部请求，在 **规定时间内** *必须* 开始或完成对该事件的处理，控制所有实时任务协调一致运行
	- 航空航天、金融银行、军用无人机等等，需要更严格的实时
		- 实时控制系统
		- 实时**信息**系统
	- 可确定性、可响应性、用户控制、可靠性、故障弱化能力
## 基本类型
- 单机OS
	- batch processing
	- time sharing
	- real time
- 并行OS
- 网络OS
- 分布式OS
## 基本特征
### 任务共行
宏观上，多个任务同时
微观上看，单机系统并发交替task concurrency，多机系统之间的并行task parallelism
### 资源共享
宏观上看多任务同时使用资源
微观上看**交替互斥**使用资源
> 中大型机IO量大，有通道的概念，输入输出通道。设备和通道可以与CPU并行
### 任务管理模型
Task：计算机系统在某个资源集合上 相对独立的计算过程
现代os中，由线程（更轻量）和进程描述 传统os仅进程
### 资源管理模式
系统用**竞争**模式管理软件资源，需要为共享资源的进程提供互斥机制
硬件资源通过**分配**模式管理，**申请-分配-使用-释放-回收**
## Architecture
操作系统软件的体系机构
### 常见风格
#### 两类子系统
- 用户接口子系统 （上层）
	- 用户命令
- 基础平台子系统 （下层）
	- 系统调用
![[Screenshot 2023-09-02 at 10.04.53.png]]
#### 结构风格
- layered structured style分层
	- 每一层实现一组基本概念，**严格**划分
	- 每层实现只依赖**直接**下层，并对上层**隐藏**其下层
	- 特殊分级结构
	- Pros 可维护性、可扩展性、可移植性
	- Cons 时空效率低，构造实现困难
- hierarchical 分级
	- 分散一些 包含若干级Level
	- 每层依赖以下**各级**，看得见下面多余的概念属性
	- 特殊分块结构
- modular 分块
	- 基本单位 module
	- 没有上下级，**任意**引用
	- Pros 切合实际 高效紧凑
	- Cons 不利于灵活性（块与块互相影响）
	- Linux 全球协作而成 所以成了Block
#### 执行模式风格
模式Mode：程序运行中，硬件体系结构提供的CPU**特权**模式与权限有关
- 多模式结构风格
	- 提供更多模式更安全可靠性
	- 模式切换开销--是一种**软中断** 模式切换规则判定
	- 硬终端是保护现场 这是更改运行权限
- 单模式结构风格
	- 只有一种模式模块，所有程序运行在一种模式下 *DOS*
- 折中：双模式 *Windows*
	- 核外子系统 用户模式 User Mode less-privileged
	- 核心子系统 内核模式 Kernel Mode more-privileged
	- MicroKernel 微核结构
		- 剔除核心子系统成分移到核外子系统实现，保证核心子系统简洁高效 
		- MaxOS X
		- 难界定哪些是多余成分，一般是drivers, file systems......
		- 扩展性可靠性分布式支持 运行效率降低（共享核心）
### 研究实例
#### Unix
#### Windows NT
#### Linux
可以看一些分析文献，看源代码特别耗时、难



## 本章作业
> 1 分析现代操作系统的基本特征
> 	**分时多任务处理和内存管理**
> 	共享 并发 互斥等
> 	在硬件之上、软件之下，提供一个框架，广泛的兼容下层硬件、广泛的支撑上层软件
> 2 为什么会有进程 分析理解进程的概念 上下文的含义
> 	互不干扰的独立运行单元
> 	上下文是该进程所需要的附近的代码段、寄存器、内存中的数据和程序需要访问的各种资源、运行状态等
> 	因为计算机的硬件资源有限，多个程序又想要同时占用计算机资源，于是出现了进程这样的概念，来管理调度多个任务的执行，为各自的程序提供独立运行的隔离空间
> 3 区别并发和并行的异同
> 	并发是单CPU通过分时等方式处理同时到来的多个任务
> 	并行是多CPU同时同步处理不同任务
> 	多核CPU只是增加了并发度，不是并行
> 4 进程调度程序、时钟中断程序和命令解释程序在那种模式下执行
> 	进程调度程序是操作系统中的一个重要组件，它负责控制系统中的进程，决定哪个进程可以获得 CPU 的使用权，以及如何分配 CPU 的时间片。**核心子系统。Kernel Mode**
> 	时钟中断处理程序是操作系统中的另一个重要组件，它负责在 CPU 运行时定期产生中断信号，通知操作系统进行下一步操作。**核心子系统。Kernel Mode**
> 	命令解释程序是操作系统中的一个重要组成部分，它负责将用户输入的命令翻译成相应的操作系统命令，使得操作系统可以根据用户的意图来执行相应的操作。**核外子系统。User Mode**
> 5 分析系统调用时的模式切换过程
> 	当一个用户程序需要执行操作系统特权任务时，会通过系统调用请求切换到内核模式运行
> 	一次系统调用的过程，其实是发生了两次 CPU 上下文切换。（用户态-内核态-用户态）
> 	 1 用户程序准备系统调用
> 	 2 通过系统调用指令触发系统调用
> 	 3 模式切换 从User Mode 到 Kernel Mode
			3.1 保存 CPU 寄存器里原来用户态的指令位
			3.2 为了执行内核态代码，CPU 寄存器需要更新为内核态指令的新位置。
			3.3 跳转到内核态运行内核任务。
		4 执行内核模式中需要执行的过程
		5 返回用户模式
			5.1 当系统调用结束后，CPU寄存器需要恢复原来保存的用户态上下文
			5.2 CPU寄存器回到为用户态指令的位置
			5.3 切换回用户程序，继续运行进程