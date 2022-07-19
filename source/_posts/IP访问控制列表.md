---
title: IP访问控制列表
tags:
  - 锐捷
categories:
  - 学习
  - 锐捷
abbrlink: 7212f06f
date: 2022-06-21 15:22:59
---

<div class="danger">

>  资料来源[锐捷](https://talent.ruijie.com.cn/certification/profession/)
>  非原创

</div>





## ACL工作原理

### 前言

访问控制列表(ACL)是一种基于包过滤的访问控制技术，它可以根据设定的条件对接口上的数据包进行控制， 允许其通过或丢弃。  访问控制列表被广泛地应用于路由器和三层交换机，借助于访问控制列表，可以有效地控制用户对网络的 访问，从而保障网络安全。



### 访问控制列表的应用场景

* 在接入交换机、汇聚交换机、核心交换机完成基本VLAN、路由配置，VLAN内/外的PC可以相互通信 
* 如果想要控制个别数据交互，又不影响与其他数据交互，这种情况下就需要使用访问控制列表

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151314.png)





* 出于安全考虑，要求员工的PC不能访问到主管的网段，这该如何实现？



![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151313.png)



### 访问控制列表的功能

* 访问控制列表（Access Control List），简称ACL，可以定义一系列不同的规则
* ACL需要设备接口进行入方向或出方向的调用，根据这些规则对数据包进行分类，并针对不同类型的报文 执行不同的处理动作。 
* ACL主要有以下两大功能： 
  *  使用ACL匹配流量 
     *  用于流量过滤：可以匹配指定流量，拒绝通过或者允许通过 
     *  用于NAT：可以匹配指定流量，对这些指定的流量进行NAT转换 
     *  用于QoS：可以根据数据包的协议，指定这些被匹配的数据包的优先级 
  *  使用ACL匹配路由 
     * 用于路由策略：可以用来匹配路由，进行路由条目的过滤，或者修改路由条目的属性
     * 用于路由重分布：可以匹配路由，在路由重分布时对匹配的路由执行特定的操作

### 访问控制列表的通配符

* 通配符也称“反掩码”， 和IP地址结合使用，以描述一个地址范围 
* 反掩码和子网掩码相似，但含义不同 
  *  0表示：对应位需要比较 
  *  1表示：对应位不比较





IP地址：192.168.0.1

| 通配符          | 含义             | 最终表示的网络地址 |
| --------------- | ---------------- | ------------------ |
| 0.0.0.255       | 只比较前24位     | 192.168.0.0/24     |
| 0.0.3.255       | 只比较前22位     | 192.168.0.0/22     |
| 0.255.255.255   | 只比较前8位      | 192.0.0.0/8        |
| 0.0.0.0         | 每一位都精确比较 | 192.168.0.1/32     |
| 255.255.255.255 | 每一位都不比较   | 0.0.0.0/0          |
| 0.63.255.255    | 只比较前10位     | 192.128.0.1/10     |



### 访问控制列表的ACE



* 每一条语句也称为ACE，访问控制表项(Access Control Entry：ACE) 
* ACE匹配的顺序为从上至下，即编号从低到高进行匹配
* 一旦被某条ACE匹配成功（无论动作是deny或permit）, 跳出该ACL 
  *  在所有的ACE之后，存在一条默认ACE：deny ip any any（不显示）



### 访问控制列表的两种动作

* ACL的动作分为两种：permit和deny 
  * permit：允许/匹配permit后面语句的数据/路由 
  * deny：禁止/不匹配deny后面语句的数据/路由



```
Ruijie(config)#ip access-list standard 1
Ruijie(config-ext-nacl)#10 permit ip 192.168.1.0 0.0.0.255
Ruijie(config-ext-nacl)#20 deny ip 192.168.2.0 0.0.0.255
```

### 访问控制列表的入方向过滤工作流程

![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151319.png)





### 访问控制列表的出方向过滤工作流程

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151323.png)





## ACL基本配置

### 访问控制列表的常用类型

* IP标准ACL 	
  * 只能匹配IP数据包头中的源IP地址 
  * 配置ACL的时候使用”standard”关键字 
* IP扩展ACL 
  * 匹配源IP/目的IP、协议（TCP/IP）、协议信息（端口号、标志代码）等 
  * 配置ACL的时候使用”extended”关键字 
* 除了上面两种常用类型外，还有以下其他的ACL类型



