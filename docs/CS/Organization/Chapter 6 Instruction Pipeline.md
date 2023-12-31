## 流水
- instruction fetch
- reg/instruction decode
- execute
- memory
- write register

书上的标准称呼
Ifetch Reg/Dec Exec Mem Wr

什么样的指令集有利于做流水线？
- 指令长度一致
- 格式少 源寄存器位置相同
- 只有Load Store可访问存储器 专用访存指令
- 内存中指令**对齐存放** 减少访存次数
规整简单一致性有利于流水线设计

## 流水线冒险 *Pipeline hazard*
### 结构冒险
一个硬件同时被不同指令使用
解决方式：
- 每个功能部件每条指令只能用一次（如：写口不能用两次或以上）
- 每个功能部件必须在相同的阶段被使用（如：写口总是在第五阶段被使用）
- 这样不访问内存的R指令在内存那一步流水为NOP，读内存的写寄存器阶段为NOP
- 分开Instruction memory and data memory

### 数据冒险 *Data Hazard*
后面指令用到数据的前面指令的结构还没有产生

#### 解决办法
方法1 stall
	硬件阻塞，比较复杂，also called "add bubble"，清零
方法2 插入NOP
	简单，但效率低
方法3 合理寄存器读取
	能解决部分冒险。寄存器读写不冲突，前半时钟写，后半时钟读。
方法4 转发 Forwarding and bypassing
	需要旁路转发单元
方法5 编译优化

### 控制冒险
当遇到改变指令执行顺序的转移指令（调用、返回等）、异常和中断情况时，在形成转移目的地址之前，流水线中已取了后续指令并在执行，这时就需要清除流水线中的部分指令的执行。

#### 解决办法
- 硬件阻塞 stall
- 软件NOP
- 分支预测 效率最高
	- 预测准确率通过各种方式能够达到90%
- 延迟分支 *Delayed branch*
	- 把分支指令前面与分支指令无关的指令调到分支指令后执行，也称延迟转移