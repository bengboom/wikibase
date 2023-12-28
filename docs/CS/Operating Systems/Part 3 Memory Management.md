> 操作系统存储管理子系统设计要素
> - 是否使用虚拟存储（硬件）
> - 使用分页分段还是段页（硬件）
> - 用什么存储管理算法（软件）
## Requirements
- 划分内存满足多进程需要
- 高效 memory allocated efficiently to pack maximum processes into memory
	- 多道操作系统：共享Sharing
- Relocation 重定位：计算程序在内存中的物理位置
	- MMU *Memory Management Unit* 地址转换由硬件完成 因为高频需要很快
- Addresses
	- Logical
		- 需要MMU来转换translation
	- Relative
		- relative to some known point
	- Physical
		- absolute in memory
- Protection
	- 访问权限
	- 越界检查由硬件Processor完成而非软件OS，OS不能预料所有引用地址界限在哪
- Sharing
	- 尽量多的共享，这样内存可以装下更多进程

Page/Frame

TLB Translation Look-aside Buffer 快表 联想存储器

## Techniques
操作系统的存储管理技术到目前为止都是这三种及他们的排列组合，例如分页分段组合就是段页式
### Partitioning 分区
连续的
#### Fixed Partitioning
系统启动时将内存划分为数目、大小固定的分区，各分区大小可以不等
- Equal-size partitions
	- process size <= partition size, available
	- all partition full -> OS **swap** process
	- too big program -> **Overlay** 后一块要能覆盖前一块 对程序要求高 写程序的时候要考虑可用内存大小
	- Memory use inefficient. 所有程序至少占一个分区
- Unequal-size partitions
	- 2,4,8,16... $2^n$ size partition
- Advantages
	- 实现简单，系统开销小
- Disadvantages
	- 有内零头（小进程占用大分区，很多空白） Internal Fragmentation
	- 分区尺寸固定，无法运行大程序
	- 分区数目固定，活动进程数量有上限
#### Dynamic Partitioning
- Partition: variable length and number
- process is allocated as exactly required 理想很美满
- get holes in memory 外零头 *External Fragmentation* 因为不断**释放**有了很多不连续的空洞
- **Use Compaction** 紧凑 shift processes -> contiguous 相邻 & make all free memory in one block
- 内存空间紧凑和磁盘碎片整理一样，很花时间。内存和磁盘虽然材质不同但”都会乱“
##### Algorithm
- Best-fit algorithm 最佳适应算法
	- 通常表现最差
	- 因为最后剩很多小片段用不了，compact频繁
- First-Fit algorithm 最先
	- Simplest, best, fastest
	- **从头到尾**找到第一块满足大小要求的块，但内存头部已有的大量进程块也要被扫描
- Next-fit algorithm 下次
	- 从上一次扫描到的位置开始找
	- 但也需要一些compaction，以在内存尾部获得较大内存块
##### Registers used during execution
base, bound界限 (判断越界) start/end address of the process, in PCB
base + relative = absolute 如果越界，硬件给OS发中断
硬件实现![[Screenshot 2023-10-07 at 21.57.15.png]]
##### Buddy system 伙伴系统
结合固定分区和动态分区
space available: $2^U$
request size
- $2^{U-1} < s \leq 2^U$, allocate entire block
- otherwise, split into two equal buddies 相同大小伙伴，二分直到上面条件满足
- 最后形成一个二叉树
### Simple Paging 分页
> 分区是连续的占用空间，空间利用率低，有内外零头，于是有了分页，提高空间利用率

离散的![[Screenshot 2023-10-10 at 10.46.32.png]]
Memory -> divide to equal-size chunks, called **frames** 页框 页帧
Process -> divide to multiple same size chunks, called **pages** 页
OS use **a page table** record: frame location for each page in the process
同时也有一个single free-frame list 空闲页框表

#### Logical Physical Address Transition
made by MMU hardware
processor using page table register to access the page table of the current process
- logic address: page number, offset
- Physical address : frame number, offset

