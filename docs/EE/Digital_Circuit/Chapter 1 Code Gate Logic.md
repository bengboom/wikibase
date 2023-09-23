## 20230308

### Bit
带权数：阿拉伯数字位数带权

W bit 的误差 期望和方差
range $$-\frac{2^{-(w-1)}}{2}<e(n)<+\frac{2^{-(w-1)}}{2}$$
Interval $$\Delta=2^{-(w-1)}$$
$e(n)$ is the uniform distribution on $[-2^{-W},2^{-W}]$
$E[e(n)]=0,Var[e(n)]=E[e(n)^2]=\frac{2^{-2W}}{3}$

一位可以减少4倍误差（本来是两倍，但是算$\sigma$成4）
2 -> 3dB
4 -> 6dB
算信噪比: 8bit: 8x6=48dB 10bit:10x6=60dB

### Arithmetic
存内计算
$A x$  A in memory ，两个加法线

乘法器Repeated Left-shift and Add Algorithm

除法器：与手算规则不同，（以除数的位宽对齐被除数）先减，看够不够，不够再加回去

MSB：Most significant bit
LSB:  Least

有符号运算？
用二进制编码方式表示正负数

#### Signed magnitude representation

二进制第一位是1就是负数，转换为十进制后：$915_{10}=11111_{2}$
容易理解，方便表征，但是有$+0,-0$ 且make simple math harder

#### 补码系统Complement Number systems

1. Radix-complement 进制补码
	$X_{complement}=R^n-X$
	8位则用九位最小值来减：100000000-10011011=其补码
	也就是按位取反后加一，再求一次补码得原数。
