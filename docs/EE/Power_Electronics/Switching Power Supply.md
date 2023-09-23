# DC-DC

## Buck
![[Pasted image 20230713203111.png]]

$Duty=\frac{V_{out}}{V_{in}}$

## Boost
![[Pasted image 20230713203126.png]]
$Duty=1-\frac{Vin}{Vout}$

## Buck-Boost
![[Pasted image 20230713203138.png]]
此电路既可以升压也可以降压。

## 同步整流
二极管换成开关管，可以小幅提升开关电源转换效率

# DC-AC

## H bridge and SPWM
sin wave, 4 MOSFETs.

H桥，spwm，占空比调幅度![[Screenshot 2023-07-29 at 22.35.07.png]]
要注意有感性负载的情况下，电感两端电流不能突变，开关全关的时候不存在放电回路，会有问题，需要做一个放电回路。可以是通过开启两个mos实现，也可以mos+二极管。也就是说，这四个mos管需要单独控制，必要时需要加组合逻辑来控制。
![[Pasted image 20230729133636.png]]

而本身他就需要储能元器件，来平滑pwm曲线，所以一定存在电容电感，所以不能把12、34的mos控制端短接在一起控制，还有别的情况！这一段纯属自己分析，还不确定。

确实！要分单极性调制和双极性调制，还要分高频臂和低频臂，确实不是只有14，23两种状态 https://zhuanlan.zhihu.com/p/41070569
![[Pasted image 20230729223238.png]]![[Pasted image 20230729223245.png]]
高低频率臂，工作的时候不能全关。  https://www.dianyuan.com/article/26214.html
![[Screenshot 2023-07-29 at 22.35.38.png]]