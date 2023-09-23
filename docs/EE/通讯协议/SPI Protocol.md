SPI, Serial Peripheral Interface, [Wikipedia]()

## Feature

4 Wires:
- CS *Chip Select*, 每个slave一根，相当于对于该元件的connection establish enable
- SCLK *S*erial Clock
- MOSI *Master Out Slave In*
- MISO *Master In Slave Out*
Full-duplex, Variable Data Length
High speed data transfer rates, compared to  I2C UART

## Procedure
1. master selection, 给要发送那个数据的元件的CS发高电平
2. clock synchronization
3. Data Transfer
4. Clock Phase and Polarity 时钟信号的模式（触发和空闲时状态）
    -   **Clock Phase (CPHA):** Determines whether data is captured on the leading (1) or trailing (0) edge of the clock. 上升沿还是下降沿
    -   **Clock Polarity (CPOL):** Determines the idle state of the clock line (0 = low, 1 = high).空闲时的高低电平
5. Chip Select, CS回归低电平