#### Translation Look-aside Buffer
> Cache 能利用起来是因为局部性原理

Virtual memory reference -> two physical memory accesses
- fetch page table
- fetch the data
Overcome -> TLB, like memory cache, contain recently used page table entries![[Screenshot 2023-10-10 at 11.24.39.png]]
计组：[[Chapter 7 Storage Hierarchy]]
> 现在硬件成本不高，牺牲存储空间来换效率是目前的主要趋势
> 页表在主存中占用连续的空间，解决方法： 多级页表 计组

#### Page size
大页：占用更多虚拟内存空间
小页：一个进程需要更多的page，页表就会更大了，可能页表也只能虚拟
但是：Secondary memory favors large page size. Secondary memory is designed to efficiently transfer large blocks of data.
Page fault rate:
![[Screenshot 2023-10-12 at 10.32.01.png]]

三种解决大页表占用大内存的方法
- Virtual Page Tables
	- 页表也存入虚拟内存，页表一次放不进主存
- 多级页表
	- ![[Screenshot 2023-10-12 at 10.42.12.png]]
- Inverted Page Tables
	- 倒置页表![[Screenshot 2023-10-12 at 10.48.35.png]]
	- 哈希冲突后用chain链接到其他地方去，地址空间大，哈希冲突多，最后地址转换效率低
	- 基本上没有系统使用倒置页表

### Simple Segmentation 分段
离散的
符合程序设计思路，模块化程序设计
程序员吧进程分割为大小不一定相同的Segment段
系统将物理内存空间分成尺寸不一定相等的Partition
Segment 在内存中不一定存储在连续的分区中
![[Screenshot 2023-10-12 at 11.01.06.png]]
#### Data Structure in Fragmentation
- Segment Table 段表
	- 描述进程分段情况，进程与物理内存之间的映射关系
	- 段号、段的物理起始地址、段长度
- Partition Table 分区表
	- 记载物理内存分区情况
#### Segment Protection
地址变换和存储保护
• 以逻辑地址中的段号为索引检索段表从而得到指定段所对应的段表项； 
• 若逻辑地址中的段内偏移大于段表项中段的长度，则产生存储保护中断 （该中断将由OS处理）； 
• 把逻辑地址中的段内偏移与段表项中的分区的起始物理地址相加从而得到物理地址

分段系统中，段表在物理内存中，段表寄存器存放**当前正在运行的进程其段表**在物理内存中的起始物理地址
![[Screenshot 2023-10-12 at 11.01.33.png]]
可以设置数据段的访问权限，针对于进程的
保护分两类
- 越界检查
	- 系统在段表寄存器中保存当前进程的段表长度并在每个段表项保存各段长度
- 存取控制
	- 为了进行，在每个段表项设置“存取控制”字段，规定进程对段的访问权限
##### 环保护
为了进行环保护，系统将在每个段表项中保存各个段所在环的特权级别。
环保护通常遵循以下保护规则
- 一个程序可以调用驻留在同级环或较高特权环中的服务
- 一个程序可以访问驻留在同级环或较低特权环中的数
- ![[Screenshot 2023-10-12 at 11.04.09.png]]

#### Paging vs. Segmentation
- Page 物理单位，在特定系统中大小不变，逻辑地址一维
- Segmentation 逻辑单位，长度不定，同一进程的两个段长度也可能不等，逻辑地址是二维或多维（每个Seg，Seg内......模块嵌套模块）
- 分页源于管理内存资源的需要，在系统内部进行，用户看不见
- 分段源于模块化程序设计的需要，在系统外部进行，由用户实施

