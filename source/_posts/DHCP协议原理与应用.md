---
title: DHCP协议原理与应用
tags:
  - 锐捷
categories:
  - 学习
  - 锐捷
abbrlink: ca4e8c36
date: 2022-06-07 10:54:31
---

<div class="danger">

>  资料来源[锐捷](https://talent.ruijie.com.cn/certification/profession/)
>  非原创

</div>



# DHCP协议原理与应用

## 前言

DHCP（Dynamic Host Configuration Protocol，动态主机配置协议）通常被应用在大型的局域网络环 境中，主要作用是集中的管理、分配IP地址，使网络环境中的主机动态的获得IP地址、Gateway地址、 DNS服务器地址等信息，并能够提升地址的使用率。

## DHCP协议概述

![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105615.png)



![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105617.png)



![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105619.png)

## DHCP协议工作原理

DHCP简介

* DHCP（Dynamic Host Configuration Protocol），动态主机配置协议
* 定义在RFC2131中， C/S架构，为终端设备提供TCP/IP参数（IP地址、掩码、网关、DNS等）的自动配置。
* DHCP报文格式和BootP（RFC951、RFC1542）报文兼容，保证了互操作



## DHCP协议名词解释

* DHCP Client 
  * DHCP客户机，即用户终端设备，可以是手机、电脑、打印机等需要接入网络的终端设备 
* DHCP Server 
  * DHCP服务器，为终端分配网络参数，管理地址池



DHCP服务器配置



![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105621.png)

PC的DHCP设置

![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105624.png)

![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105626.png)

DHCP协议工作过程

![13](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105627.png)





DHCP协议报文及用途

| 报文类型      | 用途                                          |
| ------------- | --------------------------------------------- |
| DHCP discover | 客户端广播查找可用服务器                      |
| DHCP offer    | 服务器响应DHCP discover报文，分配相应配置参数 |
| DHCP request  | 客户端请求配置参数、请求配置确认、续租约      |
| DHCP ack      | 服务器确认DHCP request报文                    |
| DHCP decline  | 客户端发现地址被使用时，通知服务器            |
| DHCP release  | 客户端释放地址时通知服务器的报文              |
| DHCP inform   | 客户端已有IP地址，请求更详细配置参数          |
| DHCP nak      | 服务器告诉客户端地址请求不正确或租期已过期    |

DHCP协议报文及用途

![14](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105631.png)



DHCP报文介绍 —— DHCP Offer

![15](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105632.png)



DHCP报文介绍 —— DHCP Request

![16](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105633.png)



DHCP报文介绍 —— DHCP Ack

![17](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105635.png)



DHCP协议的租约

* 租约50%时刻
  * 客户端主动向服务器发送DHCP request报文，请求更新租约时间； 
    *  若服务器可用，回复DHCP ack，更新租约； 
    *  若服务器不可用，回复DHCP nak，不更新租约；
* 租约87.5%时刻 
  * 客户端主动向服务器发送DHCP request报文，请求更新租约时间； 
    *  若服务器可用，回复DHCP ack，更新租约； 
    *  若服务器不可用，回复DHCP nak，不更新租约； 
* 待到租约时间过去，客户端会重新发送DHCP discover报文。



### DHCP常见部署方式

校园网中常见的DHCP服务部署方式

* 方式一：网关交换机作为DHCP服务器 
  * 优势：节省一台服务器资源 
  * 劣势：地址池配置分散在网络中多台汇聚网关交换机上，无法集中管理 
* 方式二：网关交换机作为DHCP中继，在服务器区部署一台专用的DHCP服务器
  * 优势：集中管理，不需要每一个网段都配置一个DHCP服务器，节约资源 
  * 劣势：需要占用一台服务器资源



方式一：网关交换机作为DHCP服务器

![18](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105637.png)



方式二：部署专用DHCP服务器

![19](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105640.png)



DHCP相关安全设计

* 用户使用静态IP地址接入
  *  部分用户手动配置IP地址，但DHCP服务器是不知道的，因此在为DHCP用户分配IP地址的时候，用户通过 DHCP获取到的IP地址，在网络中是已经使用的，导致了IP地址冲突。
* 用户架设非法DHCP服务器
  *  在同一VLAN中，如果存在恶意用户私自架设了一台DHCP服务器，那么将会使该VLAN内的用户获取到错误 的IP地址，导致无法接入进校园网





使用IP source guard解决用户手动配置IP地址问题



![20](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105643.png)

使用DHCP Snooping实现防非法DHCP服务器问题（1）

* 在接入交换机上使用DHCP Snooping相关功能，可以实现防止下联用户架设非法DHCP服务器 

* 以下为DHCP Snooping原理动画演示：

  

  ![21](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105646.png)

  

* 在接入交换机上使用DHCP Snooping相关功能，可以实现防止下联用户架设非法DHCP服务器 

* 以下为DHCP Snooping配置步骤：



![22](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220607105648.png)
