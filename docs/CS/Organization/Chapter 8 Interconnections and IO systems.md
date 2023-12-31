## IO devices and harddisk

### Performance Criteria
Throughput:  I/O bandwidth
Response time: Latency

### Function
解决各种形式信息的输入和输出。用户将各种形式的信息（文字、图表、声音、视频等）通过不同的外设输入到计算机中，以及计算机内部处理的结果信息如何通过相应的外设输出给用户。

### 外设
分类略
#### 外设的通用模型
![[Screenshot 2023-05-18 at 21.33.29.png]]
#### 外存
##### 磁盘
电磁转换 N-S磁极
磁道 扇区 密度提升 增加盘片数
硬盘=硬盘控制器+硬盘驱动器+磁记录介质
DMA *Direct Memory Access*与主存直接交换数据，不经过CPU，采用专用芯片

- 低密度磁盘 各个磁道扇区数相同 内磁道密度更高。
- 高密度磁盘 各个磁道上的位密度相同，各磁道存储的数据量不同，外磁道上的扇区数比内磁道多。高密度磁盘比低密度磁盘容量高很多。

主要技术指标
- 磁盘容量
	- 未格式化 包括间歇 标记（ID域） 数据域等
		- 对于低密度容量计算，总容量=记录面数x柱面数x内圆周长（最小的）x位密度（最高的）
	- 格式化 只考虑数据区 实际能存储多少数据
		- 磁盘数据量=2x 盘片数 x 磁道数/面 x 扇区数/磁道 x 512/扇区 
- 平均存取时间
	- T = 平均寻道时间5ms + 平均旋转等待时间5ms（与转速有关） + 数据传输时间（一般忽略不计）
 
##### RAID
冗余磁盘阵列 *Redundant Arrays of Inexpensive Disk*
与SLED *Single Large Expensive* 对立
**增加容量 提高速度 提升稳定性**

| RAID级别 | 冗余性 | 性能 | 容量利用率 | 成本 | 奇偶校验数量 | 冗余数据存储位置 |
|---------|------|------|----------|------|--------------|-----------------|
| RAID 0  | 无   | 高   | 高       | 低   | 无           | 无              |
| RAID 1  | 高   | 读取高、写入低 | 50%（原始容量的一半） | 中等 | 无           | 镜像驱动器上     |
| RAID 5  | 中等 | 高（读取）、中等（写入） | （驱动器数-1）x最小驱动器容量 | 中等 | 1个校验位     | 不同驱动器上     |
| RAID 6  | 高   | 中等   | （驱动器数-2）x最小驱动器容量 | 高   | 2个校验位     | 不同驱动器上     |
| RAID 10 | 高   | 高   | （驱动器数/2）x最小驱动器容量 | 高   | 无           | 镜像驱动器上     |


##### SSD
_Solid State Disk_
NAND 读比写快，这是闪存（Flash）的特性。早期的SSD被设计成模拟硬盘，例如使用USB，SATA（_Serial ATA_ *Advanced Technology Attachment*），以及IDE（_Integrated Drive Electronics，一种旧式、宽大的接口_）。现在，SSD已经有了自己专用的接口，如M.2(**M.2**, pronounced _m dot two and formerly known as the **Next Generation Form Factor** (**NGFF**))和PCIe（Peripheral Component Interconnect Express_）。
> M.2只是一种接口，支持PCIe，SATA，USB等协议
> PCIe USB既是接口又是协议
> NVME *Non-Volatile Memory Express* 协议 为SSD设计

## Bus, interconnection, ports
### Buses
![[Screenshot 2023-05-25 at 20.02.25.png]]
#### 概念
总线裁决：多设备共用时需要决定谁使用
总线定时：同步和异步
并行与串行：总线中同时传输一位还是多位
设计趋势 **点对点 异步 串行**
#### 性能指标
- 总线宽度 数据线条数
- 总线工作频率 工作频率不一定是时钟频率，一个时钟可以传多次数据
- 总线带宽 最大传输速率 B=Width\*Freqclk/N N一次传输所用周期数
- 总线传送方式
	- 非突发：每个数据均带地址
	- 突发：传一次地址，后续地址默认为增量，只传数据
#### 芯片内总线
CPU内
#### 系统总线
> Intel公司在推出845、850等芯片组时，对“System Bus”有专门的定义，将处理器总线称为前端总线(Front Bus)或系统总线
##### 单总线结构
CPU MM IO卡 通过底板总线 *Backplane Bus*连接 称为*Industry Bus*标准总线
##### 多总线结构
用局部总线互联。
##### 组成
- 数据线 *Data bus*
- 地址线 *Address*
- 控制线 *Control*
##### 处理器总线 CPU和北桥芯片之间连接 
- FSB *Front Side Bus* 较老
	- 早期Intel架构中采用
- QPI *Quick Path Interconnect* 
	- 目前Intel使用
	- 现在CPU直连Memory不经过北桥，因CPU集成了主存控制器
	- QPI: 一个周期传两次 二十个数据线双向传16数据4校验 两方时钟独立
