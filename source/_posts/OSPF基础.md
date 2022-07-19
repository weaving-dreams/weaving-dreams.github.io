---
title: OSPF基础
tags:
  - 锐捷
categories:
  - 学习
  - 锐捷
abbrlink: 2400a6ce
date: 2022-06-20 15:48:36
---





<div class="danger">

>  资料来源[锐捷](https://talent.ruijie.com.cn/certification/profession/)
>  非原创

</div>



<br>

<br>

OSPF协议，全称开放最短路径优先协议（ Open Shortest Path First ），它是IETF组织开发的一个基于 链路状态的内部网关路由协议（IGP）。因其的开放性以及丰富的功能特性，成为目前业内使用最广泛的 路由协议之一





## OSPF的基本概念

### 概念

* 专为TCP/IP网络设计，支持VLSM、路由汇总、等价负载均衡、区域划分、认证 
* OSPF在RGOS平台上的管理距离（AD）是110 
* OSPF报文封装在IP报文中，IP协议号为89 
* OSPF具有无环路、收敛快、扩展性好，可适应大规模网络 
* OSPF目前应用中有两个版本： 
  * V2：适用于IPv4 
  * V3：扩展支持IPv6

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154540.png)



### OSPF的Cost值

* OSPF的接口开销（Cost）计算方式：基于物理链路的带宽来计算度量值 

  * Cost=默认计算基数/物理链路带宽（Bit为单位），默认计算基数= 108 bit=100M
    *  100M带宽的接口，Cost值就是1 
    *  10M带宽的接口，Cost值就是10 
  * 如结果出现小数，会舍弃小数部分取整（如果整数部分为0，则Cost值记为1）， 例如带宽是1000M的链路， Cost值也是1 
* OSPF路由条目的Cost值=路由的原始Cost值和沿途入向接口Cost值的累
* 如图，当10.1.0.0/24访问10.2.0.0/24时，数据从F0/1出去，这是一条次优路径！ 
* 修改Cost值解决.

![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154547.png)










  * 方案一：在OSPF进程中可以修改度量值计算基数来避免这种问题。命令中的1000代表默认计算基数1000M

  ```
Ruijie(config)#router ospf 10
Ruijie(config-router)#auto-cost reference-bandwidth 1000
  ```

  

  * 方案二：在接口下，使用该命令，直接修改Cost值为200，来影响OSPF路径计算结果

  ```
uijie(config)#int f0/1
Ruijie(config-if)#ip ospf cost 200
  ```

##   OSPF邻居建立的过程

### OSPF相关术语

* Router-ID：在AS中唯一标识一台运行OSPF的路由器的编号 
  * AS（自治系统 Autonomous System） ：使用同一种路由协议一组路由器所构成的一套系统 
  * 每个运行OSPF的路由器都必须有一个Router ID，同一个AS内，Router-ID不能重复 
  * Router-ID可以手动命令配置，也可以系统自动选举
* 邻居（Neighbor）：两台运行OSPF协议的路由器，从它们相连的接口上会相互发出各自的OSPF参数， 如果双方的参数符合建立邻居的条件，就会形成邻居关系 
* 邻接（Adjacency）：邻居不一定邻接。如果两台路由设备之间交换链路状态信息，并根据更新后的数据库计算 出OSPF路由，才能称为邻接关系

![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154600.png)





### Router ID

* 在没有手工指定的情况下（手工指定，直接成为Route ID）
  *  如果本地有激活的Loopback接口，则取Loopback接口IP最大值作为OSPF Router-ID 
  *  如果没有Loopback接口，则取活跃的物理接口IP地址中的最大值 
* 项目实施中，一般先创建loopback接口并配置IP地址，随后手工指定OSPF Router-ID为该接口地址

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154604.png)





### OSPF的五种报文类型

* 有5种不同OSPF报文类型，这些报文类型用从1到5的类型号标识

![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154616.png)





### OSPF邻居建立的三个阶段

* 邻居发现，形成邻居：（成功的标志：2-Way状态） 
  * 通过Hello报文发现并形成邻居关系 
  * 形成邻居表 
* 形成邻接，路由通告：（成功的标志：Full状态，LSDB同步） 
  *  邻接路由器之间通过LSU洪泛LSA，通告拓扑信息 
  *  通过DBD、LSR、LSACK辅助LSA的同步
  *  最终同一个区域内所有路由器LSDB完全相同 