2. diminished radix complement
	$01100101_{2's complement}=10011011$
	$01100101_{1's complement}=10011010$
	X进制有:X的补码和X-1的补码，分别是x和x-1来减。
用x的补码表示负数，好处：方便计算$4-3=(4-(16-3))mod 16$
2+(-6)=0010 + (10000-0110 )  =0010+1010=1100 (-4的补码表示，10000-0100=1100)  
判断Overflow?如果两个数符号相同，且加起来后符号变了，则有溢出。或者全加器最后一位carry为1则溢出。

## 20230313

### Code
#### binary coded decimal BCD
一位十进制：4bit
十位十进制：
用7-segment数码管来显示BCD，若有小数点就是8bit

__
|__ |
|__ | 。

共阴/共阳

不同BCD：8421 4221 5421 BCD (四个bit 的权重)
![[Pasted image 20230624164137.png]]
Unpacked 一个BCD占用字节空间 / packed两个BCD一个字节


#### Gray Code 格雷码 （Reflected binary code）
两个相邻码字只有一个Bit不同

|Dec|7|8|
|---|---|---|
|Bin |0111| 1000|
|Gray| 0100| 1100|

只差一位，有规则
0 1
00 01 11 10
 00  01  11  10  10  11  01  00......(对称过去)
	000 001 011 010 110 111 101 100......然后在前面添0000,1111
直接转换规则：最高位相等，然后可以递归求。MSB和次高位相加去掉进位得Gray次高位(XOR)，如此递归到最低位。
格雷码到二进制直接转换：最高位一样。二进制已求得的最低位与格雷码更低的一位相加得到二进制码更低位(XOR)，递归。

对于数字信号传输，这种码error更小
>Gray codes are widely used to prevent spurious output from [electromechanical](https://en.wikipedia.org/wiki/Electromechanical "Electromechanical") [switches](https://en.wikipedia.org/wiki/Switch "Switch") and to facilitate [error correction](https://en.wikipedia.org/wiki/Error_correction "Error correction") in digital communications such as [digital terrestrial television](https://en.wikipedia.org/wiki/Digital_terrestrial_television "Digital terrestrial television") and some [cable TV](https://en.wikipedia.org/wiki/DOCSIS "DOCSIS") systems.

用于memory寻址减少功耗，连续地址变化小。有点像基因的突变。

### Other Error Detection Code
Parity Code
Repetitioncode
CRC : Used in IP package (detect only)
Hamming Code
[[Chapter 2 Numbers]] 浮点数 见计组

### Gate
各种Gate的表征方式、图示、转化
NOR NAND 可直接实现，所有都可以用这两个表示，所以叫universal gates

## 20220315

### Logic circuits

#### Bipolar families
DL：Diode Logic 二极管构造与或门方便
RTL：可构造非门Resistor Transistor Logic (RTL)
DTL：Diode Transistor Logic (DTL)
TTL：功耗更高  Transistor Transistor Logic
ECL I2L：在高速电路使用Emitter Coupled Logic (ECL),Integrated Injection Logic (I2L)

#### MOS families
常用 便宜功耗低
PMOS：Gate低电平通，画的时候多一个圈
NMOS：Gate高电平通
CMOS：互补金属氧化物半导体，NP都用，仅在切换时耗电
图示、掺杂方式、逻辑
CMOS状态：HIGH LOW Undefined
可等效于开关（开断）、电阻（高低阻值）

Bi-mos logic family: bipolar and MOS devices 功率器件

#### 74 / 54 Series
74xx(民-20~85℃) 54xx(military -55~125℃) 区别在温度范围、加速度
xx：technology LS（低功耗）  Fast  AS  HCT（高速） 
>[Wiki](https://en.wikipedia.org/wiki/7400-series_integrated_circuits)  VCC GND 管脚固定
>为了标准封装 存在NC（NotConnect, I guess)
>Schottky Diode 肖基特二极管 (S) 高速

nnn/nn：function 04反相器，138译码器

#### CMOS logic
输出点：p n 连接点
p并n串 p串n并 这样才能做到不短路
反相器NOT：p接vdd n接gnd
与非门NAND：两p并 两n串  1110
![[Pasted image 20230624172816.png]]
或非门NOR：两p串 两n并     1000
![[Pasted image 20230624172825.png]]
Noninverting gates：输入输出不变
![[Pasted image 20230624172930.png]]
buffer,缓冲器（调延迟 驱动）由工具（EDA）自行加入
与或非门：$\overline{AB+CD}$，在MOS级实现比Gate级节约大量空间，EDA来做
或与非门：

多通道：扩展串并联
Fan in of CMOS：输入门增加后，并联导通电阻会变小，串联增加，产生影响。解决方式：多个小Fan-in但Delay增加
>**Fan-in** is the number of inputs a [logic gate](https://en.wikipedia.org/wiki/Logic_gate "Logic gate") can handle.[[1]](https://en.wikipedia.org/wiki/Fan-in) Physical logic gates with a large fan-in tend to be slower than those with a small fan-in. This is because the complexity of the input circuitry increases the input [capacitance](https://en.wikipedia.org/wiki/Capacitance "Capacitance") of the device. Using logic gates with higher fan-in will help in reducing the depth of a logic circuit; this is because circuit design is realized by the target logic family at a digital level, meaning any large fan-in logic gates are simply the smaller fan-in gates chained together in series at a given depth to widen the c ircuit instead.

#### noise margin specification
- HIGH-level input voltage, VIH.
- LOW-level input voltage, VIL.
- HIGH-level output voltage, VOH.
- LOW-level output voltage, VOL.

$V_{OH}=VCC-0.1V \ \  V_{IH}=70\%VCC \ \  V_{IL}=30\%VCC \ \  V_{OL}=GND+0.1V$
![[Pasted image 20230624173113.png]]
Fan Out 扇出输出同时接多少个：有限制
Fanout of Low or High states
考虑AC交流
用buffer效果：减少充放电时间，将大电容充放电变成一堆小电容同时充放电
设置fanmax，减少充放电时间，加入buffer，也会增加delay，进行取舍。

不用的管脚：连到VCC或GND，不能悬空float，输出也不行，有静电电荷，积累会击穿MOS
![[Pasted image 20230624173148.png]]

#### Power consumption
Internal Power
$$P_T = C_{PD} V_{DD}^{2} f_{transition}$$

Load Power
$$P_L=C_LV_{dd}^2 f$$

Dynamic Power
$$P_D=(C_L+C_T)V_{dd}^2f$$

降压减小功耗，propagation delay + , noise margin -
降频降低功耗，电压同时下降，功耗降低更多。

#### Open drain output
输出需要接上拉电阻。上升下降时间不同。
这种可以把多个门电路的输出连在一起，形成一个“总线”，“输出选择器”
可以完成一个与运算
![[Pasted image 20230624174608.png]]

#### 三态输出 three state outputs
高阻 H L （Bit  switch)![[Pasted image 20230624174649.png]]

## 20230507 Complementation
Weighted Number System
- fixed point
	- conventional
		- ...
	- unconventional
		- signed digit
		- fractional
		- logarithmetic
		- RNS *residue number system* 效率高面积小 High-performance Computation
- floating-point
	- conventional
		- IEEE 754
	- unconventional
		- 18bit
		- Splash II
		- format