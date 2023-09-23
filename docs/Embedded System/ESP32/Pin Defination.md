![[doc-esp32-pinout-reference-wroom-devkit.webp]]

## Pay Attention
Input Only 34 35 36 39
6-11 unusable
touch: can feel human, 电容
ADC DAC, in photo

**Pins HIGH at Boot**
GPIO 1
GPIO 3
GPIO 5
GPIO 6 to GPIO 11 (connected to the ESP32 integrated SPI flash memory – not recommended to use).
GPIO 14
GPIO 15

#### I2C
ESP32有两个I2C通道，任何管脚都可以设置为SDA或SCL。将ESP32与Arduino IDE一起使用时，默认I2C引脚为：
GPIO 21（SDA）  GPIO 22（SCL）
如果要使用其他管脚，在使用导线库时，只需调用： Wire.begin(SDA, SCL);


#### SPI
![[2128318-20200930204752896-1495799301.png]]

#### Strapping pins
ESP32芯片具有以下Strapping pins：
GPIO 0
GPIO 2
GPIO 4
GPIO 5（启动期间必须为高）
GPIO 12（启动期间必须低）
GPIO 15（启动期间必须为高）
这些用于将ESP32置于引导加载程序或烧录模式。

每个GPIO的绝对最大电流为40毫安。 耐压3.6V


設置為輸入模式下拉INPUT_PULLDOWN函數(電阻接負)(開關接正)digitalRead→ LOW  
GPIO13、12、14、27、26、25、33、32、15、0、4、16、17、5、18、19、21、22、23  
設置為輸入模式上拉INPUT_PULLUP函數(電阻接正)(開關接負)digitalRead→ HIGH  
GPIO13、12、14、27、26、25、33、32、15、2(led會亮)、4、16、17、5、18、19、21、22、23