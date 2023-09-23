RTP：抽象概念，reliable transfer protocol
UTP：unreliable

与网络层双向传输：网络层提供反馈给传输层判断是否出错

TCP在操作系统中实现，数据包不同由操作系统造成

### UDP packet
message oriented, no frills, bare bones
UDP package components: source destination length checksum data
>checksum 最高的进位加到最低位 *wraparound*

connectionless, no handshaking.
used in streaming, DNS, SNMP, HTTP3

### rdt: reliable data transfer protocol *Principle*
FSM Finite state machine 有限状态机来描述server and client
- rdt1.0: reliable transfer over a reliable channel
- rdt2.0: channel with bit error 
	- *checksum* to detect error
	- recover from error
		- ACK for correct packet 
		- NAK for error pkt  -> sender retransmit pkt
		- This is ARQ (Automatic Repeat reQuest) protocols.
- rdt2.1
	- 数据和反馈可能损坏
	- 加上数据分组序号（pkt0,pkt1)
- rdt2.2
	- 不再发NAK
	- ACK with sequence number
	- pkt0出错，接收方发ack1，而不是ack0，ack1作为当前pkt0出错的反馈，替代NAK
	- 也有数据分组序号
- rdt3.0 *channels with errors and loss*
	- 超时认为丢失，发送方重传，增加超时机制，防止ACK0/ACK1丢失
	- 继续保留ACK
	- 会有pkt0 timeout后才收到ACK0的情况，导致还是重传pkt0

rdt是**停等协议(stop-and-wait behavior)**，信道利用率低 
->  更高效的：回退N *sliding window* 选择重传

#### Performance Calculation
U : utilization – fraction of time sender busy sending 利用率
$$U_{sender} = \frac{ \ (*window \ size*) \ L/R}{RTT+L/R}$$
RTT>>L/R, 利用率低 -> pipelined protocol -> window 提升效率


### TCP
connection-oriented
reliable point-to-point

close a TCP connection FIN bit  = 1
FIN message ACK message

### Congestion control
AIMD *Additive Increase, Multiplicative Decrease* -> sawtooth behavior, probing for bandwidth

Multiplicative decrease detail:  sending rate is
- Cut in half on loss detected by triple duplicate ACK (TCP Reno)
- Cut to 1 MSS (maximum segment size) when loss detected by timeout (TCP Tahoe)
更新的方法：TCP CUBIC
![[Screenshot 2023-05-29 at 22.25.29.png]]
Explicit congestion notification (ECN)

Buffers, Windows, Segments
Flow control

已注册端口是指已被 IANA（互联网号码分配机构）分配并注册，用于特定应用程序的端口号。应用程序必须向 IANA 请求已注册端口才能使用它们。一些已知的已注册端口号包括 HTTP（端口号80）、FTP（端口号21）、SSH（端口号22）等。

私有端口指的是不受 IANA 管理的端口号，通常用于本地或专用应用程序。动态端口是指操作系统动态分配的未被特定应用程序使用的端口号。源端口是指发起网络通信的计算机使用的端口号。

SNMP（Simple Network Management Protocol，简单网络管理协议）、DHCP（Dynamic Host Configuration Protocol，动态主机配置协议）和 TFTP（Trivial File Transfer Protocol，简单文件传输协议）也是应用层协议，但它们使用的是 UDP（User Datagram Protocol，用户数据报协议）或者是其他传输层协议。