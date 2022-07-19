---
title: 胖瘦AP组网方案
tags:
  - 锐捷
categories:
  - 学习
  - 锐捷
abbrlink: 82dddbd2
date: 2022-06-25 15:15:16
---

<div class="danger">

>  资料来源[锐捷](https://talent.ruijie.com.cn/certification/profession/)
>  非原创

</div>



.





## 无线“胖瘦”组网模式简介

### 基本概念介绍

* AC：无线网络控制器。在瘦AP架构的WLAN网络中扮演管理AP的角色。
* AP：无线访问接入点。在瘦AP架构的WLAN网络中提供接入服务，通常分布在无线网络服务区的多个地 方，用于覆盖该服务区，提供无线服务
* POE交换机：对AP进行供电，以及数据传输 
* STA：无线访问站点。带有无线网卡的终端。

### 胖AP概述

* FAT AP俗称“胖AP”，下文也称“胖AP” 
* 胖AP的特点： 
  *  将WLAN物理层、数据加密、认证、QoS、网络管理、L2漫游集于一身 
  *  通常AP都会有单独的网页或者命令行配置页面 
  *  用户使用时，无需其他附属设备，采用单独的AP即可提供无线接入功能，组网相对简单



![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151123.png)



### 胖AP组网模式

* 胖AP组网模式 
  *  家庭或者SOHO（Small Office and Home Office）组网通常采用胖AP的组网方式；每个AP需要单独的配置 操作，出现网络问题需要单独进行排查。 
  *  传统的企业无线网采用的是胖AP的组网模式，管理操作极为困难。
  *  大型的胖AP组网中，可使用MACC进行统一的管理操作。



![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151121.png)

### 瘦AP概述



* FIT AP俗称“瘦AP”，下文也称“瘦AP” 
* Access Controller：无线控制器，对AP进行统一管控 
  * 接入管理，认证安全，报文转发，射频管理，漫游等 
* 瘦 AP的特点：
  *  集中管理，配置统一下发 
  *  射频统一优化管理 
  *  安全认证策略全面 
  *  L2、L3漫游，适合大规模组网

![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151141.png)







### 瘦AP组网模式

* 瘦AP在使用中必须和AC配合使用 
* 瘦AP负责无线接入、加密、认证中的部分功能 
* 射频管理、用户接入、AP控制，漫游控制都在无线控制器上完成
* AC通过和AP之间建立CAPWAP隧道来控制和管理AP

> 自2002年廋AP架构成为WLAN业界新的趋势后，WLAN组网开 始通过无线控制器（AC）来管理多个AP。AP和AC间采用各厂家私 有的隧道协议进行通讯，这就造成了不同厂家AP与AC互通问题。为 了 解 决 隧 道 协 议 的 不 兼 容 问 题 ， IETF 在 2005 年成立了 CAPWAP(Control and Provisioning of Wireless Access Points) 工作组以标准化AP和AC间的隧道协议（RFC5415）

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151144.png)









### 组网模式优劣势比较

* 胖 AP与瘦 AP组网模式优劣势比较

| 属性         | 胖 AP                                               | 瘦 AP                                                        |
| ------------ | --------------------------------------------------- | ------------------------------------------------------------ |
| 技术模式     | 传统                                                | 新型，管理加强                                               |
| 安全性       | 单点安全，无整网统一安全能力                        | 统一的安全防护体系，无线入侵检测                             |
| 网络管理能力 | 单台管理                                            | 统一管理                                                     |
| 配置管理     | 每个AP需要单独配置，管理复杂                        | 配置统一下发，AP零配置                                       |
| 自动RF调节   | 没有RF自动调节能力                                  | 自动优化无线网络配置                                         |
| 漫游能力     | 支持2层漫游功能，适合小规模组网                     | 支持2层、3层快速安全漫游                                     |
| 可扩展性     | 无扩展能力                                          | 方便扩展，对于新增AP无需任何配置管理                         |
| 高级功能     | 对于基于WiFi的高级功能，如安全、语音等支持能力很 差 | 可针对用户提供安全、语音、位置业务、个性化页 面推送、基于用户的业务/完全/服务质量控制等等 |

## 胖AP配置

### AP设备介绍

![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151148.png)







* Fit模式下，LAN1口和LAN2口IP地址均为192.168.110.1/24 
* Fat模式下，LAN1口IP地址为192.168.110.1/24; LAN2口IP地址为192.168.111.1/24



![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151154.png)



### 胖AP配置环境说明

