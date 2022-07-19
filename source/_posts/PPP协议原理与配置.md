---
title: PPP协议原理与配置
tags:
  - 锐捷
categories:
  - 学习
  - 锐捷
abbrlink: cea6a88e
date: 2022-06-22 17:09:10
---







<div class="danger">

>  资料来源[锐捷](https://talent.ruijie.com.cn/certification/profession/)
>  非原创

</div>







## PPP协议简介

### 前言

* 点对点协议（Point to Point Protocol，PPP）为在点对点连接上传输多协议数据包提供了一个标准方法。
* PPP 最初设计是为两个对等节点之间的 IP 流量传输提供一种封装协议。



### PPP协议简介

* 广域网中的PPP协议： 
  *  通常使用在专线的点对点线路中，不需要使用MAC地址（MAC地址是以太网的内容） 
  *  PPP与以太网一样工作在OSI模型的数据链路层，使用PPP协议封装数据



![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162312.png)







### PPP协议层次介绍

* PPP协议层介绍：
  * PPP协议包含两个子协议：链路层控制协议LCP、网络层控制协议NCP 
  * PPP协议的NCP部分上跨到了OSI参考模型的第三层，因此PPP还具有部分网络层的功能



![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162314.png)



## PPP协议工作原理

* PPP会话建立：
  *  LCP链路建立协商阶段
  *  认证阶段（可选）：PAP和CHAP两种认证方式 
  *  NCP网络层协议协商阶段





![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162319.png)





### PPP会话建立协商过程



* 阶段1：LCP协商阶段







![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162324.png)

* 阶段2：身份认证阶段（可选）









![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162328.png)



* 阶段3：IPCP协商阶段



![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162332.png)







### 课程小结

* PPP协议通常工作在广域网的点对点链路中，PPP的通信无需MAC地址 
* PPP工作在OSI参考模型中的数据链路层，包含2个子协议，分别是LCP和NCP
* PPP会话建立有三个阶段，分别是LCP协商阶段、身份认证阶段（可选）、NCP协商阶段



## PPP协议基础配置



### PPP基础命令

* 双方接口封装PPP

```
Router(config-if)# encapsulation ppp
```

* 认证方配置对端的用户名和密码

```
Router(config)# username name password password
```

* 认证方配置PPP认证方式（启用PAP或者CHAP认证）

```
Router(config-if)# ppp authentication {chap | chap pap | pap chap | pap}
```

### PPP典型配置案例（PAP单向认证）

* ppp authentication pap命令指定本地为验证方，验证方需要配置被验证方的用户名密码列表。

![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162337.png)





* PAP认证，接口状态验证

![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162342.png)





### PPP典型配置案例（PAP双向认证）

* ppp authentication pap命令指定本地为验证方，验证方需要配置被验证方的用户名密码列表。



![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162347.png)

```
username ruijie2 password 0 ruijie2
interface Serial1/0
ip address 1.1.1.1 255.255.255.0
encapsulation ppp
ppp authentication pap
ppp pap sent-username ruijie1 password 0 ruijie1
```

```
username ruijie1 password 0 ruijie1
interface Serial2/0
encapsulation ppp
ppp authentication pap
ip address 1.1.1.2 255.255.255.0
ppp pap sent-username ruijie2 password 0 ruijie2
```



* 接口状态信息验证

![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162351.png)





```
R1#show interface serial 1/0
Index(dec):35 (hex):23
Serial 1/0 is UP , line protocol is UP
Hardware is Serial
Interface address is: 1.1.1.1/24
MTU 1500 bytes, BW 2000 Kbit
Encapsulation protocol is PPP, loopback not set
Keepalive interval is 10 sec ,retries 10.
Carrier delay is 2 sec
Rxload is 1/255, Txload is 1/255
LCP Open
Queueing strategy: FIFO
Output queue 0/40, 0 drops;
Input queue 0/75, 0 drops
5 minutes input rate 0 bits/sec, 0 packets/sec
5 minutes output rate 0 bits/sec, 0 packets/sec
38021 packets input, 5656110 bytes, 0 no buffer, 0 dropped
Received 23488 broadcasts, 0 runts, 0 giants
0 input errors, 0 CRC, 0 frame, 0 overrun, 0 abort
38097packets output, 2135697bytes, 0 underruns , 0 dropped
```

````
R1#show interface serial 2/0
Index(dec):35 (hex):23
Serial 2/0 is UP , line protocol is UP
Hardware is Serial
Interface address is: 1.1.1.2/24
MTU 1500 bytes, BW 2000 Kbit
Encapsulation protocol is PPP, loopback not set
Keepalive interval is 10 sec ,retries 10.
Carrier delay is 2 sec
Rxload is 1/255, Txload is 1/255
LCP Open
Queueing strategy: FIFO
Output queue 0/40, 0 drops;
Input queue 0/75, 0 drops
5 minutes input rate 0 bits/sec, 0 packets/sec
5 minutes output rate 0 bits/sec, 0 packets/sec
38325 packets input, 5655320 bytes, 0 no buffer, 0 dropped
Received 23358 broadcasts, 0 runts, 0 giants
0 input errors, 0 CRC, 0 frame, 0 overrun, 0 abort
38235packets output, 2135895bytes, 0 underruns , 0 dropped
````

### PPP典型配置案例（CHAP单向认证）

* ppp authentication chap命令指定本地为验证方，验证方需要配置被验证方的用户名密码列表。

![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162356.png)



```
interface Serial1/0
ip address 1.1.1.1 255.255.255.0
encapsulation ppp
ppp chap hostname ruijie1
ppp chap password 0 ruijie1
```

```
username ruijie1 password 0 ruijie1
interface Serial2/0
encapsulation ppp
ip address 1.1.1.2 255.255.255.0
ppp authentication chap
```





* 接口状态信息验证



![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162401.png)



```
R1#show interface serial 1/0
Index(dec):35 (hex):23
Serial 1/0 is UP , line protocol is UP
Hardware is Serial
Interface address is: 1.1.1.1/24
MTU 1500 bytes, BW 2000 Kbit
Encapsulation protocol is PPP, loopback not set
Keepalive interval is 10 sec ,retries 10.
Carrier delay is 2 sec
Rxload is 1/255, Txload is 1/255
LCP Open
Queueing strategy: FIFO
Output queue 0/40, 0 drops;
Input queue 0/75, 0 drops
5 minutes input rate 0 bits/sec, 0 packets/sec
5 minutes output rate 0 bits/sec, 0 packets/sec
38021 packets input, 5656110 bytes, 0 no buffer, 0 dropped
Received 23488 broadcasts, 0 runts, 0 giants
0 input errors, 0 CRC, 0 frame, 0 overrun, 0 abort
38097packets output, 2135697bytes, 0 underruns , 0 dropped
```

````
R1#show interface serial 2/0
Index(dec):35 (hex):23
Serial 2/0 is UP , line protocol is UP
Hardware is Serial
Interface address is: 1.1.1.2/24
MTU 1500 bytes, BW 2000 Kbit
Encapsulation protocol is PPP, loopback not set
Keepalive interval is 10 sec ,retries 10.
Carrier delay is 2 sec
Rxload is 1/255, Txload is 1/255
LCP Open
Queueing strategy: FIFO
Output queue 0/40, 0 drops;
Input queue 0/75, 0 drops
5 minutes input rate 0 bits/sec, 0 packets/sec
5 minutes output rate 0 bits/sec, 0 packets/sec
38325 packets input, 5655320 bytes, 0 no buffer, 0 dropped
Received 23358 broadcasts, 0 runts, 0 giants
0 input errors, 0 CRC, 0 frame, 0 overrun, 0 abort
38235packets output, 2135895bytes, 0 underruns , 0 dropped
````

### PPP典型配置案例（CHAP双向认证）

* ppp authentication chap命令指定本地为验证方，验证方需要配置被验证方的用户名密码列表。





![13](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162408.png)

```
username ruijie2 password 0 ruijie2
interface Serial1/0
ip address 1.1.1.1 255.255.255.0
encapsulation ppp
ppp authentication chap
ppp chap hostname ruijie1
ppp chap password 0 ruijie1
```



```
username ruijie1 password 0 ruijie1
interface Serial2/0
ip address 1.1.1.2 255.255.255.0
encapsulation ppp
ppp authentication chap
ppp chap hostname ruijie2
ppp chap password 0 ruijie2
```

* 接口状态信息验证



![14](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220622162411.png)





```
R1#show interface serial 1/0
Index(dec):35 (hex):23
Serial 1/0 is UP , line protocol is UP
Hardware is Serial
Interface address is: 1.1.1.1/24
MTU 1500 bytes, BW 2000 Kbit
Encapsulation protocol is PPP, loopback not set
Keepalive interval is 10 sec ,retries 10.
Carrier delay is 2 sec
Rxload is 1/255, Txload is 1/255
LCP Open
Queueing strategy: FIFO
Output queue 0/40, 0 drops;
Input queue 0/75, 0 drops
5 minutes input rate 0 bits/sec, 0 packets/sec
5 minutes output rate 0 bits/sec, 0 packets/sec
38021 packets input, 5656110 bytes, 0 no buffer, 0 dropped
Received 23488 broadcasts, 0 runts, 0 giants
0 input errors, 0 CRC, 0 frame, 0 overrun, 0 abort
38097packets output, 2135697bytes, 0 underruns , 0 dropped
```

```
R1#show interface serial 2/0
Index(dec):35 (hex):23
Serial 2/0 is UP , line protocol is UP
Hardware is Serial
Interface address is: 1.1.1.2/24
MTU 1500 bytes, BW 2000 Kbit
Encapsulation protocol is PPP, loopback not set
Keepalive interval is 10 sec ,retries 10.
Carrier delay is 2 sec
Rxload is 1/255, Txload is 1/255
LCP Open
Queueing strategy: FIFO
Output queue 0/40, 0 drops;
Input queue 0/75, 0 drops
5 minutes input rate 0 bits/sec, 0 packets/sec
5 minutes output rate 0 bits/sec, 0 packets/sec
38325 packets input, 5655320 bytes, 0 no buffer, 0 dropped
Received 23358 broadcasts, 0 runts, 0 giants
0 input errors, 0 CRC, 0 frame, 0 overrun, 0 abort
38235packets output, 2135895bytes, 0 underruns , 0 dropped
```
