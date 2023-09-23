## Arithmetic Circuits

#### Addition
**Half Adder**
两输入，需要四个与非门完成异或得到sum，1个非门进位，共5门18晶体管
**Full Adder**
三输入，两输出 两个半加器加一个或门，18+18+6=42晶体管
**Ripple Carry Adder**
进位链，进位最慢
**Carry Look Ahead**
进位传播 进位生成 CS/Components/[[Chapter 3 Operations]]
代价：逻辑电路变复杂
**Prefix Adder**
一段一段的![[Screenshot 2023-05-10 at 11.10.45.png]]

$t_{prefixAdder}=t_{pg}+log_2N(t_{pg-prefix})+t_{XOR}$


> 这玩意有点像树状数组/线段树 把时间复杂度降到log级别

**BCD Adder**
4bit0-9 + 4bit0-9 = max=19
其实和二进制加法很像，但是大于9，十位是1
用两个4 bit binary adder 和一些组合逻辑实现
输入两个bcd，输出一个bcd和十位是否为1

#### Subtraction
Adder-Subtractor

#### Comparator
利用ALU的标志位完成
大于变小于可以交换ALU两个输入的位置

#### Multiplication
像列竖式一样的级联ALU，n bit Multiplication 复杂度 = (n-1) Adder 复杂度
![[Pasted image 20230508103239.png]]
关键路径长度
二进制补码表示的有符号数乘法 Horner's Rule 高位用符号位补全，直接乘，最后保留低的那一半
其他乘法器
Parallel Multipliers with modified booth recording 速度快
Bit-Serial Multiplexer 用全加器 串行累加

#### Counter 定时器  计数器
ALU 其中一个是(计数步长)，另一个接输出，有一个独立的Register
ALU出现Overflow, 产生中断，定时到了，实现反馈

### ALU in MCU
ALU Flags: Zero Carry Negative oVerflow 用于比较大小等操作 NZCV 标志位
Shifter: Shifter Register, EE/Digital_Circuit/[[Chapter 3 Sequential Logic Design]]/Shift Register
Rotators
多种移位方式 移5位5个CLK？  用MUX电路，每一位输出前都接一个MUX，根据输入的选择端决定输出哪一位的数字。那32位的呢？级联4层4输入mux实现，每次移四位，四次最大16位，左移右移对称性，可以完成32bit数的移位
![[Screenshot 2023-06-03 at 17.04.04.png]]

## Floating Point Addition
**Procedure**
- Extract exponent and fraction bits
- prepend 1 from the mantissa
- compare exp
- shift mantissa
- add mantissa
- normalize mantissa, adjust exponent
- round result
- assemble to FP format
**Speed Precision and Error**

## Memory Arrays
Address->Decoder->WordLine, BitLine
OE Overfloe Enable
读写都在一条线上

ROM 把程序写死 坏了重新加载

Static ROM
有电数就不会丢，但太空中高能粒子会影响
in,sel,wr,out

Static RAM-SRAM [[Chapter 7 Storage Hierarchy]]
做成阵列
DRAM

## Program Modules
[[Chapter 5 Digital Building Blocks]]
PLA
CPLD
FPGA
- IOB
- CLB
- Matrix

Lab of this course
Vivado
a COE (Coefficient) file is a plain text file used to define coefficients or data values for initializing memories, look-up tables, or other types of data storage elements in a digital design.