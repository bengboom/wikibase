![[ESP8266-ESP-12E-chip-pinout-gpio-pin.webp]]
![[ESP8266-NodeMCU-kit-12-E-pinout-gpio-pin.webp]]
### Table of use
| Label | GPIO  | Input          | Output       | Notes                                                 |
|-------|-------|----------------|--------------|-------------------------------------------------------|
| D0    | GPIO16| no interrupt   | no PWM or I2C support | HIGH at boot, used to wake up from deep sleep    |
| D1    | GPIO5 | OK             | OK           | often used as SCL (I2C)                               |
| D2    | GPIO4 | OK             | OK           | often used as SDA (I2C)                               |
| D3    | GPIO0 | pulled up      | OK           | connected to FLASH button, boot fails if pulled LOW   |
| D4    | GPIO2 | pulled up      | OK           | HIGH at boot, connected to on-board LED, boot fails if pulled LOW |
| D5    | GPIO14| OK             | OK           | SPI (SCLK)                                            |
| D6    | GPIO12| OK             | OK           | SPI (MISO)                                            |
| D7    | GPIO13| OK             | OK           | SPI (MOSI)                                            |
| D8    | GPIO15| pulled to GND  | OK           | SPI (CS), boot fails if pulled HIGH                   |
| RX    | GPIO3 | OK             | RX pin       | HIGH at boot                                          |
| TX    | GPIO1 | TX pin         | OK           | HIGH at boot, debug output at boot, boot fails if pulled LOW |
| A0    | ADC0  | Analog Input   | X            |                                                       |


### Pins used during Boot
-   **GPIO16:** pin is high at BOOT
-   **GPIO0:** boot failure if pulled LOW Flash D3 不要用
-   **GPIO2**: pin is high on BOOT, boot failure if pulled LOW
-   **GPIO15**: boot failure if pulled HIGH
-   **GPIO3**: pin is high at BOOT 
-   **GPIO1**: pin is high at BOOT, boot failure if pulled LOW
-   **GPIO10**: pin is high at BOOT
-   **GPIO9**: pin is high at BOOT


6-11 connect with internal flash
cannot be used as GPIO
