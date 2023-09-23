## General
cell 记忆单元 存储基元 位元 双稳态器件
addressing unit 存储单元
bank 存储体、矩阵、阵列 由大量记忆单元构成
addressing mode 编址方式 字节/字
MAR *Memory Address Register* 用于访问的主存单元的地址的寄存器
MDR/MBR *Memory Data/Buffer Register* 存储器数据寄存器

## Classification
- 读取性质
RAM *Random Access Memory* 每个单元的读取时间都相同 内存
SAM *Sequential Access Memory* 磁带
DAM *Direct Access Memory* 直接存取 磁盘 直接定位到读写数据块，在读写数据块时按顺序进行
AM/CAM *Associate Memory / Content Addressed Memory* 检索 快表
- 材质
	- 半导体
		- 读写
			- SRAM cache 六管静态 Static Random Access Memory
				- 双稳态触发器![[Pasted image 20230508095418.png]]
				- 读写原理 结构图
			- DRAM 主存  Dynamic Random Access Memory
				- 动态单管记忆单元
				- 结构图![[Screenshot 2023-05-08 at 09.54.47.png]]
				- 读写原理 字线 位线
				- **破坏性读出** 读完之后数据就没了 还需要重新写入
				- **电容漏电** 定时补充操作 **刷新Reflash，即为读操作** 按行进行 周期10ms-100ms
				- 多种刷新方式
					- 集中（全统一 有死区） 不适合实时
					- 分散（每次读写都刷） 效率较低
					- 异步（按行轮流）行数\*刷新周期=数据有效期
					- ![[Pasted image 20230611193536.png]]
				- SDRAM *Synchronous* 与CPU之间的通信同步的存储芯片
					- SDRAM的突发传送：SDRAM允许在一个命令周期内对连续的地址进行一系列的读/写操作，这种模式称为突发传输模式（Burst Mode）。这种模式可以提高内存访问的效率，特别是对于需要读取或写入大量连续地址的应用非常有利，如图像和视频处理等。
			- 一个单元的SRAM 晶体管数量是 DRAM 六倍 但造价高100倍
		- 只读 ROM
			- 不可在线修改内容
			- Mask ROM
			- PROM EPROM EEPROM
			- Flash Memory![[Pasted image 20230509200233.png]]
				- 擦写
				- 编程
				- 读取
	- 磁表面
	- 光
- 可更改性
ReadWrite ReadOnly
- 断电可保存
非易失性（不挥发） nonvolatile memory
- 速度
Register Cache Main/PrimaryMemory 


## CPU与主存及存储器容量扩展
#### 存储器容量扩展
字扩展
位拓展
字位同时扩展

#### 通信方式
- 异步
	- 需要握手过程 cpu会等待 直到收到*完成*信号
- 同步
	- 用统一的时钟信号 一定会在几个时钟周期内读到

#### 连接方式
内存条，把多个DRAM芯片焊在一小块电路板上
DDR2-DDR5
双列直插式
同时需要Cache来加速

## Cache 高速缓存
Cache是透明的，高级语言看不到，但了解Cache后可以写出更好的程序
主存块与Cache行大小相等

有Cache的情况下数据的访问过程：
Cache有数据->easy
Cache无数据->主存送CPU，Cache也进行缓存

有效位的作用：有效位为1则数据有效，为0则无效，刷新冲刷时（DMA,进程切换）操作系统控制全清零

#### Cache mapping 映射
> 把访问的局部主存区域（块）取到Cache中时，该放到Cache的何处.
> Cache行比主存块少，故存在多个主存块映射到同一个Cache行

Block Slot Line Block Page....映射行大小相等 不一定是Byte 而最终地址要到Byte级
	块内地址*Block Offset*