* 配置信息规划： 
  *  AP的地址为172.16.1.253/24，网关172.16.1.254/24 
  *  无线用户地址段为172.16.1.0/24，网关172.16.1.254/24 
  *  无线用户和AP vlan都使用vlan1 
  *  无线ssid名称 ruijie







![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151158.png)



### 胖AP配置思路

* 配置要点
  *  连接好网络拓扑，保证AP能正常供电，正常开机； 
  *  保证要接AP的网线接在电脑上，电脑可以使用网络，使用ping测试； 
  *  完成AP基本配置后验证无线SSID 能否被无线用户端正常搜索发现到； 
  *  无线用户端连接，并验证网络连通性；  AP其他可选配置（信道优化、黑白名单、无线的认证及加密方式等等） 
* 注意： 
  *  AC：没有默认密码。 
  *  胖AP：console密码是admin，没有默认enable密码。
  *  瘦AP：console密码是ruijie，enable密码是apdebug





### 胖AP配置CLI版 —— 步骤

1. 切换AP为胖模式并创建用户VLAN



![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151202.png)





```
Ruijie#ap-mode fat // 切换AP的模式为胖模式
Ruijie(config)#vlan 1
Ruijie(config-vlan)#name sta
```



2. 配置无线信号

* 创建指定ssid的wlan，在指定无线子接口绑定该wlan以使能发出无线信号

```
Ruijie(config)#dot11 wlan 1
Ruijie(dot11-wlan-config)#vlan 1 //关联vlan1
Ruijie(dot11-wlan-config)#broadcast-ssid //广播SSID
Ruijie(dot11-wlan-config)#ssid ruijie //无线信号名称为ruijie
Ruijie(dot11-wlan-config)#exit
```



* 在指定无线子接口绑定该wlan以使能发出无线信号：

```
Ruijie(config)#interface Dot11radio 1/0.1 //在射频卡的子接口封装vlan
Ruijie(config-if-Dot11radio 1/0.1)#encapsulation dot1Q 1 //指定AP射频子接口VLAN
Ruijie(config-if-Dot11radio 1/0.1)#wlan-id 1 //在AP射频子接口使能VLAN
Ruijie(config-if-Dot11radio 1/0.1)#exit
```

 3.配置AP接口、管理地址和路由 

*  配置AP的以太网接口,让无线用户的数据可以正常传输



```
Ruijie(config)#interface GigabitEthernet 0/1
Ruijie(config-if-GigabitEthernet 0/1)#encapsulation dot1Q 1 //指定AP有线口VLAN
Ruijie(config-if-GigabitEthernet 0/1)#exit
注意：要封装相应的vlan，否则无法通信
```

* 配置interface vlan地址

```
Ruijie(config)#interface BVI 1 //配置管理地址接口，VLAN 1对应BVI 1
Ruijie(config-if-BVI 1)#ip address 172.16.1.253 255.255.255.0 //该地址只能用于管理
```

* 配置静态路由

```
Ruijie(config)#ip route 0.0.0.0 0.0.0.0 172.16.1.254
```

4. 配置无线用户DHCP 

* 给连接的无线分配地址 
* 如网络中已经存在DHCP服务器可跳过此配置，一般接入部署时上联设备都已经配置好无线用户的dhcp池



```
Ruijie(config)#service dhcp //开启DHCP服务
Ruijie(config)#ip dhcp excluded-address 172.16.1.253 172.16.1.254 //不下发地址范围
Ruijie(config)#ip dhcp pool test //配置DHCP地址池，名称是 “test”
Ruijie(dhcp-config)#network 172.16.1.0 255.255.255.0 //下发172.16.1.0地址段给无线用户
Ruijie(dhcp-config)#dns-server 8.8.8.8 //下发DNS地址
Ruijie(dhcp-config)#default-router 172.16.1.254 //下发网关
```

* 确认配置正确，保存配置

```
Ruijie(config)#end
Ruijie#write （胖AP配置完后需要保存配置，否则重启配置丢失）
```

### 胖AP配置WEB版 —— 步骤

1. 切换AP为胖模式

* http://192.168.110.1，用户名：admin；密码：admin 
* 然后选择胖模式

![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151209.png)









2. 配置无线信号



![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151214.png)







3. 配置AP工作模式和管理vlan





![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151218.png)



4. 配置无线用户DHCP

![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151221.png)







### 查看胖AP状态



![13](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151227.png)





### 查看用户状态



![14](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151229.png)







## 瘦AP配置

### 瘦AP架构工作原理（集中式转发）

