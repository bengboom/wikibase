implemented in NIC *Network Interface Card*
combination of hardware, software, firmware
多路访问需要地址，点对点其实不需要地址，有也是形同虚设

**两种link**
**P2P**
**Broadcast**(shared wire/medium) 一个站点发送信息，所有站点都能收到
比如Wi-Fi、4G信号，它就在空中，都能收到，这就是broadcast的link
信号是广播传递，不代表信息是广播传递。根据地址来决定是否接收

PPP *Point-to-Point Protocol*
HDLC *High-level Data Link Control*

## Link Layer Reliability
也需要做可靠性, EDC *Error Detection and Correction*
- flow control
	-  nope,  主要是accrss control
- error detection and correction
	- Parity Cheking 奇偶校验，一维二维，只错一位的情况下，二维可修复
	- Internet Checksum 把数据看作16bit int, 全加起来
	- CRC Detailed Explaination:[[Chapter 2 Numbers]]
- half-duplex and full-duplex
	- 主流的都是半双工，WiFis
	- 以太网双绞线，两个通道，可以全双工

## multiple access protocols
信道使用、访问、接入控制
determine how nodes share channel
#### MAC *multiple access channel*
e.g. R bps, M nodes, each R/M bps speed, an ideal protocol
**分类** *taxonomy*
- channel partitioning, based on (time slots, frequency, code)
	- TDMA *Time Division Multiple Access* 时分多址 时分复用
		- access to channel in rounds, unused->idle 怠工
	- FSMA *Freqency Division Multiple Access* 频分多址 复用
	- CDMA *Code Division Multiple Access* 码分多址
- random access 随机接入协议
	- recover from collision
	- ALOHA *1968年美国夏威夷大学的一项研究计划的名字,是夏威夷人表示致意的问候语*
	- slotted ALOHA
		- 如果冲突了，就都退避，等一会再发，在冲突再避再等再发
		- 随机等待时间，像是路上行人之间的避让，碰运气
		- Time division, need sync clock
		- efficiency 37![[Screenshot 2023-05-06 at 11.11.51.png]]
	- pure ALOHA
		- no sync
		- efficiency 18%![[Screenshot 2023-05-06 at 11.14.44.png]]
	- CSMA *Carrier Sence Multiple Access* 载波侦听多路访问
		- 在发送数据前先侦听当前信道，busy就不发了，因为发了冲突也白发
		- 像两个人说话一样，在线会议听到别人说话自己就停下
		- collision detection easy in wired, difficult with wireless
		- 因为propagation delay, 检测需要时间，还是可能whole package wasted![[Screenshot 2023-05-06 at 11.22.03.png]]
	- CSMA/CD *Collision Detection* **Used in Ethernet**
		- transmission aborted on collision detection 发送的同时监听
		- binary (exponential) backoff 指数增长回避等待时间
	- CSMA/CA *Collision Avoidance* **Used in 802.11**
		- 无线网络检测和恢复碰撞要困难得多
		- **载波感知**：与CSMA/CD一样，CSMA/CA在发送数据前会先监听信道。如果信道忙，节点将不会发送数据。
		- **碰撞避免**：CSMA/CA通过一种名为“退避”的机制来避免碰撞。当节点检测到信道忙时，它将等待一段随机时间再试图发送数据。
		- **虚拟载波感知**：CSMA/CA通过使用RTS（请求发送）和CTS（清除发送）数据包来进一步避免碰撞。节点先发送一个RTS数据包，然后等待网络的回应CTS数据包。如果收到CTS，节点才会发送主数据包。这种机制有效地预留了信道，使得其他节点在这段时间内不会试图发送数据。
		- **确认机制**：接收节点在成功接收数据包后会发送一个确认（ACK）数据包，如果发送节点没有收到ACK，它会假定数据包已经丢失，并会尝试重新发送。
- take turns
	- polling
		- controller node “invites” other nodes (clients)to transmit in turn 需要一个控制器
		- ![[Screenshot 2023-05-06 at 11.34.01.png]]
	- token passing
		- control token passed from one node to next sequentially
		- 一个环 收到自己的了就说明发完了 交给下一个![[Screenshot 2023-05-06 at 11.42.27.png]]
	- Bluetooth, FDDI *Fiber Distributed Data Interface*,  token ring 



## LANs *Local Area Networks* 局域网
早期网络分类局域城域广域，体现通信距离，广域最好别wasted，都传这么久了太可惜了
城域还是token passing一类

#### MAC address
MAC address allocation administered by IEEE, manufacturer buys portion of MAC address space 
组播MAC地址为01开头
任何长度小于 64 个字节的帧都被接收站点视为“冲突碎片”或“残帧”而自动丢弃。超过 1500 个字节的数据帧被视为“巨帧”或“小型巨型帧”。

#### ARP *Address Resolution Protocol*
IP/MAC address mappings for some LAN nodes:< IP address; MAC address; TTL>
TTL (Time To Live): time after which address mapping will be forgotten (typically 20 min)
Request 广播
Response 单发
command: show ip arp / arp -a
ARP broadcast and spoofing--responder giving the wrong mac address to the requester, pretending to be the default getaway to steal packets
DAI *Dynamic ARP Inspection* -- validate ARP packets

#### IPv6 ND *Neighbor Discovery* Protocol
using ICMPv6
-   Neighbor Solicitation messages
-   Neighbor Advertisement messages
-   Router Solicitation messages
-   Router Advertisement messages
-   Redirect Message

#### Ethernet
connectless, unreliable , unslotted CSMA/CD with binary backoff
frame structure:preamble, dest, src, upper-layer protocol type, data, CRC

#### switches
layer 2 switch, 透明的, self-learning *plug and play*, transparent
**Scalable** self-learning switches can be connected together
信号经过长距离传输会衰减，受传输介质限制，可以使用物理层放大中继，也可以使用中继交换机，layer2中继，交换机采用一些方法分开**冲突域**，这样冲突概率降低。
switch forwarding table (mac address, interface port, time stamp TTL)

#### VLANs *Virtual Local Area Network (VLAN)*
switch(es) supporting VLAN capabilities can be configured to define multiple virtual LANS over single physical LAN infrastructure.  内网隔离
Port-based VLANs
- traffic isolation
- dynamic membership
- forwarding between VLANs
- trunk port. package need VLAN ID info*802.11q*

## Link Virtualization - MPLS 
MPLS *MultiProtocol Label Switching*

## Data center networking
elements: Border routers, Tier1 Tier2 Switched, TOR *top of rack* switches(one per rock), server racks. 
中间有fully connected的交换机
use multipath to enrich interconnection between racks **Redundancy**
load balancer: application-layer routing
**protocol innovations** 
- link layer RoCE *Remote DMA(RDMA *Direct Memory Access*) over Converged Ethernet*
- transport layer 
	- ECN *explicit congestion notification* 
		- DCTCP *Data Center TCP*
		- DCQCN *Data Center Quantized Congestion Control*
	- hop-by-hop backpressure congestion control 一跳一跳的压力反向传播
- routing, management
	- SDN
	- place related services physically closely

## End of Top Down Approach -- a day in the life of a web  request