- 分页系统中，内零头得到了有效的抑制，外零头则完全被消除；因此，使 用分页技术可以提高物理内存的利用率。
- 分段系统中，动态数据结构、程序和数据共享、程序和数据保护等问题得 到了妥善解决因此，分段技术有利于模块化程序设计。 
- 段页技术汲取了分页技术和分段技术的上述优点
### 段页式
分段与分页相结合 Combined paging and segmentation
对于程序员，进程被分为多个Segment
每个段被**系统**分为多个Page
物理内存被划分为多个Frame
当一个进程被装入物理内存时，系统为该进程的每个段的各页面独立地分 配一个Frame；一个进程的同一段的多个页面不必存放在连续的多个 Frame中
![[Screenshot 2023-10-12 at 20.12.19.png]]
在段页式访问数据中需要访问三次内存，第一次访问段表获得该段页表首地址，地二次访问页表加上便宜得到物理地址，第三次访问数据。Linux要考虑可移植性，RISC对分段支持有限。Linux用分段，给存管理更简洁。

- 分页Paging 对程序员是透明的，消除了外零头
- Segmentation 对程序员visible, allows for growing data structures, modularity, and support for sharing and protection
- Segment -> broken into fixed-size pages

> 查文献：用户视角是模块化的，怎么到内存里面是分页？在分页前要做一些事情，将用户空间*线性化？*

## Virtual Memory
一个程序是否全部装入内存
Simple:全部存入内存
Virtual:部分存入内存。没有虚拟分区，因为分区要求一次装入（后面进程太大装不下分区引入了覆盖*Overlay*技术）
Real Memory: Main Memory
Virtual memory: on disk, allow for effective multiprogram
优点：内存中可以装下更多进程，一个进程可以占用比物理内存更大的空间
使用Virtual Memory 需要硬件支持分页与分段，OS管理页和段在主存和外存的移动。 还需支持中断：缺页处理时。

> 操作系统和管理学类似，学好OS对管理有帮助 OS管理硬件资源 追求最大效率 和社会最求更高生产力也相似

#### 虚拟内存下的程序执行
操作系统将一部分程序装入内存 Resident set 驻留集 *Portion of process that is in main memory*
当要的地址不在内存中时，产生中断，进程Block，然后把那一部分装入内存（ OS issue I/O request, another process is dispatched to run, interrupt again when I/O complete, original process back to Ready） I/O与另一个进程并行
### Principle of Locality 局部性原理
- Program and data references within a process tend to cluster （簇）
- Only a few pieces of a process will be needed over a short period of time.
- Possible to make intelligent guesses about which pieces will be needed in the future.
- This suggests that **virtual memory may work efficiently**
### Thrashing 抖动
- Swapping out a piece of a process just before that piece is needed.
- The processor spends most of its time swapping pieces rather than executing user instructions.
直接原因：置换算法不好、系统负载过高（进程数太多）
### V Mem Paging
- Process -> own page table
- load bit: indicate page in/not main memory
- modify bit: if modified since last load(if write back)
![[Screenshot 2023-10-19 at 11.01.46.png]]
### V Mem Segmentation
#### Segment Tables
Entries 条目
V Address: Segment Number + Offset
Segment Table Entry
P+M+Other Control bits + Length + Segment Base
#### Software Manage Techniques
##### Resident Set Management 驻留集管理
Resident Set  指进程驻留在内存中的页面所组成的集合。
每个活动进程驻留集尺寸应该多大？
- 给每个活动进程分配的frame越少：驻留活动更多，调度就绪进程多
- frame越少：每个进程发生缺页中断概率越大
- 分配给每个活动进程frame数到达一定程度时，缺页率不再显著下降
- ![[Screenshot 2023-10-19 at 19.55.18.png]]
工作集：工作所需的尺寸大小
分配方法
- fixed-allocation
	- 固定的页框数
	- 缺页时必需替换
- variable-allocation
	- 调整驻留集尺寸
	- 工作集方法
		- 工作集$w(t,\Delta)$是指在$t$之前的一段时间$\Delta$内该进程所引用的全部页面的集合，是关于时间的函数，$\Delta$是窗口尺寸，尺寸越大工作集尺寸越大
		- 进程有效运行：任何时候进程的工作集都在内存中
		- 监控造成的开销大，且只能监控历史记录，对未来的预测未必准确，窗口尺寸难以确定
	- PFF *Page Fault Frequency* 缺页率算法
		- 如果一个进程的缺页率低于某个最小阈值，那么系统将减小该进程的驻留集尺寸。
		- 如果一个进程的缺页率高于某个最大阈值，那么系统将增加该进程的驻留集尺寸。 
		- 因为缺页率与缺页时间间隔成反比，因此，可以通过测量缺页时间间隔来监控缺页率。