| ACL类型             | 可匹配内容                                                   |
| ------------------- | ------------------------------------------------------------ |
| IP标准ACL           | 源IP                                                         |
| IP扩展ACL           | 源IP，目的IP，ICMP type，ICMP code，TCP、UDP端口号，Fragment，TOS，DSCP，Precedence |
| MAC扩展ACL          | 源MAC，目的MAC，COS，协议字段（包括各种协议）                |
| 专家级ACL           | IP扩展可匹配内容，MAC扩展可匹配内容，VID，inner VID，inner COS |
| 自定义ACL（ACL 80） | 专家级ACL可匹配内容，报文前80字节的任何内容                  |
| IPV6 ACL            | 源IP，目的IP，ICMP type，ICMP code，TCP、UDP端口号，Fragment，DSCP，flow-label |

### 标准ACL与扩展ACL的不同

* 标准ACL和扩展ACL能够匹配的数据流类型不同

  * 标准ACL：仅匹配数据包的源IP地址

  ```
  Ruijie(config)#ip access-list standard 13 
  Ruijie(config-std-nacl)#permit ?
  A.B.C.D Source address
  any Any source host
  host A single source host
  ```

  *  扩展ACL：能够匹配3层及以上多种协议，并且可以同时匹配源IP和目的IP等

  ```
  Ruijie(config)#ip access-list extended 101
  Ruijie(config-ext-nacl)#permit ?
  <0-255> An IP protocol number
  eigrp Enhanced Interior Gateway Routing Protocol
  gre General Routing Encapsulation
  icmp Internet Control Message Protocol
  igmp Internet Group Managment Protocol
  ip Any Internet Protocol
  ipinip IP In IP
  nos NOS
  ospf Open Shortest Path First
  tcp Transmission Control Protocol
  udp User Datagram Protoco
  ```

  ### 访问控制列表的命名

* 数字命名 

  *  默认的命名，需要注意标准和扩展两种类型ACL的数字命名范围是不一样的 
  *  标准ACL常用数字命名为1-99，1300-1999扩展ACL常用数字命名为100-199，2000-2699 

* 自定义名称 

  *  定义更具有代表意义的名称，推荐使用
     * 比如禁止VLAN10内的PC访问VLAN30，可以定义为DENY_VLAN10_TO_VLAN30

```
Ruijie(config)#ip access-list standard ?
<1-99> IP standard acl
<1300-1999> IP standard acl (expanded range)
WORD Acl name
```

### 访问控制列表的配置命令（标准ACL）

* ACL通过带条件的语句来标识数据包

  *  例如禁止PC1（IP:192.168.1.10）和PC2（IP:192.168.2.10）访问任意目的的数据流 
  *  使用下面的配置方法进行配置

  

  ![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151332.png)

  

  * 配置完成后，在相关设备的接口上调用即可

* ACL通过带条件的语句来标识数据包 

  * 例如禁止PC1（IP:192.168.1.10）和PC2（IP:192.168.2.10）访问PC3（IP:192.168.3.10）的数据流 
  * 使用下面的配置方法进行配置

  

  ![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151336.png)

  

  

  *  配置完成后，在相关设备的接口上调用即可。

  

  

  ### 访问控制列表的多条语句配置方法

* 若存在多种不同的访问控制需求，就需要在一个ACL中定义多条语句 

  *  不允许VLAN10内的PC访问192.168.4.0/24内的所有PC 
  *  不允许VLAN10内的PC访问192.168.5.0/24内的所有PC 
  *  仅允许VLAN 10内的特定PC（192.168.1.10）访问 192.168.3.0/24内的所有PC 
  *  允许VLAN 10内PC可以访问10.0.5.5的tcp 80端口，该地址的其他端口均不允许访问 
  *  其他数据流放行

* 配置思路 

  *  确定是采用标准ACL还是扩展ACL
  *  将格式相同（动作、数据流类型、子网号、反掩码等）的语句放置在一起配置 
  *  根据需求配置多条语句时，要确认所配置的顺序能否满足需求（前后是否有冲突）。具体配置如下：


```
Ruijie(config)#ip access-list extended FOR_VLAN10 
Ruijie(config-ext-nacl)#10 permit ip host 192.168.1.10 192.168.3.0 0.0.0.255
Ruijie(config-ext-nacl)#20 deny ip 192.168.1.0 0.0.0.255 192.168.4.0 0.0.0.255
Ruijie(config-ext-nacl)#30 deny ip 192.168.1.0 0.0.0.255 192.168.5.0 0.0.0.255
Ruijie(config-ext-nacl)#40 permit tcp 192.168.1.0 0.0.0.255 host 10.0.5.5 eq 80
Ruijie(config-ext-nacl)#50 deny ip 192.168.1.0 0.0.0.255 host 10.0.5.5 
Ruijie(config-ext-nacl)#60 permit ip any any
```

