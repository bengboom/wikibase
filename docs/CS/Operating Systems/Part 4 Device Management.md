## I/O Devices
- human readable
- machine readable 
- communication

- Data Transfer Rate
- Application
- Complexity of Control
- Data Transfer Unit
	- 磁盘的扇区，键盘的一次敲击
- Data Representation
- Error Conditions

> **驱动程序**用来屏蔽设备细节，为操作系统提供接口，外部设备商提供的，也有接口的标准。操作系统会集成一些已有的设备驱动，对于更新的设别可能就需要手动安装。
## Organization
[[Chapter 8 Interconnections and IO systems#I/O]]
### Programmed I/O
![[Screenshot 2023-10-31 at 11.29.50.png]]
### Interrupt-Driven
用中断解决Programmed I/O的busy-waiting忙等问题
- Processor is interrupted when I/O module ready to exchange data.
- Processor is free to do other work.
- No needless waiting.
- Consumes a lot of processor time because every word read or written passes through the processor.
![[Screenshot 2023-10-31 at 11.43.27.png]]
### DMA Control
- Transfers a block of data directly to or from memory.
- An interrupt is sent when the task is complete.
- The processor is only involved at the beginning and end of the transfer.
cycle stealing 
## OS Module Structure
[[Chapter 8 Interconnections and IO systems#直接存储器访问方式 DMA]]
DMA是目前最常用的I/O技术
![[Screenshot 2023-10-31 at 11.51.24.png]]

### Evolution
- CPU control I/O directly, 指令集不分开
- I/O Controller added
	- 指令集开始划分
	- using programmed I/O
- Controller using interrupt
- DMA (PC机已经足够)
- I/O separated processor: I/O channel（中大型机、服务器）
	- own local memory, also a computer

### Structure
![[Screenshot 2023-11-07 at 10.35.32.png]]

Design Issue
- Efficiency
- Generality
	- 分层结构，上层屏蔽下层细节![[Screenshot 2023-11-07 at 11.12.51.png]]
		- 通常两层，设备硬件无关层（设备映射功能）和设别硬件相关层（设备驱动功能）![[Screenshot 2023-11-07 at 11.13.02.png]]

逻辑I/O设备分类
- 字符设备 Stream-Oriented Deice
	- a stream of bytes
	- byte:键盘 line:显示、打印
- 块设备 Block-Oriented Device
	- fixed size blocks
	- disks, tapes


![[Screenshot 2023-11-07 at 11.20.37.png]]
## I/O Buffering
### Importance
假定：一个用户进程将从磁带上读入若干块数据(每块100字节)；数据块装入用户进程地址空间中的一个数据区，其虚拟地址为1000～1099 （100字节）
请问：如果从磁带上读进一块数据并直接把它送入用户进程的工作区，会有什么问题？
- 进程速度受到外设限制
- 会影响OS交换策略，如果OS挂起该进程，虚地址1000-1099必须驻留在内存，（OS挂起进程和IO读取是不相关的）否则数据可能丢失，进程不能完整交换出内存，（进程回内存之后地址会变）可能导致**单进程死锁**
于是在内存建立缓冲区，缓存*输入设备流入内存的数据*和*内存流向输出设备的数据*

### 提前读 延后写
#### Read Ahead
用户进程从I/O缓冲区取走前一个数据后立即发出对下一个数据的输入请求；OS将在适当的时候响应该请求，把需要的数据读入I/O 缓冲区。
只要数据在缓冲区，数据就是内存到内存，比外存到内存快很多
#### Write Postponing
当用户进程请求输出数据时，OS将很快把用户进程请求输出的数据从用户进程的工作区取走，将其暂存在I/O缓冲区中； 直到用户进程指定的输出设备空闲时，OS才把暂存在I/O缓冲区中的数据写入用户进程指定的输出设备上。

### Simple Buffer
![[Screenshot 2023-11-07 at 11.49.58.png]]
- Block-oriented
- Stream-oriented

若一块数据从外部设备输入到内存所花费的时间为T，在内存中移动所花费的时间为M，被用户进程加工处理所花费的时间为C，则 
- 在没有使用I/O缓冲区的情况下，平均每块数据的处理时间近似为：$T + C$
- 在使用单I/O缓冲区的情况下，平均每块数据的处理时间近似为：$max(T,C) + M$
### Double Buffer
- 外部设备和应用进程常常交替引用 Double Buffer
- A process can transfer data to or from one buffer while the operating system empties or fills the other buffer.
- 也叫Buffer Swapping 缓冲交换
![[Screenshot 2023-11-09 at 12.04.16.png]]

### Circular Buffer
- Each individual buffer is one unit in a circular buffer.
- Used when I/O operation must keep up with process.
![[Screenshot 2023-11-09 at 10.57.51.png]]

### 怎么传递给用户进程
当用户进程请求从磁盘上读入一个扇区时， 如果系统能够在disk cache中找到该扇区的副本， 那么系统如何把该副本提交给用户进程？
- 如果不允许用户进程访问disk cache 复制一份 用了更多空间 换安全
- 允许访问 传递所需要的扇区在disk cache中的位置指针

### 置换算法
当系统需要从磁盘上读入一个扇区并在disk cache中为其建立一个副本时，如果disk cache没 有空闲空间，那么系统使用何种策略从disk cache中选择一个被置换扇区？
置换算法 基于miss rate评价性能
- LRU *Least Recently Used*
	- 局部性原理 总体优于LFU
	- stack implementation
- LFU *Frequently*
	- Cache结构和访问方式相同时，比LRU更优
	- counter 记录访问频率，但是有些快短时间大量频繁访问，所以所有扇区组成一个栈，栈顶最近访问，同时分section
		- New section 靠近栈顶
		- Old Section 靠近栈底
		- ![[Screenshot 2023-11-09 at 10.54.50.png]]
		- 置换时在old section选择count最小的
		- （其实也用到了一点LRU 和局部性原理）
	- 问题：一些bug
		- ![[Screenshot 2023-11-09 at 10.56.06.png]]
	- 改进：增加Middle Section
		- ![[Screenshot 2023-11-09 at 10.56.22.png]]

### SPOOLing 
在快速辅助存储设备中建立I/O缓冲区，用于缓存从慢速输入设备流入内存的数据，或缓存从内存流向慢速输出设备的数据。
早期提出时 内存很贵 所以在外存建立缓存
而现在的作用可能体现在云计算环境下
![[Screenshot 2023-11-09 at 11.02.01.png]]

## Disk Scheduling
磁盘调度策略

> 打印机都是先来先服务，没有调度，而磁盘的调度是有意义的，所以要研究他的调度策略

> 大容量磁盘每个磁道都有磁头，中小型磁盘设备一个盘面一个磁头，需要寻道、移动

### 时间组成
- Seek Time 寻道时间 机械移动
	- $T_{seek}=m*n+s$
	- m 与磁道移动速度相关
	- n 磁道之间的距离
	- s 磁盘启动时间
	- rpm *round per minute*
- Rotational delay/latency 旋转延迟
	- $T_r$ 由购买的硬盘确定
- Access time 寻道+旋转
	- $T_{a}=T_s+T_r+T$
- Data transfer ($T$)
	- ![[Screenshot 2023-11-09 at 11.21.38.png]]
	- $T=byte/(rotate\_speed*N)$
	- N 每个磁道上的字节数

### Rotational Position Sensing
RPS 旋转定位感知
- 磁盘驱动器发出寻道命令后便释放相关的**通道控制器**（I/O通道），以便系统用它来处理其它I/O操作。
- 当磁臂(磁头)移到指定磁道时，磁盘驱动器驱动磁盘旋转，把指定扇区的起始位置定位到磁臂(磁头)下。
- 当指定扇区的起始位置定位到磁臂(磁头)下， 磁盘驱动器重新申请通道控制器，建立到主机的通路。如果请求失败，磁盘驱动器将驱动磁盘旋转一周,再申请通道控制器。**（维持磁盘旋转的成本更低，所以要再转一圈不是停在原地） 避免复杂的控制逻辑**

### Policies
> 假定：一个硬盘的扇区长度为512个字节，磁 道长度为32个扇区，平均寻道时间为20ms，传 输速率为1MB/s，转速为3600rpm。显然，如 果一个长度为128K个字节的文件存放在该硬盘 上，那么该文件将在该硬盘上占用256个扇区。
> 请问：如果系统从该硬盘上完整地读入该文 件， 将花费多长时间？

128k 256sector扇区
256/32 = 8 磁道track
寻道20ms\*8=160ms
转速3600rpm 每圈60/3600 = 1/60s
找期望半圈8.3ms 读一圈至少转一圈16.7ms
传输速率1MB/s 传一圈的数据需要125/8=15.625ms 一个扇区15.625/32=0.5ms
- 连续存在8个磁道：
	- (20+8.3+16.7)+7\*(8.3+16.7)=220ms
- 最差：全部分散，每个扇区都要寻道+旋转
	- 256\*(20+8.3+0.5)=7373ms
	- 大量时间花在寻道上

于是需要调度算法 文件的存储方式影响效率，且当系统访问一组磁盘扇区时，如果能够减少**总的寻道时间和总的旋转延迟**，那么系统的访问效率将得到提高。
![[Screenshot 2023-11-09 at 11.31.32.png]]
- 基于请求者的磁盘调度策略
	- Random Scheduling Strategy RSS
		- 该策略性能最差。可用作评估其它磁盘调度策略
		- 为什么这里使用最差的对比？之前是用OPT来对比啊
			- 磁盘调度没有一个普遍认可的“最佳”策略。磁盘调度策略的效果受到许多因素的影响，如请求的分布、磁盘的物理结构和系统的具体需求。
	- FIFO
	- Priority-Based Strategy PBS
	- Last in, First out LIFO

> “下面这些不是让你们背，你们也一定能推出来，只不过你们晚生了五十年，不然你们也是发明者”

- 基于被请求扇区的磁盘调度策略
	- SSTF Shortest Service Time Fast
		- 总是满足当前移动距离最小的请求，但不能保证一组的总和移动距离最小，比FIFO好，但不断有新请求时可能会饥饿
	- SCAN **用的最多**
		- Arm moves in one direction only, satisfying all outstanding requests until it reaches the last track in that direction. Then, reverse its direction
		- 避免了SSTF的饿死现象
		- 磁道如果太多的话：有可能有的进程被严重推迟
	- C-SCAN
		- 只允许单向，返回时不服务，再次从头开始
		- ![[Screenshot 2023-11-14 at 10.39.18.png]]
	- N-step-SCAN
		- ![[Screenshot 2023-11-14 at 10.41.40.png]]
		- N的取值不好定
	- FSCAN
		- ![[Screenshot 2023-11-14 at 10.43.59.png]]

> “我们那一代，让不要学医，说学医要背很多，建议学计算机。结果学了计算机，知识更新很快，一直学一直学，结果学得还更多。以后你们就是学一辈子。”

## 磁盘容错技术
增加冗余部件，保证数据可靠性
System Fault Tolerance, SFT
### SFT-I
- 低级磁盘容错技术，防止磁盘表面介质缺陷引起的数据丢失
	- 最早的容错技术。
	- 双份目录、双份文件分配表
		- FAT File Allocation Table 文件分配表
		- 可在不同的磁盘上或同一磁盘的不同区域中， 分别建立维护两份文件目录和FAT
		- 当其中一个目录或FAT损坏时，系统便自动启用另一个目录和FAT，同时在磁盘的其它区域再建立新的文件目录和FAT。
		- 每当系统重新启动时，都要对这两份目录和FAT进行检查，以保证它们的一致性。
	- 热修复重定向
		- 系统将一定容量作为存放发现磁盘块有缺陷时的待写数据
	- 写后读校验
		- 这是检测坏道的方式
		- 写和读的不一致，就能说明是坏块，数据写入热修复重定向区中

> 老师学os的时候这些是重点，因为那时还没出现新的技术

### SFT-II 中级
- 防止磁盘驱动器、磁盘控制器引起的数据丢失。
	- 磁盘镜像
		- 一模一样的拷贝![[Screenshot 2023-11-14 at 11.28.30.png]]
		- 多一个磁盘驱动器
	- 磁盘双工
		- 控制器也是双份，成本更高![[Screenshot 2023-11-14 at 11.28.37.png]]
		- 这样可以加快读的速度
		- 可靠性更高，写入相比磁盘镜像是两个磁盘并行

### SFT-III 高级
- 双服务器等，一台故障还有备份
	- 热备份模式
	- 互为备份模式
		- 两台都服务
### RAID
二级容错技术
Redundant Array of Inexpensive Disks 以前是廉价磁盘冗余阵列
Redundant Array of Independent Disks 成本低了 独立磁盘冗余阵列

RAID = 磁盘阵列 + 磁盘阵列管理软件
![[Screenshot 2023-11-14 at 11.48.34.png]]
磁盘阵列管理软件屏蔽了磁盘阵列的物理细节，使OS的其它成份不知道磁盘阵列的存在；在它们看来，系统中存在一个大容量的逻辑磁盘。
也要存储校验信息，以供故障后恢复

[[Chapter 8 Interconnections and IO systems#RAID]]

#### 条带Strip粒度
- 条带可以是细粒度的（如一个字节或字）， 也可以是粗粒度的（如一个扇区或多个扇区）
- 细粒度条带，几乎每个存取请求都会同时存取RAID中的所有磁盘，使得无法同时响应多个存取请求。 只利于对单个存取请求进 行并行处理
- 粗粒度条带，不会使每个存取请求都同时存取 RAID中的所有磁盘。只利于对多个独立的存取请求进行并行处理

> Trade-off

数据校验信息：镜像存储、汉明码、奇偶校验

#### 分类
- RAID 0 读写快 不重复 安全性低
	- ![[Pasted image 20231114201854.png]]
- RAID 1 磁盘镜像 利用率只有50% 读性能较好
	- ![[Pasted image 20231114201909.png]]
- RAID 2 细粒度Strip 汉明码放在专用磁盘 能纠正1bit，检测2bit错误
	- 比raid1成本低，比raid345成本高
	- ![[Pasted image 20231114201934.png]]
- RAID 3 细粒度Strip bit-level parity 存储数据奇偶校验信息，放在专用磁盘
	- ![[Pasted image 20231114201946.png]]
- RAID 4 粗粒度Strip block-level parity
	- 小数据写入时需要读两次、写两次磁盘 Write Penalty
	- 写请求会导致访问奇偶校验盘，多个请求难以并行处理
	- Parity disk become a bottleneck
	- ![[Pasted image 20231114201954.png]]
- RAID 5 粗粒度Strip 校验位分布存储在多个
	- bottleneck in RAID4 avoided
	- block-level distributed parity
	- ![[Pasted image 20231114202000.png]]
- RAID 6 粗粒度Strip
	- Dual Redundancy 采用两种不同的奇偶校验计算,奇偶校验信息分布存储在不同块中
	- 可靠性高，但写性能低，每次写都会写两个校验块
	- ![[Pasted image 20231114202041.png]]


![[Screenshot 2023-11-14 at 20.12.33.png]]

#### 嵌套RAID
- RAID 10
- RAID 01
	- 10 01 顺序不一样
	- 让相同块 镜像更近的话 修复会更快更容易
- RAID 30
- RAID 50
	- ![[Pasted image 20231114202122.png]]
- RAID 51
	- ![[Pasted image 20231114202140.png]]
- RIAD 60
	- ![[Pasted image 20231114202219.png]]
- RAID 100
	- ![[Pasted image 20231114202236.png]]
#### RAID优点：效率高、可靠性高、性价比高