##### Placement Policy 放置策略
系统在内存什么位置为进程分配frame
##### Fetch Policy 获取策略
when a page be brought into memory?
- Demand Paging
	- 在进程开始时有较多缺页fault
- Pre-paging 预调页
	- bring in more pages than needed, 开始运行时已经装入
- 要明确获取策略才能讨论缺页次数（程序开始载入时是否算缺页，预调页不算）
##### Replacement Policy 置换策略
在什么范围内判断有无空闲frame?没有空闲移出哪个页面？
- Variable Allocation, Global Scope
	- 可变分配 全局置换 
	- 大多数OS采用 易实现
- Variable Allocation，Local Scope
	- 可变分配 局部置换
- 固定分配则必须使用局部置换，全局置换必须使用可变分配
- 可变和局部可以组合

Frame Locking: 锁住frame不能被replaced 手机上的后台程序上锁可驻留

Basic Replacement Algorithms
- Optimal Algorithm *OPT*
	- 置换在将来不会访问/在最远的将来才会访问的页面
	- 不可能预知未来，仅能作为算法性能参照标准
- Least Recently Used Algorithm
	- 可以实现但开销大，OS通常使用近似LRU算法
- FIFO Algorithm
	- 简单，但对于有局部存储引用特征的程序（大多数），FIFO性能较差
	- 易产生抖动和Belady现象
	- **Belady现象：虚拟存储系统中的一种异常现象，即增加进程的页框数，缺页率反而上升**
	- OPT和LRU不会导致Belady,其他的算法都可以构造出出现Belady的实例
- Clock Algorithm
	- 为page新增Use bit, 首次加载为1，被访问也置1
	- 第一个Use bit=0的page被replace，环形搜索过程中碰到的1均置0
	- 系统将置换范围内的所有frame 组成一个环形缓冲区，并为其设置一个扫描指针。
	- 没有进行页面置换时，扫描指针总是指向上一次进行页面置换时被置换页面所在位置的下一个位置
	- 画的时候，用\*号表示Use bit=1
	- ![[Screenshot 2023-10-24 at 11.24.32.png]]
- 改进Clock Algorithm
	- 考虑写回的开销
	- 系统把一个页面移出内存时，如果该页面驻留内存期间没有被修改过，那么不必把它写回辅存，否则系统必须把它写回辅存。这表明，换出未修改过的页面比换出被修改过的页面开销小。
	- 改进后的CLOCK算法将在置换范围内首选在最近没有使用过、没有被修改过的页面作为被置换页面。
	- 为什么优选Usebit而不是Modifybit--是Usebit体现了局部性原理，是最核心的考虑因素

移出时如果该页已修改：移到modified page list/buffer
##### Cleaning Policy 清除策略
何时将已修改的页面写回辅助存储器？
- Demand Cleaning
	- 仅在移出时写回
- Pre-cleaning

Best Approach: Page Buffering
- placed in two lists
	- modified
		- periodically written out in batches
	- unmodified
##### Load Control 负载控制
工作集方法、PFF方法蕴含着负载控制
- L-S principle
	- 发生页面失败的平均时间=处理页面失败的平均时间 此时CPU利用率最大
- 50% principle
	- 分页设施利用率在50%左右
- ![[Screenshot 2023-10-24 at 11.48.34.png]]

Process Suspension
[[Part 2 Processes and Scheduling]]
可以考虑
- lowest priority process
- faulting process
- last process activated
- process with smallest resident set
	- reload easy
- largest process
	- obtain most space
- process with the largest remaining execution window
