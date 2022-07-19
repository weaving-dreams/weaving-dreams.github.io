---
title: Windows常用网络命令
tags:
  - 锐捷
categories:
  - 学习
  - 锐捷
abbrlink: 13c75e65
date: 2022-06-08 17:25:32
---

<div class="danger">


>  资料来源[锐捷](https://talent.ruijie.com.cn/certification/profession/)
>  非原创

</div>

## 前言



## Ping命令应用解析

### ICMP协议

* ICMP协议全称Internet控制报文协议（Internet Control Message Protocol），是网络层的一个重要协 议
* ICMP协议用来在网络设备间传递各种差错和控制信息
* 对于收集各种网络信息、诊断和排除各种网络故障具有至关重要的作用
* ICMP协议有两个重要应用，在Windows系统与RGOS中经常会使用到

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155123.png)

### ICMP信息反馈

* 根据ICMP的反馈信息，可以分析出到达目的地址连通性的信息 
* 根据这些信息反馈，可以初步分析网络故障的原因

| 反馈信息                     | 信息解释                                          |
| ---------------------------- | ------------------------------------------------- |
| ！                           | 收到1个ICMP的request replay                       |
| U                            | 收到1个ICMP不可达信息                             |
| .                            | 表示在规定的时间内没有收到对应的request replay    |
| Request Timed Out            | 超时，目标主机不存在 或ICMP应答功能关闭，不予应答 |
| Destination Host Unreachable | 网络设备没有到目标主机的路由                      |
| Unknown host                 | 未知主机 ，有可能是DNS的问题                      |



### Ping命令应用解析

* Ping命令应用解析
  * 作用：基于ICMP协议，测试网络联通性
  * 默认情况下，ping操作会收到4个ICMP回包，包大小为32字节，TTL等于64。
  * 操作演示

![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155128.png)



* Ping命令扩展（-t）
  * 作用：长ping数据包不中断 
  * 说明：键入Ctrl+C可中断ICMP回包 
  * 操作演示：

![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155135.png)

* Ping命令扩展（-n）
  * 作用：指定单次ping包的发送数量 
  * 操作演示：

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155145.png)

* Ping命令扩展（-l）
  * 作用：修改Ping包大小，默认为32字节。可用于测试接口MTU值设置是否合理等场景
  * 操作演示：

![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155149.png)



* Ping命令扩展（参数组合）
  * 作用： ping命令后加相应参数，可自定义连通性测试的效果

略

* Ping命令扩展（ping域名）
  * 作用：测试与特定网络资源的连通性时，若不知其具体IP地址，可直接ping其域名，此时DNS可自动解析为 对应IP地址
  * 操作演示：

![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155154.png)



## Tracert命令应用解析

### Tracert命令应用解析



* Tracert命令

  * 作用：基于ICMP协议，在探测网络连通性的同时，探测到目标主机所经过的路径，显示沿路径设备的接口IP 
  * 原理：
    * 源主机首先发三个包，TTL=1 
    * 第一台路由器收到后，TTL-1=0，丢弃并回复源主机ICMP报超时消息 
    * 收到超时消息，源主机显示信息并发送下一组三个包，TTL=上一组TTL+1 
    * 收到目标主机的应答后程序终止
    * *号表示探测包丢失

  

![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155203.png)



* Tracert命令

  * 说明：运营商核心骨干网及大型IDC数据中心为了安全起见一般会通过相关技术做到核心路径隐藏，此时 tracert并非实际路径跟踪
  * 操作演示



![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155211.png)

* Tracert命令（域名跟踪） 
* 作用：不得知目标具体IP地址的情况下，可tracert其域名，此时DNS将会解析出其IP地址。 
* 操作演示



![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155207.png)





## Ipconfig命令应用解析

### IPconfig命令应用解析

* ipconfig命令
  * 作用：显示所有适配器（网卡）的基本IP配置 
  * 操作演示：

![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155220.png)





* Ipconfig /all命令 
  * 作用：显示所有适配器（网卡）详细完整的IP配置
  * 操作演示：



![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155224.png)



## 其他Windows命令解析

### 清除DNS缓存命令解析

* pconfig /flushdns命令
  * 作用：查看Windows系统arp表项。
  * 说明：可使用ipconfig /flushdns清除DNS缓存表
  * 操作演示：

![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155229.png)







### arp -a命令应用解析

* arp –a 命令 
  * 作用：查看Windows系统arp表项。
  * 说明：可使用arp –d命令清除arp缓存表 
  * 操作演示：



![13](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155234.png)

### Route print命令应用解析

* Route print 命令
  * 作用：查看Windows系统路由表
  * 操作演示：



![14](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155238.png)



### 添加静态路由



* route –p add 【目标IP地址/网段】 mask 【子网掩码】【下一跳】

  *  作用：Windows添加静态路由 
  *  操作演示

  ![15](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155243.png)

  

  * route print查看

  ![16](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155246.png)

  

  * Route delete删除

  

  ![17](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608155252.png)

  

  * 说明：增添/删除静态路由操作需要在Windows超级管理员权限下进行
