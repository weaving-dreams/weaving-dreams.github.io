---
title: AC双热机备份
tags:
  - eNSP
categories:
  - 学习
  - eNSP
  - WLAN和安全
abbrlink: 50b45d5b
date: 2022-06-15 15:20:40
---

## 拓扑

![image-20220615151949929](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220615151950.png)



## LSW

交换机的配置

```
sys
sysname LuRen-SW
vlan batch 10 to 14 801
in g0/0/3
port link-type trunk
port trunk pvid vlan 10
port trunk allow-pass vlan 10 to 14
in g0/0/4
port link-type trunk
port trunk pvid vlan 10
port trunk allow-pass vlan 10 to 14
in g0/0/1
port link-type trunk
port trunk allow-pass vlan 10 to 14 801
in g0/0/2
port link-type trunk
port trunk allow-pass vlan 10 to 14 801
in vlan 801
ip address 10.1.201.1 24 
```

配置各业务的网关

```
in vlan 10
ip address 10.1.10.1 24
in vlan 11
ip address 10.1.11.1 24
in vlan 12
ip address 10.1.12.1 24
in vlan 13
ip address 10.1.13.1 24
in vlan 14
ip address 10.1.14.1 24
int LoopBack 0
ip add 101.101.101.101 32 //模拟公网
```









## AC1

AC1的基础配置

```
sys
sys LuRen-AC1
vlan batch 10 to 14 801
in g0/0/1
port link-type trunk
port trunk allow-pass vlan 10 to 14 801
in vlan 10
ip add 10.1.10.100 24
in vlan 11
ip add 10.1.11.100 24
in vlan 12
ip add 10.1.12.100 24
in vlan 13
ip add 10.1.13.100 24
in vlan 14
ip add 10.1.14.100 24
in vlan 801
ip add 10.1.201.100 24


```

```
dis ip int brief
```

```
ip route-static 0.0.0.0 0.0.0.0 10.1.201.1
ping 101.101.101.101
```

.创建AP组

## ac1

```
wlan
ap-group name ap-g1
```

## ac2

```
sys
sys LuRen-AC2
wlan
ap-group name ap-g1
```













## AC1

配置AP上线

开启DHCP服务

```
dhcp enable
ip pool ap
network 10.1.10.0 mask 24
gateway-list 10.1.10.1
option 43 sub-option 3 ascii 10.1.201.100
in vlan 10
dhcp select global
quit
ip pool sta1
network 10.1.11.0 mask 24
gateway-list 10.1.11.1
ip pool sta2
gateway-list 10.1.12.1
network 10.1.12.0 mask 24
ip pool sta3
network 10.1.13.0 mask 24
gateway-list 10.1.13.1
ip pool sta4
network 10.1.14.0 mask 24
gateway-list 10.1.14.1
in vlan 11
dhcp select global
in vlan 12
dhcp select global
in vlan 13
dhcp select global
in vlan 14
dhcp select global
quit
```

配置业务vlan pool：vlan分配算法为hash

```
vlan pool sta-p1
vlan 11 12
assignment hash
vlan pool sta-p2
vlan 13 14
assignment hash
quit
```



配置域管理模板

 

```
wlan
regulatory-domain-profile name dom
country-code cn
quit
quit
capwap source interface Vlanif 801 

```

配置AP认证：MAC认证

```
wlan
ap auth-mode mac-auth
ap-mac 00e0-fc72-3d60 ap-id 0
ap-group ap-g1


ap-name ap1
ap-mac 00e0-fc97-2a40 ap-id 1
ap-group ap-g1


ap-name ap2
dis ap all


quit
quit
```

AC1上配置WLAN业务

创建安全模板，配置安全策略



```
wlan
security-profile name yw1
security open
security-profile name yw2
security wpa2 psk pass-phrase 12345678 aes

```

创建SSID模板



```
quit
ssid-profile name yw1
ssid yw1
ssid-profile name yw2
ssid yw2

```

创建vap模板，并引用安全和SSID模板



```
quit
vap-profile name yw1
forward-mode tunnel
service-vlan vlan-pool sta-p1
security-profile yw1
ssid-profile yw1
vap-profile name yw2
forward-mode direct-forward
service-vlan vlan-pool sta-p2
security-profile yw2
ssid-profile yw2

```

AP组引用域管理模板和vap模板