* 路由计算阶段： 
  *  LSDB同步后，每台路由器独立进行SPF运算
  *  把计算出的最佳路由信息放进路由表

![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154621.png)





### OSPF常用的三张状态表

* 邻居表（Neighbor Table）： 
  *  通过Hello报文形成邻居	
  *  用邻居机制来维持路由 
  *  邻居表存储双向通信的OSPF路由器列表信息 

```
R2#show ip ospf neighbor 
Neighbor ID Pri State Dead Time Address Interface
172.16.2.254 0 FULL/ - 00:00:33 10.1.0.1 Serial0/1/0
10.3.0.254 1 FULL/BDR 00:00:31 10.2.0.2 FastEthernet0/0
```





*  链路状态数据库（LSDB）： 
   * 通过LSU报文更新LSDB 
   * 描述拓扑信息的LSA存储在LSDB中 

```
R2#show ip ospf database 
OSPF Router with ID (10.2.0.1) (Process ID 10)
Router Link States (Area 0)
Link ID ADV Router Age Seq# Checksum Link count
172.16.2.254 172.16.2.254 352 0x80000004 0x00097c 4
10.2.0.1 10.2.0.1 310 0x80000004 0x005554 3
10.3.0.254 10.3.0.254 310 0x80000003 0x006a97 2
Net Link States (Area 0)
Link ID ADV Router Age Seq# Checksum
10.2.0.1 10.2.0.1 310 0x80000001 0x004303
```



*  路由表（RIB）： 
   * OSPF计算出来的路由将会加载到路由表 
   * 路由优先级 O>O IA>[O E1/N1]>[O E2/N2]

```
R2#show ip route 
O 10.3.0.0 [110/2] via 10.2.0.2, 00:06:42, FastEthernet0/0
O 172.16.1.0 [110/65] via 10.1.0.1, 00:07:27, Serial0/1/0
O 172.16.2.0 [110/65] via 10.1.0.1, 00:07:27, Serial0/1/0
```



### OSPF的邻居发现过程

![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154626.png)



### OSPF的链路状态摘要交换过程

![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154631.png)



### OSPF的详细链路状态信息同步过程

![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154635.png)



### OSPF的路由计算与路由表加载

* 每台OSPF路由器都会使用SPF算法，根据自己的LSDB独立地计算去往每个目的网络的最短路径 
  *  同步后，同一区域的OSPF路由器，LSDB一定是相同的

![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154642.png)







## OSPF的网络类型

### 广播型多路访问网络-Broadcast

* 以太网接口之下，默认的OSPF网络类型为Broadcast 
* OSPF在广播多路访问的网络上，会进行DR（Designated Router指定路由器）与BDR（Backup Designated Router）的选举 
  *  DR与BDR的能够与该链路上其他路由器（DR other）建立邻接关系，进入Full状态 
  *  DR other之间建立邻居，停留在2-way状态，不会交换LSA



![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154646.png)

### OSPF的DR、BDR机制

* DR、BDR的机制减少了在同一广播域中邻接的数量，减少了广播网络中LSA的泛洪，节省带宽 
* 例如下图的案例中
  * 如果两两建立邻接需要15对邻接【排列组合，C（6，2）=15】 
  * 如果DR机制，只需要8对邻接

![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154653.png)





### OSPF的DR、BDR的选举

* 优先级高的成为DR，其次成为BDR 

  *  Hello报文携带路由设备优先级，默认=1 
  *  优先级为0的路由设备不具备选举资格，未来也不可能为DR或者BDR

* 优先级一致的情况下，Router ID更高的成为DR，第二高的成为BDR 

  *  DR和BDR一旦选定，即使OSPF区域内新增优先级更高的路由设备，DR和BDR也不会重新选举，只有当DR和 BDR都失效后，才参与选举 
  *  优先级是基于接口的，修改命令如下

  ```
  (config)#int vlan 10
  (config-if-VLAN 10)#ip ospf priority 10
  ```

![13](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154659.png)





### DR/BDR的选举示例一

* R5后来加入网络，虽然它的Router ID比原有的DR和BDR都高，但是出于稳定性的考虑，只能成为 DRother路由器。



![14](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154704.png)



### DR/BDR的选举示例二

* 当DR失效时，BDR立刻成为新的DR 
* DRother路由器进行竞争，Router ID高成为新BDR



![15](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154708.png)