* 工作原理简介，9个步骤 
  *  有线网络搭建（VLAN、DHCP、路由等）
  *  AP零配置启动，通过DHCP获取IP地址及网关IP，同时通过option 138获取AC IP地址 
  *  AP主动建立到达AC的CAPWAP隧道 
  *  AP与AC建立隧道成功后，AC下发配置信息给AP 
  *  AP获取配置后，广播SSID供STA关联并接入STA  AP将STA发出的802.11数据转换为以太数据并通过CAPWAP隧道转发给AC 
  *  AC将收到的数据解封装并进行转发至有线网络中 
  *  有线网络返回数据到AC，AC将数据通过CAPWAP隧道转发至AP 
  *  AP根据配置信息将以太数据转换为802.11数据，转发给STA



### 瘦AP集中转发工作原理 

1.有线网络搭建（VLAN、DHCP、路由等）



![15](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151243.png)





### AP发现AC建立隧道连接

* 部署无线接入点之前，通常不需要对AP进行任何配置。只需在无线控制器统一集中配置，这种方式简化了 部署及配置过程。因此，AP启动后，需要首先寻找无线控制器的地址，以连接到无线控制器，以接受统一 管理，获取配置，创建数据隧道等。
* AP发现AC常用的有以下这几种方式：DHCP、DNS、静态配置AC地址 
  * DHCP：使用option 138字段 
  * DNS ：AP解析的DNS域名默认是ac.ruijie.com.cn，在DNS服务器的域名解析里添加该条解析即可。
  * 静态配置AC地址





2. AP零配置启动，通过DHCP获取IP地址及网关IP，同时通过option138获取AC IP地址



![16](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151247.png)





3. AP主动建立到达AC的CAPWAP隧道 

*  CAPWAP：无线接入点控制与配置协议



![17](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151251.png)



4. AP与AC建立隧道成功后，AC下发配置信息给AP





![18](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151256.png)







5. AP获取配置后，广播SSID供STA关联并接入STA

![19](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151300.png)







6. AP将STA发出的802.11数据转换为以太数据并通过CAPWAP隧道转发给AC



![20](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151303.png)







7. AC将收到的数据解封装并进行转发至有线网络中



![21](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151306.png)



8. 有线网络返回数据到AC，AC将数据通过CAPWAP隧道转发至AP



![22](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151310.png)





9. AP根据配置信息将以太数据转换为802.11数据，转发给STA





![23](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151315.png)

### 瘦AP架构配置步骤（集中式转发）

*  配置过程介绍，5个步骤 
   *  根据有线网络结构确定AC的部署位置及连接方式 
   *  制作AP信息表，包括：AP位置、命名、型号、MAC地址、SSID、AP所属VLAN及网段、无线用户VLAN及 网段、所连接POE交换机及端口等信息 
   *  DHCP规划、路由规划
   *  WLAN设备配置 
      *  AC的接口及路由配置 
      *  AC、AP的软件升级 
      *  AC上完成AP配置信息 
      *  POE交换机对AP供电
   *  WLAN设备配置总结





### 瘦AP集中转发配置 —— 步骤一

1.根据有线网络结构确定AC的部署位置及连接方式 

* 单核心二层结构：AC与核心之间双线互连



![24](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151318.png)







* 单核心二层结构：AC与核心之间单线互连





![25](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151321.png)



### 瘦AP集中转发配置 —— 步骤二



2.制作AP信息表





![26](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151325.png)



### 瘦AP集中转发配置CLI版本 —— 步骤三

3.DHCP及路由规划 

* AP网段和无线用户网段的DHCP服务器规划 
  *  网关交换机作为DHCP服务器 
  *  AP的地址池中需要定义Option 138选项，内容为AC的Loopback0地址 
  *  配置STA的地址池（略）





![27](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151328.png)







### 瘦AP集中转发配置CLI版本 —— 步骤四

4. WLAN设备配置 

   

   4.1 在AC定义WLAN

```
AC(config)# wlan-config 10 vlan-10 @ruijie. //WLAN ID号是10,描述是vlan-10，SSID名字是 @ruijie
AC(config)#wlan-config 10 vlan-10 //WLAN ID号是10
AC(config-wlan)#ssid @ruijie //WLAN 的SSID是@ruijie
AC(config-wlan)#band-select enable //开启SSID广播功能
```

	4.2 在AC上定义VLAN

