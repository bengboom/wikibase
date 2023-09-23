## 20230424
#### FPGA *Field Programmable Gate Array* 现场可编程门阵列

## Memory 
Programmable

#### ROM *Read Only Memory*
Fuse 融化 保险丝
将二极管用很大电流融化
融化/不融化 表示1/0
-> aiming for reversable

#### PLD *Programmable Logic Devices*
#### PROM *Programmable ROM*
hard wired AND-array
programmable OR-array ( 选择是否连接到OR gate 上 )
**图上的叉叉‘X’表示可以选择是否连接**

-> more programmable array
- PAL *Programmable Array Logic*
- PLA *Programmable Logic Array*
	- Programmable AND-array *PAL* + OR-array
- GAL *Generic Array Logic*
	- EECMOS Programmable AND-array 
	- *Electrically Erasable Complementary Metal Oxide Semiconductor*
	- OLMC *Output Logic Management Complement*
		- 给与门求和，加FIFO MUX 输出+反馈
		- 可以搭建FSM
- CPLD *Complex Programmable Logic Devices*
	- LB *Logic Block*  
		- 多个macrocell
			- 基本的组合逻辑 PAL/GAL
			- 一些额外的辅助元件 如产生加法进位链的XOR MUX等
	- Programmable Interconnect Matrix 可编程内部交换网络
	- 上述两个相连，构成交叉网络 外部与IO control block相接

#### EEPROM *Electrically Erasable PROM*

## FPGA
各种Block, CLB IOB DCM RAM MULT 通过switch matrix连接起来
像电话交换器一样（计网switching fibre）速度高
输入输出之间有一个晶体管控制导通，预置很多线，Gate端连SRAM Gate端电平决定是否连上两点
配置SRAM可改变组件间连接方式
#### CLB *Configurable Logic Block*
没用Programmable Array, 用LUT, 存1bit 6transistors
有加法进位链 可以写行为级加法 直接利用硬件加法链
#### Block Select RAM

#### Multiplier
硬件乘法器

#### IOB *IO Block*
can be configured to have various electronic charateristics
IOB是双向的 tristate buffer
programmable
PAD
Input/Output Register 输入输出寄存器 可以暂存待发送、正接受的数据
用FPGA EDA MAP功能 把最后一级reg放入IOB 加快速度
#### PCR *Programmable CLock Resource*
等长 金线
PLL *Phase-Locked Loop* 
DLL *Delay-Locked Loop* 
锁相环
#### DCM *Digital Clock Management*

#### GCM *Global Clock Mux*


## 20230426
**FPGA Resource Limitation** 
IO CLK LTU *Look-up Table Unit* B*lock*RAM DSP

**增量设计** 在fpga里微小改动不会更改过多synthesis的时间


#### HDL *Hardware Description Language*
Descriptive!
并发性 HDL时序 Parallel Concurrency
Timing schedule
HDLs are RTL *Register Transfer Level* 想到组合逻辑+寄存器的电路结构
They have many layers *Behavioral Gate ?*

国内主要用Verilog，VHDL看懂的人不多 天然的加密
> 老师源代码随便给 注释写在另一个文件里面 别人看懂的时候自己写也能写出来了

美国人写VHDL

VHDL *Very High speed asic Description Language*
floor planning 布线 -> Place and Route -> Avoid Timing Violation

**课程设计 MCU 可以一个人专门研究Floor Planning

除非特别需要 不要用latch

用括号改变优先级 增加并行

nesting 嵌套

DDS 直接数字序列合成 Direct Digital Synthesis


#### Basic Concepts
Interfaces Behavior Structure TestBenches Analysis Elaboration Simulation Synthesis


#### 设计过程
![[Pasted image 20230427193206.png]]

component = entity ***architecture declaration*** + interface ***entity declaration***


Hierarchy in HDL
![[Pasted image 20230427170040.png]]