## OSPF的区域与基本配置

### OSPF的单区域问题

* 同一个区域内所有路由器为了LSDB保持完全相同，在链路发生变动的时候需要更新LSA 
  *  每台路由器都会收到的大量的LSA通告； 
  *  内部某条链路动荡，都会触发全网路由器重新执行SPF算法，导致网络不稳定； 
* 区域内路由无法汇总，需要维护的路由表越来越大，资源消耗过多，性能下降，影响数据转发

![16](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154713.png)





### OSPF的单区域问题解决方案

* 把大型网络分隔为多个较小，可管理的单元 ：区域（area）： 
  * 控制LSA只在区域内洪泛，有效地把拓扑变化控制在区域内，拓扑的变化影响限制在本区域 
  * 提高了网络的稳定性和的扩展性，有利于组建大规模的网络
  * 在区域边界可以做路由汇总，减小了路由表

![17](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154717.png)







### OSPF多区域层次化

* OSPF区域可以分为骨干区域和非骨干区域
  * 骨干区域：area 0，骨干区域在逻辑上不能被分割；
  * 非骨干区域：area 0以外的其他区域都是非骨干区域，非骨干区域必须和骨干区域相连



![18](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154721.png)



### OSPF多区域环境路由器类型

* 内部路由器IR （ Internal Area Router）：
  * 所有接口在同一个Area内 
  * 同一区域内的内部路由器，LSDB完全相同 
* 区域边界路由器ABR（ Area Border Router）：
  * 接口分属于两个或两个以上的区域，并且有一个活动接口属于area 0 
  * ABR为它们所连接的每个区域分别维护单独的LSDB 
  * 区域间路由信息必须通过ABR进行传递 
* 自制系统边界路由器ASBR（Auto System Border Router） 
  *  自制系统边界的路由器，至少有一个接口属于OSPF之外的其他路由协议



![19](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154725.png)

### 单区域OSPF案例

* OSPF路由案例1需求 
  * 在三台路由器上配置OSPF路由，配置OSPF进程号为10 
  * 配置OSPF区域全为0，使PC1和PC2能ping通PC3 





![20](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154728.png)



### 单区域OSPF案例 —— 基础配置

* 启动OSPF进程：
  *  (config)# router ospf [process ID] 
* 配置OSPF运行的接口以及接口的区域ID （建议使用精确宣告）：
  *  (config-router)# network network mask area [area ID]

```
R1(config)#router ospf 10
R1(config-router)#network 172.16.2.254 0.0.0.0 area 0
R1(config-router)#network 172.16.1.254 0.0.0.0 area 0
R1(config-router)#network 10.1.0.1 0.0.0.0 area 0
```

```
R2(config)#router ospf 10
R2(config-router)#network 10.1.0.2 0.0.0.0 area 0
R2(config-router)#network 10.2.0.1 0.0.0.0 area 0
```

```
R3(config)#router ospf 10
R3(config-router)#network 10.2.0.2 0.0.0.0 area 0
R3(config-router)#network 10.3.0.254 0.0.0.0 area 0
```



### 单区域OSPF案例 —— 路由表

* 查看R1、R2和R3的路由表 
  *  使用特权用户模式命令show ip route，显示IP路由表

```
R1#show ip route
C 10.1.0.0/30 is directly connected, GigabitEthernet 0/1
C 10.1.0.1/32 is local host. 
O 10.2.0.0/30 [110/2] via 10.1.0.2, 00:00:48, GigabitEthernet 0/1
C 172.16.1.0/24 is directly connected, GigabitEthernet 0/0
C 172.16.1.254/32 is local host. 
C 172.16.2.0/24 is directly connected, GigabitEthernet 0/2
C 172.16.2.254/32 is local host. 
O 192.168.1.0/24 [110/3] via 10.1.0.2, 00:00:32, GigabitEthernet 0/1
```

```
R2#show ip route
C 10.1.0.0/30 is directly connected, GigabitEthernet 0/0
C 10.1.0.2/32 is local host. 
C 10.2.0.0/30 is directly connected, GigabitEthernet 0/1
C 10.2.0.1/32 is local host. 
O 172.16.1.0/24 [110/2] via 10.1.0.1, 00:01:25, GigabitEthernet 0/0
O 172.16.2.0/24 [110/2] via 10.1.0.1, 00:01:25, GigabitEthernet 0/0
O 192.168.1.0/24 [110/2] via 10.2.0.2, 00:01:12, GigabitEthernet 0/1
```