### 访问控制列表的序号

* 自动生成的序号，默认以10位单位递增。也可以在配置ACL中的语句时提前添加不同的序号。

```
ip access-list extended FOR_VLAN10
10 permit ip host 192.168.1.10 192.168.3.0 0.0.0.255 
20 deny ip 192.168.1.0 0.0.0.255 192.168.4.0 0.0.0.255 
30 deny ip 192.168.1.0 0.0.0.255 192.168.5.0 0.0.0.255 
40 permit tcp 192.168.1.0 0.0.0.255 host 10.0.5.5 eq www 
50 deny ip 192.168.1.0 0.0.0.255 host 10.0.5.5 
60 permit ip any any
```

* 如果新增需求：禁止VLAN 10内的PC访问192.168.6.0/24。配置如下：

```
Ruijie(config)#ip access-list extended FOR_VLAN10
Ruijie(config-ext-nacl)#31 deny ip 192.168.1.0 0.0.0.255 192.168.6.0 0.0.0.255
```

```
ip access-list extended FOR_VLAN10
10 permit ip host 192.168.1.10 192.168.3.0 0.0.0.255 
20 deny ip 192.168.1.0 0.0.0.255 192.168.4.0 0.0.0.255 
30 deny ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255 
31 deny ip 192.168.1.0 0.0.0.255 192.168.6.0 0.0.0.255 
40 permit tcp 192.168.1.0 0.0.0.255 host 10.0.5.5 eq www 
50 deny ip 192.168.1.0 0.0.0.255 host 10.0.5.5 
60 permit ip any any
```

### 访问控制列表的应用位置

1、应用在接口的入方向还是出方向？ 

* 数据流是从交换机的接口出入，需要将配置好的ACL应用在接口上 
* 接口可以是物理接口也可以是SVI 
* 应用的方向根据ACL的内容以及数据流进入接口的方向进行配置选择

```
ip access-list extended FOR_VLAN10
10 permit ip host 192.168.1.10 192.168.3.0 0.0.0.255 
20 deny ip 192.168.1.0 0.0.0.255 192.168.4.0 0.0.0.255 
30 deny ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255 
31 deny ip 192.168.1.0 0.0.0.255 192.168.6.0 0.0.0.255 
40 permit tcp 192.168.1.0 0.0.0.255 host 10.0.5.5 eq www 
50 deny ip 192.168.1.0 0.0.0.255 host 10.0.5.5 
60 permit ip any any
```

![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151344.png)





2、在什么设备上配置ACL？

*  需要结合实际的需求来判断将ACL应用在什么层次的设备上 
   *  控制VLAN内的数据流，则需要在接入交换机上配置ACL 
   *  控制VLAN间的数据流，则需要在汇聚交换机（网关）配置ACL 
*  配置如下的ACL，应用在何处才能够使得PC A无法访问PC B？
   *  B是对的，因为同一个vlan内的数据流不需要经过汇聚交换机

```
ip access-list extended FOR_VLAN10
10 deny ip host 192.168.1.10 host 192.168.1.20 
20 permit ip any any
```



![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151348.png)







2、在什么设备上配置ACL？ 

* 要求VLAN10不能访问VLAN30，VLAN20不能访问VLAN40 
  *  如果将ACL配置在接入交换机上，需要在多台交换机上配置，配置工作量较大，而且容易出错 
  *  因此控制跨网段转发的数据，建议在汇聚网关(SVI接口)上配置ACL，这样可以减少配置量，并方便维护

```
ip access-list extended FOR_VLAN10（为VLAN20配置的ACL略）
10 deny ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255 
20 permit ip any any
```

```
Ruijie(config)#int vlan 10
Ruijie((config-VLAN 10)#ip access-group FOR_VLAN10 in
Ruijie(config)#int vlan 20
Ruijie((config-VLAN 20)#ip access-group FOR_VLAN20 in
```

![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151352.png)

  







3、ACL是在靠近源的设备上应用还是靠近目的的设备上应用？ 

* 需要结合ACL的类型以及实际的应用、配置的工作量进行考虑
  *  标准ACL（匹配源地址），在靠近报文目的的设备上进行配置 
     *  如果应用在靠近数据源的设备上，会有什么问题？