```
quit
ap-group name ap-g1
regulatory-domain-profile dom



vap-profile yw1 wlan 1 radio all
vap-profile yw2 wlan 2 radio all

```

查看vap状态

```
dis vap ssid yw1
dis vap ssid yw2


quit
quit
```















## AC2

.配置备用LuRen-AC2的基础

```
vlan batch 10 to 14 801
in g0/0/1
port link-type trunk
port trunk allow-pass vlan 10 to 14 801
in vlan 10
ip add 10.1.10.200 24
in vlan 11
ip add 10.1.11.200 24
in vlan 12
ip add 10.1.12.200 24
in vlan 13
ip add 10.1.13.200 24
in vlan 14
ip add 10.1.14.200 24
in vlan 801
ip add 10.1.201.200 24
dis ip int brief


quit
ip route-static 0.0.0.0 0.0.0.0 10.1.201.1
```

创建AP组

```
wlan
ap-group name ap-g1
quit
quit 
```



开启DHCP服务

```
dhcp enable
ip pool ap
network 10.1.10.0 mask 24
gateway-list 10.1.10.1
option 43 sub-option 3 ascii 10.1.201.100
in vlan 10
dhcp select global
quit
ip pool sta1
network 10.1.11.0 mask 24
gateway-list 10.1.11.1
ip pool sta2
network 10.1.12.0 mask 24
gateway-list 10.1.12.1
ip pool sta3
network 10.1.13.0 mask 24
gateway-list 10.1.13.1
ip pool sta4
network 10.1.14.0 mask 24
gateway-list 10.1.14.1
quit
```

使vlanif接口能DHCP功能

```
in vlan 11
dhcp select global
in vlan 12
dhcp select global
in vlan 13
dhcp select global
in vlan 14
dhcp select global
quit
```

配置vlan pool，用于业务vlan

```
vlan pool sta-p1
vlan 11 12
assignment hash
vlan pool sta-p2
vlan 13 14
assignment hash

```

配置LuRen-AC2域管理模板

```
quit
wlan
regulatory-domain-profile name dom
country-code cn

```

配置LuRen-AC2的源接口

```
quit
quit
capwap source interface Vlanif 801
```

配置LuRen-AC2的AP认证

```
wlan
ap auth-mode mac-auth
ap-mac 00e0-fc72-3d60 ap-id 0
ap-group ap-g1

ap-name ap1
ap-mac 00e0-fc97-2a40  ap-id 1
ap-name ap2
ap-group ap-g1

```

10.LuRen-AC2上配置WLAN业务参数

创建安全模板，配置安全策略

```
quit
quit
wlan
security-profile name yw1
security open
security-profile name yw2
security wpa2 psk pass-phrase 12345678 aes

```

创建ssid模板

```
quit
ssid-profile name yw1
ssid yw1
ssid-profile name yw2
ssid yw2

```

创建VAP模板，转发模式为直接转发，引用安全和ssid模板

```
quit
vap-profile name yw1
forward-mode tunnel
service-vlan vlan-pool sta-p1
security-profile yw1
ssid-profile yw1
vap-profile name yw2
forward-mode direct-forward
service-vlan vlan-pool sta-p2
security-profile yw2
ssid-profile yw2

```



AP组引用管理模板和VAP模板

```
quit
ap-group name ap-g1
regulatory-domain-profile dom


vap-profile yw1 wlan 1 radio all
vap-profile yw2 wlan 2 radio all

```

















## AC1

11.在主LuRen-AC1上配置VRRP实现双机热备份

创建管理vrrp备份组，优先级为120，抢占时间为120秒

```
int Vlanif 801
vrrp vrid 1 virtual-ip 10.1.201.3
vrrp vrid 1 priority 120
vrrp vrid 1 preempt-mode timer delay 120
admin-vrrp vrid 1
quit
```

创建业务vrrp备份组

