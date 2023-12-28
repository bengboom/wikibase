### 移码表示 

>**Offset binary**,[[确定性知识推理]](https://en.wikipedia.org/wiki/Offset_binary#cite_note-Patrice_2006-1) also referred to as **excess-K**,[[确定性知识推理]](https://en.wikipedia.org/wiki/Offset_binary#cite_note-Patrice_2006-1) **excess-_N_**, **excess-e**,[[2]](https://en.wikipedia.org/wiki/Offset_binary#cite_note-Dokter_1973-2)[[3]](https://en.wikipedia.org/wiki/Offset_binary#cite_note-Dokter_1975-3) **excess code** or **biased representation**, is a method for [signed number representation](https://en.wikipedia.org/wiki/Signed_number_representation "Signed number representation") where a signed number n is represented by the bit pattern corresponding to the unsigned number n+K, K being the _biasing value_ or _offset_.

$Bias=2^{n-1}$ 标准移码，用来表示浮点数的阶码
标准移码和2's补码的关系

| Decimal | Offset binary, K = 8 | Two's complement |
|:-------:|:--------------------:|:----------------:|
| 7       | 1111                 | 0111             |
| 6       | 1110                 | 0110             |
| 5       | 1101                 | 0101             |
| 4       | 1100                 | 0100             |
| 3       | 1011                 | 0011             |
| 2       | 1010                 | 0010             |
| 1       | 1001                 | 0001             |
| 0       | 1000                 | 0000             |
| −1      | 0111                 | 1111             |
| −2      | 0110                 | 1110             |
| −3      | 0101                 | 1101             |
| −4      | 0100                 | 1100             |
| −5      | 0011                 | 1011             |
| −6      | 0010                 | 1010             |
| −7      | 0001                 | 1001             |
| −8      | 0000                 | 1000             |

### 浮点数的表示
IEEE 754 Standard
                       符号 阶码  尾数
单精度 float     1+8+23
双精度 double 1+11+52
float阶码偏移量：+127.  double +1023
为什么不是+128 +1024? 去除全1后，浮点数能表达的最大值小了（254-127>254-128)

类似于科学计数法，尾数1.xxxx整数部分一位（规范数）

规范数表示（阶码not all zeros or all ones) 在尾数部分最高位省略的是1 阶码1-254
非规范数的表示(阶码all zeros 真值-126) 尾数最高位省略的是0
0的表示 : All zero
无穷的表示: 阶码all ones 255,尾数 zero 
NaN的表示: 阶码all ones 255,尾数Not zero

表示范围

### 汉字码
汉字内码 国际字符集 ISO/IEC 10646(UCS-4 UCS-2)   UCS-2 Unicode
汉字显示：字模点阵、轮廓描述

### 数据的存储和排列顺序-大端、小端与边界对齐
小端方式（ Little Endian）:  LSB所在的地址是数的地址，即低字节放低地址，然后往地址更大的方向存位权大的。
大端方式（Big Endian）:  MSB所在的地址是数的地址,即高字节放低地址。然后往地址更大的方向存位权小的。
存0x12345678

**指令存放时大端和小端只影响指令中的多字节常数，不影响其他字段的存放顺序。**

|存储方式|地址+0|地址+1|地址+2|地址+3|
|---|---|---|---|---|
|大端|0x12|0x34|0x56|0x78|
|小端|0x78|0x56|0x34|0x12|

边界对齐：字对齐、半字对齐。最小空间单位。

### 数据检错与纠错
奇偶校验码、海明校验码、循环冗余校验码CRC Check digit

如果是奇校验加上校验位后,编码中1的个数为**奇数个**。如果是偶校验加上校验位后,编码中1的个数为**偶数个**。
>计网 CRC, VRC, LRC, Checksum error detection techniques
>https://blog.csdn.net/zhouzuoluo/article/details/102733922

#### Detailed CRC explaination
Cyclic Redundancy Check
###### 检错方法
发送信息$M(x)=100101$,n bits，生成多项式$G(x)=10101$（系数）, 5bit,校验位位数k=bit-1
相除，除完之后余数为零为正确。
###### 校验位生成
$x^k M(x)/G(x)=校验位......0$
CRC码：$x^kM(x)+校验位$
CRC码/G(x) =?......0
余数为零即正确。
###### 纠错
余数不为零，判断出错位置......难。出错位置与余数有关，映射关系。
余数继续除，除完之后是循环的余数，这就是由来。
在网络传输中，因为n位数大，只检测，要求重发，不纠错。
>原理？Proof on [WikiPedia](https://en.wikipedia.org/wiki/Cyclic_redundancy_check)
>Reed, I. S., & Solomon, G. (1960). Polynomial codes over certain finite fields. _Journal of the society for industrial and applied mathematics_, _8_(2), 300-304.


二维码用的Reed-solomon码更强大
里德-所罗门码
>ChatGPT：Reed-Solomon码和CRC码都是常见的纠错码技术，用于提高数字通信中数据的可靠性和容错性。它们之间的主要区别在以下几个方面：1.  纠错能力：CRC码主要用于数据帧的错误检测和纠正，它只能检测和纠正少量的比特错误，一般能够纠正的比特错误数量不超过16个。而Reed-Solomon码则可以检测和纠正更多的比特错误，它能够纠正的比特错误数量可以达到码字长度的一半或更多，因此在一些要求更高可靠性和容错性的应用场合，Reed-Solomon码通常比CRC码更加适合。2.  码长和校验位数：CRC码的码长和校验位数通常是固定的，而Reed-Solomon码的码长和校验位数可以根据具体需求进行设计和调整。在需要更高容错性的场合，Reed-Solomon码可以采用更多的校验位，从而提高纠错能力。   3.  编码和解码方式：CRC码的编码和解码方式相对简单，它的计算主要是基于模2除法运算。而Reed-Solomon码则基于有限域上的向量运算，计算复杂度相对较高，但它的解码过程具有良好的数学理论基础，可以实现高效的解码算法，具有较高的运算效率。


### ASCLL Table
0:48 A65 a97![[Pasted image 20230611131104.png]]