![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151356.png)





3、ACL是在靠近源的设备上应用还是靠近目的的设备上应用？ 

* 需要结合ACL的类型以及实际的应用、配置的工作量进行考虑 
  * 标准ACL（匹配源地址），在靠近报文目的的设备上进行配置 
  * 扩展ACL（匹配目的地址），建议在靠近报文源的设备上进行配置 
    * 避免数据包在网络中经过多个设备转发才在靠近目的的设备上被丢弃掉，会浪费网络资源



![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151359.png)





3、ACL是在靠近源的设备上应用还是靠近目的的设备上应用？

* 需要结合ACL的类型以及实际的应用、配置的工作量进行考虑 
* 标准ACL（匹配源地址），在靠近报文目的的设备上进行配置 
* 扩展ACL（匹配目的地址），建议在靠近报文源的设备上进行配置 
* 对于扩展ACL，如果想集中控制的话，也可以在报文目的设备上进行配置 
  * `若要控制很多源IP网段不能访问PC B，可以在靠近PC B的设备上应用ACL，减少配置工作量，实现集中管理



![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151403.png)



## ACL的其他应用

### 防病毒应用

* 在实际中除了使用ACL控制网段之间的互访外，还有一种比较常见的用法，就是封闭常见的病毒或木马占 用的端口，并在三层网关SVI接口下应用，如下的防病毒ACL配置 

  * 防病毒的ACL也可能会与某些应用的端口号重合，实施时需要注意 
  * 控制互访和防病毒两种应用在实际中也多结合在一起，因此在配置的时候需要注意ACE的先后顺序

  ```
  ip access-list extended antivirus
  10 deny tcp any any eq 1068 
  20 deny tcp any any eq 5554 
  30 deny tcp any any eq 9995 
  40 deny tcp any any eq 9996 
  50 deny tcp any any eq 1022 
  60 deny tcp any any eq 1023 
  70 deny tcp any any eq 445 
  80 deny tcp any any eq 135 
  90 deny tcp any any eq 4444 
  100 deny tcp any any eq 1080 
  110 deny tcp any any eq 3128 
  …(省略部分)
  280 permit ip any any
  ```

### 基于时间的ACL配置思路

* ACL可以添加时间参数，使其在特定的时间生效 
  * 绝对时间段： absolute，定义一个绝对的时间段 
  * 周期时间段： periodic，定义一个循环往复的时间段 
* 确保设备时间配置准确：在#模式下使用clock set命令设置
*  将时间参数和ACE关联 
* 调用ACL





### 基于时间的ACL配置案例

* 例如：公司架设有web服务器，其IP地址为192.168.1.100，要求该服务器在特定的时间内对外提供服务， 详细需求如下： 
  *  仅在工作日的早上9：00至下午18：00提供员工访问，其他时间均不能访问 
  *  仅在2021年 7月28日早上9：00至中午12：00提供员工股访问，其他时间均不能访问



![13](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220621151442.png)



### 配置基于时间的ACL



* 配置方法 
  * 正确配置设备时间
  *  在#模式下使用clock set命令设置 
  *  定义时间段：为ACL中的特定ACE关联定义好的时间段 
    *  绝对时间段： absolute，定义一个绝对的时间段 
    *  周期时间段： periodic，定义一个循环往复的时间段

```
Ruijie(config)#time-range WORK_TIME
Ruijie(config-time-range)#absolute ?
end Ending time and date
start Starting time and date
Ruijie(config-time-range)#absolute start 9:00 28 July 2021 end 
12:00 28 July 2021
```



```
Ruijie(config)#time-range WORK_TIME
Ruijie(config-time-range)#periodic ?
Daily Every day of the week
Friday Friday
Monday Monday
Saturday Saturday
Sunday Sunday
Thursday Thursday
Tuesday Tuesday
Wednesday Wednesday
Weekdays Monday through Friday
Weekend Saturday and Sunday
Ruijie(config-time-range)#periodic weekdays 9:00 to 18:00
```



* 配置方法
  * 正确配置设备时间 
  *  在#模式下使用clock set命令设置 
  * 定义时间段：为ACL中的特定ACE关联定义好的时间段 
  * 当不在WORK_TIME定义的时间范围内，则所配置的两条ACE语句不生效



```
ip access-list extended OA
10 permit tcp any host 192.168.1.100 eq www time-range WORK_TIME 
20 deny tcp any host 192.168.1.100 eq www
30 permit ip any any
```