```
R3#show ip route
O 10.1.0.0/30 [110/2] via 10.2.0.1, 00:01:29, GigabitEthernet 0/0
C 10.2.0.0/30 is directly connected, GigabitEthernet 0/0
C 10.2.0.2/32 is local host. 
O 172.16.1.0/24 [110/3] via 10.2.0.1, 00:01:29, GigabitEthernet 0/0
O 172.16.2.0/24 [110/3] via 10.2.0.1, 00:01:29, GigabitEthernet 0/0
C 192.168.1.0/24 is directly connected, GigabitEthernet 0/1
C 192.168.1.254/32 is local host
```

### 单区域OSPF案例 —— 连通性测试

* 测试网络连通性 
  *  PC1和PC2能ping通PC3

```
PC1>ping 192.168.1.1
Pinging 192.168.1.1 with 32 bytes of data:
Reply from 192.168.1.1: bytes=32 time=2ms TTL=125
Reply from 192.168.1.1: bytes=32 time=1ms TTL=125
Reply from 192.168.1.1: bytes=32 time=3ms TTL=125
Reply from 192.168.1.1: bytes=32 time=3ms TTL=125
Ping statistics for 192.168.1.1:
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
Minimum = 1ms, Maximum = 3ms, Average = 2ms
```

```
PC1>ping 192.168.1.1
Pinging 192.168.1.1 with 32 bytes of data:
Reply from 192.168.1.1: bytes=32 time=2ms TTL=125
Reply from 192.168.1.1: bytes=32 time=1ms TTL=125
Reply from 192.168.1.1: bytes=32 time=3ms TTL=125
Reply from 192.168.1.1: bytes=32 time=3ms TTL=125
Ping statistics for 192.168.1.1:
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
Minimum = 2ms, Maximum = 3ms, Average = 2ms
```

### 单区域OSPF案例 —— 状态查看

* 查看OSPF 协议状态：show ip protocols 
  * FULL/BDR ：表示邻接状态已经建立，并且此时本路由器为BDR角色 
* 查看OSPF邻居表：show ip ospf neighbor

```
R2#show ip protocols 
Routing Protocol is "ospf 10"
Outgoing update filter list for all interfaces is not set
Incoming update filter list for all interfaces is not set
Router ID 10.2.0.1
Memory Overflow is enabled
Router is not in overflow state now
Number of areas in this router is 1: 1 normal 0 stub 0 nssa
Routing for Networks:
10.1.0.2 0.0.0.0 area 0
10.2.0.1 0.0.0.0 area 0
Reference bandwidth unit is 100 mbps
Distance: (default is 110)
```

```
R2#show ip ospf neighbor
Neighbor ID Pri State BFD State Dead Time Address Interface
172.16.2.254 1 Full/DR - 00:00:37 10.1.0.1 GigabitEthernet 0/0
192.168.1.254 1 Full/DR - 00:00:34 10.2.0.2 GigabitEthernet 0/1
```

* 查看接口OSPF相关信息： Show ip ospf interface XX

```
R2#Show ip ospf interface gigabitEthernet 0/1
GigabitEthernet 0/1 is up, line protocol is up
Internet Address 10.2.0.1/30, Ifindex 2, Area 0.0.0.0, MTU 1500
Matching network config: 10.2.0.1/32
Process ID 10, Router ID 10.2.0.1, Network Type BROADCAST, Cost: 1
Transmit Delay is 1 sec, State BDR, Priority 1
Designated Router (ID) 192.168.1.254, Interface Address 10.2.0.2
Backup Designated Router (ID) 10.2.0.1, Interface Address 10.2.0.1
Timer intervals configured, Hello 10, Dead 40, Wait 40, Retransmit 5
Hello due in 00:00:08
Neighbor Count is 1, Adjacent neighbor count is 1
Crypt Sequence Number is 768
Hello received 50 sent 55, DD received 3 sent 6
LS-Req received 1 sent 1, LS-Upd received 4 sent 3
LS-Ack received 2 sent 3, Discarded 0
```



### 多区域OSPF案例

* OSPF路由案例2需求： 
  * 在三台路由器上配置OSPF路由，配置OSPF进程号为10
  * 按照拓扑图配置OSPF区域，使PC1和PC2能ping通PC3



![21](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220620154736.png)

