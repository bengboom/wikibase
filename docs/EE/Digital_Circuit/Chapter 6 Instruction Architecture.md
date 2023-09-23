补充知识：计算机组成原理  存储结构等等，
见 CS/Computer_Organization[[Chapter 4 Instruction Systems]]

> 计算机科学不只有Python，不只有AI，不只有NLP
> 老师所在的抗干扰实验室对我校**计算机科学**的贡献度有27%

> 编译器 汇编器 数据库管理系统 操作系统是计算机中非常重要的核心的东西
> 很可惜 现在国内没多少做这个的 国内都做的应用软件
> 九十年代的时候泥电的操作系统做得真的好，现在没有了
> 这些是真正的根技术

> 老师汇编写了打印下来好几厘米的代码
> 又是天然的加密
> 写了一个中文的下拉菜单 真该用C语言写

DMA *Direct Memory Access*
由dma控制器从硬盘把数据导到硬盘

Watchdog 看门狗
电容加电阻，低电平触发reset
程序正常运行，定期给watchdog发高电平，充电
然后电容电阻放电 放到低电平说明没有正常的信号->reset

函数调用的现场保护：要把子函数需要用到的寄存器压入栈中，保存覆写之前的状态。
LR是存上层的地址，子函数运行完后，返回到上层函数的程序位置。
> 有的同学认为计算机只有软件，因为我们学校的硬件被\*\*\*丢光了

### ARM instructions
[[ARM]]
#### Data processing
cond4 + op2 + func6 + Rn4 + Rd4 + Src2 (12)
Rn Src2 : Source
Rd: Destination
cond: branch
op: 00 for data processing instructions

in func i+cmd+s
i: 1 means immediate
s: set flag
cmd: 4bit to ALU, or doing logic operations

in src2
- immediate ....
- register shamt7+sh2+0+Rm4
- register-shifted register Rs4+0+sh2+1+..

#### Memory


#### Branch

MIPS: Register Immediate Jump.