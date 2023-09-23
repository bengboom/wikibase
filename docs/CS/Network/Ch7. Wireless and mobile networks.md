wireless: communication over wireless link
mobility: handling the mobile user who changes point of attachment to network

## Wireless
- 有中心
	- infrastructure mode 架构较为简单 主要研究速率提升
		base station connects mobiles into wired network
		handoff: mobile changes base station providing connection into wired network
		Mesh
- 去中心
	- ad hoc mode [meaning](https://en.wikipedia.org/wiki/Ad_hoc)
		no base stations 自组织 所以很复杂 很需要研究怎么组织
		nodes can only transmit to other nodes within link coverage
		nodes organize themselves into a network: route among themselves
		MANET, VANET 车联网

#### Characteristics make it difficult
- decreased signal strength 衰减
- interference from other sources 干扰
- multipath propogation 信号反射导致多路

WLAN 通讯距离短
2.5G 3G 通讯距离长
4G 迈出了很大发展

#### SNR BER
SNR *Signal Noise Ratio* 信噪比
increase power comsumption -> increase SNR -> decrease BER *bit error ratio*

#### CDMA code set partitioning
encoding: inner product: (original data) X (chipping sequence)
decoding: summed inner-product: (encoded data) X (chipping sequence)
> 有点像密码学的异或

#### IEEE 802.11 Wireless LAN
BS *base station* = AP *Access Point*
Basic Service Set (BSS) (aka “cell”)

###### Association
host scan channels, listening beacon frames(ssid, mac)
both passive and active scanning
authentication, DHCP

###### 四个地址
![[Screenshot 2023-05-16 at 21.25.32 1.png]]
Receiver Transmitter 第二层AP
Source Destination 第三层Network
同一设备第二层和第三层的mac地址可能相同，也可能不同。
WSN *Wireless Sensor Network* 休眠 降低功耗 不能收发
为了提高传输质量，分片，小帧传输，减少在公开信道上的暴露时间，安全性也提高

###### avoid collisions: CSMA/CollisionAvoidance
sender wait DIFS *Distributed Inter-Frame Space*, idle, then send Data, else wait...
receiver return ACK after SIFS (ACK needed due to hidden terminal problem)

**RTS CTS**
sender first transmits small request-to-send (RTS) packet to BS using CSMA
RTSs may still collide with each other (but they’re short)
BS broadcasts clear-to-send CTS in response to RTS
CTS heard by all nodes, sender transmits data frame, other stations defer transmissions

###### Advanced Capabilities
Rate Adaptation: BERup -> SNRdown -> switch to slower rate -> SNRup,BERdown![[Screenshot 2023-05-16 at 20.25.36.png]]

Power Management
sleep until next beacon file

#### Bluetooth 
PAN *Personal Area Networks*
ad hoc, nearby, master controller, client device, parked inactive device
TDM FDM up to 3Mbps
- parked mode -> save battery
- bootstrapping -> nodes self-assemble (plug and play) into piconet

#### 4G/5G
wide-area mobile Internet
transmission rates up to 100’s Mbps
4G: Long-Term Evolution (LTE) standard
###### Mobile Devices
64-bit International Mobile Subscriber Identity (IMSI), stored on SIM (Subscriber Identity Module) card
LTE jargon *行话*: User Equipment (UE)
###### BS *Base Station*
LTE jargon: eNode-B
Like AP in Wi-Fi
###### HSS *Home Subscriber Server*
stores info about mobile devices for which the HSS’s network is their “home network”
works with MME in device authentication
###### P-GW *Packet Getaway* 
in edge, access to Internet
provides NAT services
###### S-GW *Serving Getaway*
inside cellular network
###### MME *Mobility Management Entity*
- device authentication (device-to-network, network-to-device) coordinated with mobile home network HSS
- mobile device management
	- device handover between cells
	- tracking/paging device location
- path (tunneling) setup from mobile device to P-GW

###### 控制平面
采用了隧道，隧道协议是GTP-U
![[Screenshot 2023-05-17 at 11.26.02.png]]
RRC 信令
NAS *Network Access Service*


## Mobility
移动通信的意思是AP *Access Point* 发生了变化
并不是设备在移动就算mobile，从网络角度讲，**要接入点变化**
- device moves among APs in one provider network
- device moves among multiple provider networks, while maintaining ongoing connections
别交给router, unscalable, 交给 end-devices做切换
保守切换接入点，强度能用就不切换，因为切换会有一段时间没有网络，进行认证过程等

- indirect routing
	- triangle routing : inefficient when correspondent and mobile are in same network
	- on-going (e.g., TCP) connections between correspondent and mobile can be maintained!
	- 有点像全部都要去归属地转发一遍，但在远端的切换是透明的
- direct routing
	- non-transparent to correspondent: correspondent must get care-of-address from home agent
	- 频繁切换时复杂度高

#### Management
###### 4G/5G
![[Screenshot 2023-05-16 at 21.50.30.png]]
切换的动画示例在Chapter7-Slides-72
漫游：也是mobility的体现，不在归属地的接入点
Global cellular network

###### Mobile IP 
too old...
•indirect routing to node (via home network) using tunnels
•mobile IP home agent: combined roles of 4G HSS and home P-GW
•mobile IP foreign agent: combined roles of 4G MME and S-GW
•protocols for agent discovery in visited network, registration of visited location in home network via ICMP extensions