---
title: ARP协议原理与应用
tags:
  - 锐捷
categories:
  - 学习
  - 锐捷
abbrlink: 9f9ac9a
date: 2022-06-07 10:54:54
---

<div class="danger">

>  资料来源[锐捷](https://talent.ruijie.com.cn/certification/profession/)
>  非原创

</div>

## 前言

* OSI模型把网络工作分为七层，IP地址在OSI模型的第三层，MAC地址在第二层，彼此不直接打交道。在 通过以太网发送IP数据包时，需要先封装第三层、第二层的包头，但由于发送时只知道目标IP地址，不知 道其MAC地址，又不能跨第二、三层，所以需要使用地址解析协议即ARP协议。
* ARP协议可根据网络层IP数据包包头中的IP地址信息解析出目标硬件地址（MAC地址）信息，以保证通信 的顺利进行。



## ARP协议概述

* 数据要在以太网中传输，需要完成以太网封装，这项工作由网络层负责；
* 要完成以太网的数据封装，需要知道目的设备的MAC地址；

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105555.png)



* ARP 
  * Address Resolution Protocol 地址解析协 议
  * 作用：将 IP地址解析为 MAC地址
  * 注意：ARP报文不能穿越路由器，不能被转 发到其他广播域 
* ARP缓存表
  * 用于存储IP地址及其经过解析的MAC地址的 对应关系

![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105558.png)



## ARP协议工作原理

ARP工作流程

![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105601.png)

ARP工作原理

* 先查看ARP表，如果ARP表中没有目的IP地址对应的MAC表项，则发送ARP请求包；
* 源主机广播发送ARP request 数据包，请求目的主机的MAC地址；
* 同网段内的所有主机都能收到ARP request请求包，但只有目的主机才会回复ARP reply数据包；
* 源主机收到ARP reply后，将目的主句的IP-MAC对应关系添加进ARP表中，完成数据的以太网封装，进行 数据交互

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105604.png)

ARP缓存表

* 动态表项
  * 通过ARP协议学习，能被更新，缺省老化时间120s 
* 静态表项
  * 手工配置，不能被更新，无老化时间的限制




ARP协议分类

免费ARP（Gratuitous ARP ）

* 发送ARP请求，请求本机IP对应的MAC 
  *  免费ARP的作用：确定其它设备的 IP地址是否与本机 IP地址冲突 
  *  更改了地址，通知其他设备更新 ARP表项



代理ARP（Proxy ARP ）

* 由启动了代理ARP功能的网关/下一跳设备代为应答ARP请求，该ARP请求的是其他IP对应的MAC地址。
* 回应ARP请求的条件
  * 本地有去往目的IP的路由表 
  * 收到该ARP请求的接口与路由表下一跳不是同一个接口



RARP与IARP

* RARP  Reverse Address Resolution Protocol 反向地址解析协议 
  * 把MAC地址解析为IP地址 
  * 应用场景：常用于无盘工作站 
* IARP 
  * Inverse Address Resolution Protocol 逆向地址解析协议 
  * 在帧中继网络中解析对端IP地址和本地DLCL的映射关系
  * 应用场景：应用于帧中继网络