```
AC(config)# vlan 10 //创建无线用户VLAN
AC(config-vlan)#name STA
AC(config)# interface vlan 10 //创建无线用户VLAN的三层SVI接口
AC(config-vlan)#name STA
AC(config-if-VLAN 10)#ip address 10.0.0.253 255.255.255.0
AC(config)#int loopback 0 //创建回还口，配置capwap地址
AC(config-if-Loopback 0)#ip address 1.1.1.1 24
```







	4.3 在AC上定义AP Group

*  将无线用户所属的WLAN与VLAN进行关联映射 
*  一个group下面可以配置多个WLAN与VLAN的映射关系



![28](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151332.png)









	4.4 在POE交换机连接AP的接口上打开POE供电



```
SW_Hexin(config)# interface FastEthernet 0/1
SW_Hexin(config-if-GigabitEthernet 0/1)#poe enable //开启POE供电功能
SW_Hexin(config-if-GigabitEthernet 0/1)# switchport access vlan 20 //AP所属VLAN
```





 4.5 AP加组 

* 等待AP与AC建立CAPWAP隧道后，AP的信息会自动出现在AC的配置中 
* 如下所示,为一台RG-AP520-I关联上AC之后,在AC配置中自动出现的信息

![29](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151335.png)







4.6 定义WLAN的安全参数（可选） 

* RSN(WPA 2)---推荐使用PSK认证和AES加密

```
AC(config)#wlansec 10 
AC(config-wlansec)#security rsn enable 
AC(config-wlansec)#security rsn ciphers aes enable 
AC(config-wlansec)#security rsn akm psk enable 
AC(config-wlansec)#security rsn akm psk set-key ascii 12345678
```

4.7 查看CAPWAP隧道状态



![30](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151338.png)





* 如果上述信息为空或者缺少部分信息: 
* 1、检查POE交换机是否对相应AP供电,接口是否poe enable 
* 2、检查AP的DHCP分配信息show ip dhcp bindings 
* 3、路由是否可达（AP到AC lo0地址的路由）： telnet 至AP,密码为ruijie，在AP上ping AC的Loopback0



4.8 查看AP上线：show ap-config summary 

```
Ruijie-AC#show ap-config summary 
========= show ap status =========
Radio: Radio ID or Band: 2.4G = 1#, 5G = 2#
	E = enabled, D = disabled, N = Not exist
	Current Sta number
	Channel: * = Global
	Power Level = Percent
	
Online AP number: 1
Offline AP number: 0

AP Name IP Address Mac Address Radio Radio Up/Off time State
--------------- ------ ---------- ------------- --------------- ---- ----------- ----------- -----
0074.9c5a.f76c 20.0.0.1 0074.9c5a.f76c 1 E 0 6* 100 2 E 0 153* 100 0:01:15:54 Run
```

4.9 查看无线用户上线：show ac-config client 



```
Ruijie-AC#show ac-config client 
========= show sta status =========
AP : ap name/radio id
Status: Speed/Power Save/Work Mode/Roaming State, E = enable power save, D = disable power save
Total Sta Num : 1

STA MAC IPV4 Address AP Wlan Vlan Status Asso Auth Net Auth Up time 
-------------- ----------- ---------------- ----- ----- ---- ------- ------ ---- -------- ---------
dc0c.5cd6.60b3 10.0.0.2 0074.9c5a.f76c/2 10 10 192.0M/D/ac WPA2_PSK OPEN 0:00:04:58
```





### 瘦AP集中转发配置WEB版本 —— 步骤

1. WLAN设备配置 

   在AC上定义AP Group



![31](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151342.png)







2. WLAN设备配置 

   在AC定义WLAN







![32](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151348.png)





3. WLAN设备配置 

   在AC上定义VLAN







![33](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151351.png)









4. WLAN设备配置 

   查看AP上线



![34](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220625151403.png)



### 瘦AP集中转发配置 —— 配置小结



 瘦AP配置小结： 

* 目前企业组网采用AC+瘦 AP架构； 
* 瘦 AP分为集中转发与本地两种工作方式；
* AC与AP之间运行CAPWAP协议，AC与AP之间可以是二层组网、三层组网、穿透NAT等；
* CAPWAP隧道由控制隧道和数据隧道组成，控制隧道负责AP的升级、配置、射频管理等功能，数据隧道负责 数据业务的封装转发 
* CAPWAP建立过程分为：AP获取IP地址与AC的IP地址、AP发现AC、AP请求加入AC、AP自动升级、AP配置 下发、AP配置确认、通过CAPWAP隧道转发数据； 
* 集中转发：数据流通过AC，并且由AC解封装CAPWAP协议； 
* 本地转发：数据流不通过AC，没有CAPWAP数据隧道；
