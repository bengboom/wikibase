## 20230330 指令系统设计
IS Instruction Set
ISA *Instruction Set Architecture*
Design:
Instruction = opCode + opVal (Src, Dst) + Next Instruction Address

###### Opcode 操作码
定长、变长
地址码字段：0个（空操作/默认地址）、1个（取反、另一运算符默认）、2个（其中一个存结果，CISC）、3个（RISC）、多个（矩阵运算，SIMD *Single Instruction Multiple Data* ）   
**OP A1 A2 A3**

***Instruction Fetch,Decode, Operand Fetch, Execute, Result Store, Next Instruction***

指令设计的基本准则（不重复，尽量短，字节整数倍，规整，操作码位数足够）

四种指令即可编制任何程序：**L**oad**D**ata **S**ave**T**o **INC**rease **BR**a**N**ch

4 bits is a nibble（半字节，一个十六进制数字）
8 bits is a byte
16 bits is a half-word
32 bits is a word
（字长=32bits的情况 不同机器字长规定不同）

## 20230404
Addressing Modes 寻址方式：找到指令或操作数的方法
EA 有效地址
基本寻址方式：
A是指令里的一段部分
- 立即 立即数给出值
- 直接方式 有效地址=A 有效地址范围小
- 间接方式 有效地址=（A）内存A单元存放了要的操作数的地址  要多次访问存储器 有效地址范围大
- 寄存器方式 操作数=（R) 数是寄存器里存的值
- 寄存器间接方式 EA=（R) 寄存器存了操作数在主存中的地址
- 偏移方式 EA=A+(R) 寄存器值+指令值的偏移 例如PC跳转
	- R可以明显给出也可以隐含给出
	- 相对寻址 EA=A+(PC)
	- 基址寻址 EA=A+(B) B为基址寄存器
	- 变址寻址 EA=A+(I) I为首指寄存器
- 堆栈方式 操作数是栈顶元素

**寻址方式由OP决定**

OP R A ...
R:寄存器编号 A地址   （100），表示100号单元里的内容（间接）
各种寻址方式的有效地址 优缺点
定长操作码编码Fixed Length Opcodes  指令的**操作码部分**采用固定长度的编码
变长操作码编码Expanding Opcodes **将操作码的编码长度分成几种固定长的格式**  常用
![[Screenshot 2023-06-11 at 17.19.00 1.png]]

条件转移指令：产生条件码 根据Flag 条件执行指令

## 20230406

指令系统设计风格
- 累加器型 其中一个操作数（源操作数1）和目的操作数总在累加器中
- 堆栈型 总是将栈顶两个操作数进行运算，指令无需指定操作数地址
- 通用储存器型 操作数可以是寄存器或存储器数据
- 装入/储存型 运算指令的操作数只能是寄存器数据，只有load/store能访问存储器
后两种流行 主要

CISC RISC 特点及对比，指令的二八定律，第一代RISC:MIPS
>MIPS (Microprocessor without Interlocked Pipelined Stages)

现在还有VLIW *Very Long Instruction Word* 
> 简化处理器结构，删除复杂的控制器电路，每时钟周期可运行20条指令，而CISC通常只能运行1-3条指令，RISC能运行4条指令

## 程序表示MIPS
[UCB MIPS helper](https://inst.eecs.berkeley.edu/~cs61c/resources/MIPS_help.html)
[MIPS instruction formats](https://max.cs.kzoo.edu/cs230/Resources/MIPS/MachineXL/InstructionFormats.html)
[[MIPS]] 

### 寻址方式
立即数寻址 / 寄存器寻址 / PC相对寻址 / 伪直接寻址 / 偏移寻址
![[Pasted image 20230611191510.png]]

### 指令分类
#### R type
**R**egister: op6 **rs5 rt5 rd5** shamt5 func6 32个寄存器用5位刚好
R型指令的op全为0，具体功能由func部分确定
rs:source, t just after s in alphabet, rd:dest.reg. 
MIPS汇编语言要求，rd写前面。 add rd rs rt  == rd$\leftarrow$(rs)+(rt)
jr:jump register
shamt: shift amount

#### I type
**I**mmediate:op6 **rs5 rt5** immediate16,指令执行时扩展为32bits
op不能全为0
rs源操作数 基址寄存器
rt目的寄存器
im根据指令，可用于充当运算数或寻址（偏移寻址、相对寻址）
**I指令没有rd destination是rt**

符号扩展：加法 内存访问 跳转 （可以有负的偏移量）
零扩展：逻辑运算

立即数的用法
•以立即寻址方式提供的一个源操作数。----运算类指令
•作为偏移量，与寄存器rs组成偏移寻址方式，提供一个存储器操作数。----存取指令
•作为偏移量，与PC寄存器组成相对寻址方式，提供一个转移目的地址。----条件转移指令

#### J type
**J**ump:op6 address26
2个：j,jal
>MIPS uses the **jump-and-link** instruction jal to call functions. — The jal saves the return address (the address of the next instruction) in the dedicated register $ra, before jumping to the function.

26位addr,PC 32bits, PC高四位不变，而PC总是4的倍数，把26位左移两位->28
28+4=32
![[Pasted image 20230611191844.png]]

通用寄存器
每个寄存器规定了其作用
![[Pasted image 20230611191419.png]]

**一道错题：指令移位时，若写作16进制，需要转换为二进制移位。十六进制里面移1位相当于二进制移4位**


> 20230520 看了一个Youtube Video
> https://youtu.be/bfPV4x-HrUI
> John Hennessy讲了计算机架构的历史和未来方向，认为当下是又一个属于计算机架构的黄金时期 从早起的CISC RISC 竞争中成长 各自占领市场 根据曲线，未来TPU等重构架构的CPU会迎来高速增长期
> 现在往回看 NVIDIA的计算卡算是成了 现在越来越多的SoC都搭载了CPU之外的新的专用计算单元 应证了他的判断


