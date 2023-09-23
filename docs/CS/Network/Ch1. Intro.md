
### Calculation

packet transmission delay = L/R

L : length, bits
R : transmission rate, speed, bit/second. *AKA link capacity, bandwidth*

dnodal = dproc + dqueue + dtrans +  dprop

OSI *Open System Interconnection* RM *Reference Model*

在多层做错误检测：
例如，链路层有CRC，UDP也有Checksum
链路层发现错误，可以短距离（手机和路由器之间）重传，底层问题底层自己先解决，如果把错误报文发到远端，远端Checksum错误，全部重传，会占用更多的网络中间设备的带宽。

校正不是强制的，可以选择不做checksum

IXP *Internet Exchange Point*, connect ISPs by Peering link
