I2C, Inter-integrated Circuit, IIC, [Wikipedia](https://en.wikipedia.org/wiki/I%C2%B2C#:~:text=I2C%20(Inter%2DIntegrated,in%201982%20by%20Philips%20Semiconductors.)

## Feature
Data = Address + Data

2 Wires: Clock + Data

Master / Slave 可换向 半双工

Multimaster : 仅一个可以同时工作

## Procedure

1.  **Start condition:** The master initiates communication by generating a START condition on the bus. This signals the beginning of a transaction.
    
2.  **Addressing:** The master sends the 7-bit address of the slave device it wants to communicate with, along with a read/write bit to indicate the direction of the transfer (read = 1, write = 0).
    
3.  **Acknowledge:** If the addressed slave device is present on the bus, it sends an acknowledgment signal (ACK) back to the master.
    
4.  **Data transfer:** The master and slave devices can now send or receive data bytes. Each byte is transmitted in 8 bits, followed by an ACK signal from the receiving device.
    
5.  **Stop condition:** When the master is done with the transaction, it generates a STOP condition on the bus, indicating the end of communication.


![[Screenshot 2023-05-12 at 17.09.08.png]]