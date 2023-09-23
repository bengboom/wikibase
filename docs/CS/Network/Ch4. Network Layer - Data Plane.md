network layer protocols in every Internet device: hosts, routers
function:**Forwarding && Routing**

## Data Plane -- Forwarding(match+action)
local, per-router function
determines how datagram arriving on router input port is forwarded to router output port

#### Network Service Model
services?goal?

Service Model -- Explained by ChatGPT
>A service model is a high-level description of how a network protocol behaves in terms of the services it offers to the layers above it, such as the transport and application layers. It specifies the protocol's functionality and performance characteristics, as well as the interfaces between the layers. Service models standardize the behavior of network protocols, ensuring interoperability between different devices and applications. They provide a clear and standardized way to describe the services that a network protocol should provide, which is essential for ensuring the reliability of computer networks.

|Category|Goal|
|---|---|
|Single Packet| Guaranteed Delivery & delay limit 40ms|
|a flow of packets(报文之间有相关性，一堆TCP包)|in-order, bandwidth, 数据包之间的间隔时间|

在Network layer, **Best Effort** method (service model) is the most popular
>chatGPT
>Best-Effort服务是一种网络服务模型，其特点是不提供任何服务质量保证，数据包在网络中按照“尽力而为”的原则进行传输，具有不可靠性和不确定性。服务的性能取决于网络拥塞情况和传输路线等因素。这种服务模型的优点在于简单、灵活、广泛支持和成本低廉。
>如果您是指互联网协议，那么可以说它是最成功的协议之一。互联网协议（IP）是互联网的基础协议之一，它定义了互联网上的数据包格式和传输方式。几乎所有计算机和网络设备都支持IP协议，因此它成为了连接世界各地计算机和网络的关键协议。IP协议的成功在于其简单、灵活、可扩展和通用性。它的成功也催生了许多其他的互联网协议和技术，例如TCP、HTTP、DNS等，这些协议和技术共同构成了现代互联网的基础设施。

ATM protocol: 用户侧成本很高，不常用
在网络层做可靠性保障成本代价高，因为下面的链路差异度太大，稳定性达不到要求。
光纤、双绞线、WiFi等等，要做一个统一的有稳定性保证的网络层协议难度高，对下面链路层要求高。同时，上面传输层有reliability保证。
IP协议对下层链路层要求小。质量好或质量差的网络协议都可以承载IP协议。
***底层负责传输越快越好，高层负责传输可靠性。这与系统设计原则有关***

#### Router
**router architecture: IO ports, processor, high-speed switching fabric**
>在网络领域，"fabric"通常指的是高速交换机和其他网络设备之间互联的基础设施。具体来说，它是指由多个交换机和连接这些交换机的高速链路组成的网络拓扑结构，旨在提供高速、可靠的数据传输。"fabric"的特点在于其高带宽、低延迟、可扩展性和可靠性。相比传统的交换机互联方式，使用"fabric"可以提供更高的带宽和更低的延迟，从而满足现代数据中心、云计算和大规模企业网络的需求。


goal: 达到物理层传输介质的line speed
如果转发速度达到line speed, 利用率就最高，存在处理开销，造成速度降低，congestion->缓存->queueing->delay，转发不了就丢掉

match+action

#### Forwarding
- destination-based forwarding 目的转发
	Longest prefix matching:when looking for forwarding table entry for given destination address, use longest address prefix that matches destination address.
	而不是遍历，开销太大。

	>缺省路由（default route）是指当路由器无法确定数据包下一跳的路由路径时，将数据包转发到预先配置的缺省路由器上，由缺省路由器继续处理该数据包。缺省路由通常用于在较大的网络中提高路由器的效率和简化路由表配置。如果路由器没有找到与目标地址匹配的路由表条目，则会使用缺省路由转发数据包。缺省路由可以视为“后备方案”，以确保数据包不会被丢弃或无法到达目的地。

- generalized forwarding 通用转发 *used in SDN

#### Switching fabrics
rate=NR, ideally
type:shared memory, bus, interconnection network(常用，快，crossbar)

#### Queueing
Buffering
Drop Policy
- Drop What? newest or oldest? 队列/优先队列
	- tail drop
	- on priority basis
- how to scheduling
	- first come fitst served FIFO
	- priority
	- RR round robin 按照header分类，一次发一组里的一个包
	-  WFQ weighted fair queueing 广义RR,每个组有占用时间权重
- how much buffering
	- traditional: $link Capacity*RTT$
	- recent: $\frac{C*RTT}{\sqrt n}$

## 自学

#### IP
20byte header‘
subnet part & host part
address **CIDR** *Classless InterDomain Routing* 
address format: a.b.c.d/x, where x is # bits in subnet portion of address
转换为子网掩码格式，x=24,255.255.255.0，前24位为1.
ISP->Organization->User, 子网部分越来越长
分配addressing：静态/DHCP *Dynamic Host Configuration Protocol*

**IPv4** address space exhaustion -> NAT & IPv6
NAT *Network Address Translation* 转换端口 转换表项存在时间
NAT穿透 多种方式

NAPT PAT
端口映射，只变端口号，$2^{16}$

子网分配方式（一个地址要用作子网号（网络地址），还有一个地址用作广播地址）
在子网中，网络地址（主机位全为0）和广播地址（主机位全为1）不能分配给设备。

**IPv6**
datagram format 40 byte
Tunneling and encapsulation中间设备不支持的话，用Ipv4把整个ipv6包再装起来以兼容，遇到支持ipv6设备后再解开
ipv6不支持广播
地址简写方式
子网分配方式

ICMP, IP的辅助
IP不报告错误，由ICMP来做
error reporting (“目标不可达”、“超时”、“重定向”等等)
router signaling (ICMP ping、traceroute)路由器信令

#### SDN
generalized forwarding
南向协议openflow match+action+stats
[p4, today's network programmability](https://p4.org/)

#### Middleboxes
SDN NFV *Network Functions Virtualization* 
Programmable network devices

![[Pasted image 20230412114424.png]]