##### 直接映射 *Direct Mapping*
不够灵活 每个主存块只能映射到某一行
类似取模又叫模映射 *Module Mapping*
内存地址组成：主存群号(群大小=Cache大小）+cache**行**号（块号）+块内地址
Cache：每行：有效位+标记+数据，标记存主存群号
##### 组相联映射 *Set Associative*
上下两种相结合
组间是模映射，组内是全映射
路:cache每组的行数
结合了直接映射和全相联映射的优点
主存地址划分为：标记*tag*+Cache**组**号+块内地址
而cache中则需要：标记+数据
![[Pasted image 20230603122127.png]]
##### 全相联映射
Full Associative mapping 均可映射 一个主存块可装入任何一行
这样就要存地址 cache的标记部分较长
cache：标记（主存块号）+块内地址
主存：只有块号
![[Pasted image 20230603122135.png]]

#### Replacement 替换算法
需求：当前Cache已有数据 直接映射没有替换策略
##### FIFO *First in First out*
不是一种栈算法，命中率没有显著提升
##### LRU *Least Recently Used*
是一种栈算法，命中率提升
是通过LRU位的计数器实现frequency的记录
cache中就需要LRU位，LRU位宽和路数有关,log2(路数)
##### LFU *Least Freqently Used*
LFU的工作原理是维护一个频率计数，每当数据项被访问时，该数据项的频率计数就会增加。当需要替换数据项以释放缓存空间时，LFU策略会选择频率计数最低的数据项进行替换。
##### Random
在实际测试中发现，成本最低的随机算法只稍逊于基于使用情况的算法

#### Write Policy 写策略 
这里指CPU想要往主存里写
为了使主存与Cache数据保持一致，修改的时候都要修改
- Write Hit 写命中 要写的单元已经在Cache中
	- Write back 只写cache不写主存
	- 同时写cache和主存 Write through  -> CPI增加，机器变慢 -> 再加write buffer
	- write through 没有dirty位 back需要 被替换时若更改则写回
- Write Miss 写未命中
	- Write allocate 写分配
		- 先将主存块中相应单元更新，然后将主存块装入Cache。
	- not write allocate 不写分配 
		- 直接写主存单元，不把主存块装入到Cache。缺点 未利用空间局部性。
**空间局部性：在某个时间点访问了某个内存位置，那么在不久的将来，可能会访问到物理地址上相邻的内存位置的原理。**

处理Cache读比Cache写容易，故指令Cache比数据Cache容易设计。

## 虚拟存储器 Virtual Memory
写程序要考虑到真正的内存关系 太麻烦了 于是提出虚拟地址空间 它与物理内存有自动的映射
虚拟地址空间在磁盘上 
将地址空间划分成4K大小的区间，装入内存的总是其中的一个区间（页框、实页、物理页）
程序也是分为很多的程序块（页、虚页、逻辑页）可以不连续
分页 PageFile， 操作系统为进程生成PageTable页表（映射逻辑和物理地址 *Address Mapping*
虚拟页VP *Virtual Page* 物理也PP *Physical Page* PF *Page Frame*

**逻辑地址（Logical Address）**：程序中指令所用地址(进程所在地址空间)，也称为虚拟地址（Virtual Address）
**物理地址（Physical Address）**：存放指令或数据的实际内存地址，也称为实地址、主存地址。

逻辑地址和虚拟地址之间的转换由CPU的MMU *Memory Management Unit* 实现
虚拟存储器的管理由操作系统实现，要处理很多问题。
**主存（内存）和磁盘之间** 由于这个特点，主存速度更慢
采用**大页面 全相联** 用软件*操作系统*处理缺页
只用WriteBack写磁盘太慢 地址转换用硬件*更快*

#### 页表结构 *Page Table*
![[Pasted image 20230518214500.png]]
页表存在主存，页表首址记录在页表基址寄存器中。
修改位作用：判断是否需要写回磁盘
各进程有相同虚拟空间，故理论上一样。实际大小看具体实现方式，如“空洞”页面如何处理等
页表没有虚页号：隐藏给出，顺序就是虚页号。位置（索引）在页表中已经隐式地给出了虚拟页号。

更高级的 多级页表等结构

- 未分配页  进程的虚拟地址空间中“空洞”对应的页 NULL
- 已分配的缓存页 有内容对应并已装入主存的页
- 已分配的未缓存页  有内容对应但未装入主存的页 还在磁盘中
![[Pasted image 20230611195032.png]]


#### 异常处理
##### 缺页 page fault
Valid bit = 0
调出操作系统中的缺页异常处理程序，从磁盘读页到内存。
若内存没有空间，则还要从内存选择一页替换到磁盘上，替换算法类似于Cache。
淘汰时，根据修改位“dirty”确定是否要写磁盘，即采用回写法。

##### 保护/访问违例 protection violation fault
产生条件： 当Access Rights (存取权限)与所指定的具体操作不相符时
相应处理：在屏幕上显示“内存保护错”或“访问违例”信息
当前指令的执行被阻塞，当前进程被终止
Access Rights (存取权限)可能的取值有:R = Read-only,  R/W = read/write,  X = execute only

#### TLB *Translation Lookaside Buffer*
为减少访存次数，把经常要查的页表项放到Cache中，这种在Cache中的页表项组成的页表称为TLB（Translation Lookaside Buffer） ，中文一般称为**快表**，或页表缓冲。放在主存中的完整页就叫慢表。存在cache中，且通常L1
快表与页表采用组相连或全相联映射![[Pasted image 20230611195840.png]]
>**The first level of caching for the translation table information is an L1 TLB**, implemented on each of the instruction and data sides.

关于Cache分级：

| Feature | L1 | L2 | L3 |
| --- | --- | --- | --- |
| **Speed** | Fastest | Medium | Slowest |
| **Size** | Smallest | Medium | Largest |
| **Sharing** | Not Shared | Not Shared | Shared among CPU cores |
| **Data, Instruction Cache** | Both Data and Instruction | Data only | Data only |
| **Location** | In CPU core | In chip, outside CPU core | Outside chip |



### 虚拟地址，通过TLB、页表、cache，最终转换为实地址的查找转换过程
![[Screenshot 2023-05-18 at 20.33.39.png]]
![[Pasted image 20230518215053.png]]
TLB缺失可由硬件也可由OS处理
Cache缺失由硬件处理
缺页由OS处理（缺页异常）

### 三种虚拟存储器实现方式
- 分页式
	- 如上
- 分段式
	- 程序员或OS将程序模块或数据模块分配给不同的主存段，一个大程序有多个代码段和多个数据段构成，是按照程序的逻辑结构划分而成的多个相对独立的部分。
	- 段通常带有段名或基地址，便于编写程序、编译器优化和操作系统调度管理
	- 分段系统将主存空间按实际程序中的段来划分，每个段在主存中的位置记录在段表中，并附以“段长”项
	- 段表由段表项组成，段表本身也是主存中的一个可再定位段
	- 段的分界与程序的分界自然对应，逻辑独立，段长度可变，段间会有碎片
- 段页式
	- 上面两种结合
	 - 程序先分段，再分页，段的长度必须是页长的整数倍
	 - 结合优点，但需要查两次表

### 个人理解
这一节，引出了虚拟存储器的概念，从而扩大地址空间。
地址空间扩大后，要实现与物理地址的转换，于是需要页表，加速查表需要TLB，这两个由操作系统管理，分别存在内存和TLB中。
得到物理位置后，再进行常规的访问操作，先找Cache，不行再去主存。


> 存储器的层次结构打破了木桶原理
> 这是怎么做到的： **空间局部性**
> 好神奇 计算机的速度取决于最快的而不是最慢的
> 因为短时间内只会用到一点东西进行计算
> 一台电脑有太多程序不会同时用，一个程序有太多功能不会同时启用