### 多区域OSPF案例 —— 基本配置

* 启动OSPF进程： 

  *  Router(config)# router ospf 进程号 

* 配置OSPF运行的接口以及接口的区域ID ：

  * Router (config-router)# network 网络号 反掩码 area 区域id

  ```
  R1(config)#router ospf 10
  R1(config-router)#network 172.16.2.254 0.0.0.0 area 1
  R1(config-router)#network 172.16.1.254 0.0.0.0 area 1
  R1(config-router)#network 10.1.0.1 0.0.0.0 area 1
  ```

  ```
  R2(config)#router ospf 10
  R2(config-router)#network 10.1.0.2 0.0.0.0 area 1
  R2(config-router)#network 10.2.0.1 0.0.0.0 area 0
  ```

  ```
  R3(config)#router ospf 10
  R3(config-router)#network 10.2.0.2 0.0.0.0 area 0
  R3(config-router)#network 10.3.0.254 0.0.0.0 area 0
  ```

  ### 多区域OSPF案例 —— 路由表

* 使用特权用户模式命令show ip route，显示IP路由表

  *  “O”表示区域内部路由 
  *  “O IA”表示区域间路由

*  查看R1、R2和R3的路由表



```
R1#show ip route
C 10.1.0.0/30 is directly connected, GigabitEthernet 0/1
C 10.1.0.1/32 is local host. 
O IA 10.2.0.0/30 [110/2] via 10.1.0.2, 00:00:19, GigabitEthernet 0/1
C 172.16.1.0/24 is directly connected, GigabitEthernet 0/0
C 172.16.1.254/32 is local host. 
C 172.16.2.0/24 is directly connected, GigabitEthernet 0/2
C 172.16.2.254/32 is local host. 
O IA 192.168.1.0/24 [110/3] via 10.1.0.2, 00:00:19, GigabitEthernet 0/1
```

```
R2#show ip route
C 10.1.0.0/30 is directly connected, GigabitEthernet 0/0
C 10.1.0.2/32 is local host. 
C 10.2.0.0/30 is directly connected, GigabitEthernet 0/1
C 10.2.0.1/32 is local host. 
O 172.16.1.0/24 [110/2] via 10.1.0.1, 00:01:07, GigabitEthernet 0/0
O 172.16.2.0/24 [110/2] via 10.1.0.1, 00:01:07, GigabitEthernet 0/0
O 192.168.1.0/24 [110/2] via 10.2.0.2, 00:15:52, GigabitEthernet 0/1
```

```
R3#show ip route
O IA 10.1.0.0/30 [110/2] via 10.2.0.1, 00:01:45, GigabitEthernet 0/0
C 10.2.0.0/30 is directly connected, GigabitEthernet 0/0
C 10.2.0.2/32 is local host. 
O IA 172.16.1.0/24 [110/3] via 10.2.0.1, 00:01:36, GigabitEthernet 0/0
O IA 172.16.2.0/24 [110/3] via 10.2.0.1, 00:01:36, GigabitEthernet 0/0
C 192.168.1.0/24 is directly connected, GigabitEthernet 0/1
C 192.168.1.254/32 is local host.
```

### 多区域OSPF案例 —— 连通性测试

* 测试网络连通性 
  * PC1和PC2能ping通PC3

```
PC1>ping 192.168.1.1
Pinging 192.168.1.1 with 32 bytes of data:
Reply from 192.168.1.1 : bytes=32 time=2ms TTL=125
Reply from 192.168.1.1 : bytes=32 time=1ms TTL=125
Reply from 192.168.1.1 : bytes=32 time=3ms TTL=125
Reply from 192.168.1.1 : bytes=32 time=3ms TTL=125
Ping statistics for 10.3.0.1:
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
Minimum = 1ms, Maximum = 3ms, Average = 2ms
```

```
PC2>ping 192.168.1.1
Pinging 192.168.1.1 with 32 bytes of data:
Reply from 192.168.1.1 : bytes=32 time=2ms TTL=125
Reply from 192.168.1.1 : bytes=32 time=1ms TTL=125
Reply from 192.168.1.1 : bytes=32 time=3ms TTL=125
Reply from 192.168.1.1 : bytes=32 time=3ms TTL=125
Ping statistics for 10.3.0.1:
Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
Approximate round trip times in milli-seconds:
Minimum = 1ms, Maximum = 3ms, Average = 2ms
```
