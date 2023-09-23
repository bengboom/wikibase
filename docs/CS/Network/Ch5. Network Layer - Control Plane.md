## Control Plane - Routing
network-wide logic 找到整个互联网范围的，尽可能最优的路径
2 kind of control plane: 
**per-router**(individual routing algorithms) 独立计算
**SDN**(remote server controlled, using CA *Certificate Authority*) 集中控制

## Routing 

###### Routing algorithm classification
- Global/Decentralized
- Dynamic/Static
	- static: changes slowly instead of staying the same all the time

###### link state  *global*
Dijkstra algorithm 鲁棒性更好 基于网络的完整视图
*centralized , proactive先应式
iterative(after k iterations, know least cost path to k destinations)*
对于一个节点，要生成一个 *以该节点为树根的*树 无环图
最短路径优先树
>It differs from the minimum spanning tree because the shortest distance between two vertices might not include all the vertices of the graph. [Source](https://www.youtube.com/watch?v=k6o2x42mrIg)

Like broadcast, complexity $O(n^2)$, too complex

oscillations possible [explaination](https://www.youtube.com/watch?v=KmOviXWpec4) .以流量作为cost时可能会发生
选最空闲的发送，于是空闲的变为繁忙，繁忙的又变为空闲，造成震荡，增大路由开销。可以不同步的计算这里就没问题了，流量变化不立刻影响cost变化，而是一段时间内的值或不同时间加权值（指数衰减），就可以避免这个oscillation.
![[Pasted image 20230418093546.png]]


###### distance vector  *decentralized*
反应式reactive
Bellman-Ford ( Dynamic Programming ) 基于邻居 不广播自己的计算结果
会把自己的表转发给邻居，也是根据邻居的表更新自己的表
算法实现过程

###### intra-ISP OSPF *Open Shortest Path First*
link-state routing
making routing scalable -> administrative autonomy
subnetwork self-managed, known as AS *autonomous systems* or domains
**intra-AS/inter-AS** linked by **getaway router**
intra protocols
- RIP *Routing Information Protocol*  DV, no longer widely used
- EIGRP *Enhanced Interior Getaway Routing Protocol* DV, formerly Cisco-proprietary, opened in 2013
- OSPF
	- open means public available
	- Link State Algorithm
	- send via IP
	- security -- messages authenticated
	- two level hierarchy: local area & backbone

###### among ISPs BGP *Border Getaway Protocol*
eBGP:inter-AS *External BGP*
iBGP:AS-internal *Internal BGP*
**peer** and **advertising paths** using TCP
path vector protocol
messages: OPEN UPDATE KEEP ALIVE NOTIFICATION
*hierarchical routing saves table size, reduced update traffic*

**hot potato routing**: choose local gateway that has least intra-domain cost, don’t worry about inter-domain cost!

可选择是否转发给其他peer router
>ISP only wants to route traffic to/from its customer networks (does not want to carry transit traffic between other ISPs – a typical “real world” policy)

## SDN Control Plane
Data-plane switches
SDN controller (network OS)
network-control apps

## ICMP *Internet Control Message Protocol*
ICMPv4 ICMPv6 carried in IP datagrams
used by hosts and routers to communicate network-level information
error reporting: unreachable host, network, port, protocol
echo request/reply (used by ping)

## Network Configuration


#### SNMP *Simple Netork Management Protocol*


#### NETCONF/YANG(Language)