> 注意频率赫兹到带宽（字节）有除8的转换
> 注意双向传输带宽乘2
#### 通信总线
USB SCSI *Small Computer System Interface* 等，与外部连接 IO总线
- 第一代 EISA VESA 早被淘汰 *Extended Industry Standard Architecture*
- 第二代 PCI AGP PCI-X 也被淘汰 *Peripheral Component Interconnection*
- 第三代 PCI-E 主流 通路数量~16
	-  PCI: [地址总线](https://baike.baidu.com/item/%E5%9C%B0%E5%9D%80%E6%80%BB%E7%BA%BF?fromModule=lemma_inlink)与[数据总线](https://baike.baidu.com/item/%E6%95%B0%E6%8D%AE%E6%80%BB%E7%BA%BF?fromModule=lemma_inlink)是[分时复用](https://baike.baidu.com/item/%E5%88%86%E6%97%B6%E5%A4%8D%E7%94%A8?fromModule=lemma_inlink)，总线宽度有32位和64位
	- PCI-E 1.0 2.5Gb/s 双向 每个字节被转换为10bit
	- PCIE1.0\*n 带宽 $2.5Gb/s*2*n/10=0.5n GB/s$
现在不再有南桥北桥了
![[Screenshot 2023-05-25 at 20.26.50.png]]
DMI *Direct Media Interface*

> 不同设备速度不一样 通过设计多种总线 使得整体效率不受最慢设备限制
> 再次消灭木桶原理

## I/O 
IO接口=IO控制器+IO插座，功能有
- 数据缓冲 速度匹配
- 错误或状态检测 提供状态寄存器
- 控制和定时
- 数据格式转换
- 与主机和设备通信 在两者之间
![[Pasted image 20230611210759.png]]
将I/O控制器中CPU能够访问的各类寄存器称为I/O端口

### I/O端口编址方式
- 统一编址方式 映射到主存某区域
	- 无需设置专门I/O指令，只要用一般访存指令就可存取I/O端口。
- 独立编址方式 独立地址空间 特殊IO指令方式

### I/O数据传送控制方式
#### 轮询方式
程序控制
最简单 I/O设备（包括I/O接口）将自己的状态放到一个状态寄存器中 OS阶段性地查询状态寄存器中的特定状态，以决定下一步动作

探寻：CPU等待外设响应，中间原地踏步，效率慢
“探询”期间，可一直不断查询（独占查询），也可定时查询（需保证数据不丢失！）
> 定时查询像是：定个闹钟，先去做别的，等会过来收数据，但数据必须在该时刻到达，否则碰不上就丢了。

#### 程序中断方式
中断驱动 外部IO设备产生中断信号通知CPU，CPU调出OS来处理
程序切换 中断响应由硬件完成 中断隐指令
中断发生时，处理器要：
- 保护断点和程序状态 先行段 准备
	- 保护断点：保存PC和 PSWR *Program State Word Register*,如Flag
- 识别异常（中断）事件，并转到具体的中断处理程序
	1. 软件识别 操作系统实现 MIPS
	2. 硬件识别(向量中断方式) 硬件电路实现  中断控制器 80x86 使用中断向量表，根据中断类型，得到一个向量，查表得到对应中断服务程序的地址
还要：0. 关中断（禁止中断）即：将中断允许（触发器）标志清0
最后：结束段 恢复 恢复现场，中断返回 处理完一个中断 总是返回到处理这个中断前的状态
全过程：
![[Screenshot 2023-05-30 at 20.41.04.png]]
屏蔽词：有点像one-hot独热码，每个中断，屏蔽对应等级的中断，则为1，例如优先级1>2>3>4时：
![[Pasted image 20230530204729.png]]


8086中断
内部中断：溢出 除数为0
外部中断：INTR *Interrupt Request* 可屏蔽（外设中断），NMI *Non-Maskable Interrupt* 不可屏蔽（更重要的，紧急硬件故障）

**多重中断** 新的中断优先级**高于** ***（等于不行）*** 当前正在执行的中断，**中断嵌套**
- 中断**响应**优先级
	- 由查询**程序**或**硬件**线路决定
- 中断**处理**优先级
	- 由中断屏蔽字动态设定，可以动态改变**屏蔽字**
- ![[Screenshot 2023-06-01 at 20.02.03.png]]
> 中断服务程序比外设查询程序（轮询）长，因为有保护现场、屏蔽字等操作

#### 直接存储器访问方式 DMA
磁盘等高速外设特有，批量与主存数据交换，有专用DMA控制器
外设 -> DMA控制器 -> 请求CPU总线 -> CPU bus idle -> DMA传输
DMA开销远小于中断
![[Pasted image 20230601202928.png]]
磁盘寻道和旋转是中断方式给CPU,连续读写是DMA,最后结束和校验是中断信号。
如果不马上响应DMA请求，高速设备可能会发生数据丢失，所以，DMA的总线优先权比CPU高。

冲突时
- CPU停止法 成组传送
	- 影响了CPU工作
	- 改进 ：外设->缓存->DMA 等会再占总线 **主要使用**
	- ![[Pasted image 20230604154842.png]]
- 周期挪用（窃取）Cycle stealing法 单字传送 *闪现* 比例不一定是2:1
	- Cycle stealing is used to transfer data on the system bus
	- ![[Pasted image 20230604154855.png]]
- 交替分时访问
	- 每个存储周期分成两个时间片，一个给CPU，一个给DMA
	- 意义不大 很少用

**DMA与中断比较**
DMA只能传数据，不能处理CPU异常
中断响应在一个指令周期结束后；而DMA响应是在一个总线周期后
DMA方式用于高速设备；而中断方式用于慢速设备
DMA方式下，外设与CPU并行度高（互相影响干扰小）；而中断方式下，外设与CPU并行度低（直接打扰）

> 计算题：
> 通过总速率除以单次处理的数据量得到处理次数（中断次数、DMA处理次数）
> 再用次数乘以中断服务程序长度、DMA程序长度
> 最后得到周期数，根据时钟频率得到时间
> 占用率：处理IO的时间/总时间，IO更慢，可以取1s，乘速度得总量，再用处理这么多数据的时间除以1秒即是占用率