```
int Vlanif 10
vrrp vrid 2 virtual-ip 10.1.10.3
vrrp vrid 2 preempt-mode timer delay 120
vrrp vrid 2 track admin-vrrp interface Vlanif 801 vrid 1 unflowdown
int Vlanif 11
vrrp vrid 3 virtual-ip 10.1.11.3
vrrp vrid 3 preempt-mode timer delay 120
vrrp vrid 3 track admin-vrrp interface Vlanif 801 vrid 1 unflowdown
int Vlanif 12
vrrp vrid 4 virtual-ip 10.1.12.3
vrrp vrid 4 preempt-mode timer delay 120
vrrp vrid 4 track admin-vrrp interface Vlanif 801 vrid 1 unflowdown
int Vlanif 13
vrrp vrid 5 virtual-ip 10.1.13.3
vrrp vrid 5 preempt-mode timer delay 120
vrrp vrid 5 track admin-vrrp interface Vlanif 801 vrid 1 unflowdown
int Vlanif 14
vrrp vrid 6 virtual-ip 10.1.14.3
vrrp vrid 6 preempt-mode timer delay 120
vrrp vrid 6 track admin-vrrp interface Vlanif 801 vrid 1 unflowdown

```

配置VRRP备份组的状态恢复延迟时间为30秒

```
vrrp recover-delay 30
```

创建HSB主备服务0：配置主备通道IP地址和端口号，报文重传次数和发送间隔



```
hsb-service 0
service-ip-port local-ip 10.1.201.100 peer-ip 10.1.201.200 local-data-port 10241 peer-data-port 10241
service-keep-alive detect retransmit 2 interval 1

```

创建HSB备份组0，邦迪HSB主备服务0和管理vrrp备份组

```
hsb-group 0

bind-service 0

track vrrp vrid 1 interface Vlanif 801
```







配置NAC业务绑定HSB备份组

```
quit
hsb-service-type access-user hsb-group 0
```



配置wlan业务绑定HSB备份组

```
hsb-service-type ap hsb-group 0
```

使能双机热备功能

```
hsb-group 0
hsb enable
quit
```

更改AC1源接口

```
undo capwap source interface Vlanif 801
capwap source ip-address 10.1.201.3
```

配置dhcp服务器的option 43字段

```
dhcp server database enable
dhcp server database recover
ip pool ap
option 43 sub-option 3 ascii 10.1.201.3
```









## AC2

12.备用LuRen-AC2的配置

创建管理vrrp备份组

```
int Vlanif 801
vrrp vrid 1 virtual-ip 10.1.201.3
admin-vrrp vrid 1

```

创建业务vlan备份组

```
int Vlanif 10
vrrp vrid 2 virtual-ip 10.1.10.3
vrrp vrid 2 track admin-vrrp interface Vlanif 801 vrid 1 unflowdown
int Vlanif 11
vrrp vrid 3 virtual-ip 10.1.11.3
vrrp vrid 3 track admin-vrrp interface Vlanif 801 vrid 1 unflowdown
int Vlanif 12
vrrp vrid 4 virtual-ip 10.1.12.3
vrrp vrid 4 track admin-vrrp interface Vlanif 801 vrid 1 unflowdown
int Vlanif 13
vrrp vrid 5 virtual-ip 10.1.13.3
vrrp vrid 5 track admin-vrrp interface Vlanif 801 vrid 1 unflowdown
int Vlanif 14
vrrp vrid 6 virtual-ip 10.1.14.3
vrrp vrid 6 track admin-vrrp interface Vlanif 801 vrid 1 unflowdown
quit
```

配置备份组状态恢复延迟为30秒

```
vrrp recover-delay 30
```

创建HSB主备服务0

```
hsb-service 0
service-ip-port local-ip 10.1.201.200 peer-ip 10.1.201.100 local-data-port 10241 peer-data-port 10241
service-keep-alive detect retransmit 2 interval 1
quit
```

创建HSB备份服务组0，绑定HSB主备服务0和管理vrrp备份组

```
hsb-group 0
bind-service 0
track vrrp vrid 1 interface Vlanif 801

```

 

配置NAC业务绑定HSB备份组

```
hsb-service-type access-user hsb-group 0
```



配置WLAN业务绑定HSB备份组

```
hsb-service-type ap hsb-group 0
```



配置dhcp业务绑定备份组

```
hsb-service-type dhcp hsb-group 0
```



使能双机热备功能

```
hsb-group 0
hsb enable
quit
```

更改AC2的源接口

```
undo capwap source interface Vlanif 801

capwap source ip-address 10.1.201.3

```

修改DHCP服务器的option 43字段

```
dhcp server database enable
dhcp server database recover
ip pool ap
option 43 sub-option 3 ascii 10.1.201.3

```
