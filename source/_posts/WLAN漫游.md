---
title: WLAN漫游
tags:
  - eNSP
categories:
  - 学习
  - eNSP
  - WLAN和安全
abbrlink: ba6ef991
date: 2022-06-15 15:06:21
---

## 拓扑

![image-20220615145902818](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220615145903.png)



## 配置如下：

### 路由器的配置：

```
sys
sysname LuRen-SW
int g0/0/0
ip add 10.1.1.2 30
int lo0
ip add 1.1.1.1 32
ospf 1
area 0.0.0.0
network 1.1.1.1 0.0.0.0
network 10.1.1.0 0.0.0.3

```





### 交换机配置

```
undo terminal monitor 
sys
LuRen-SW
vlan batch 10 100
int g0/0/1
port link-type trunk
port trunk allow-pass vlan 10 100
int g0/0/2
port link-type trunk
port trunk pvid vlan 10
port trunk allow-pass vlan 10 100
int g0/0/3
port link-type trunk
port trunk pvid vlan 10
port trunk allow-pass vlan 10 100

```



### AC的配置

```
sys
sysname LuRen-AC
vlan batch 12 10 100
int g0/0/1
port link-type access
port default vlan 12
int g0/0/2
port link-type trunk
port trunk allow-pass vlan 10 100
int vlan 10
ip add 172.16.1.1 24
dhcp enable
int vlan 10
dhcp select interface
int vlan 12
ip add 10.1.1.1 30
int vlan 100
ip add 172.16.100.1 24
dhcp select interface
quit
capwap source interface vlan 10
wlan
ap whitelist mac 00e0-fcc7-7640
ap whitelist mac 00e0-fc53-2e60
security-profile name LuRen	
security wpa-wpa2 psk pass-phrase 12345678 aes




quit
ssid-profile name LuRen-ssid
ssid LuRen
quit
regulatory-domain-profile name LuRen-domain
country-code cn
quit
vap-profile name LuRen-vap
forward-mode tunnel
service-vlan vlan-id 100
ssid-profile LuRen-ssid

security-profile LuRen


quit
rrm-profile name LuRen-rrm
smart-roam enable
smart-roam roam-threshold check-snr check-rate 
smart-roam roam-threshold snr 30
smart-roam roam-threshold rate 30
quit
radio-2g-profile name LuRen-2g



rrm-profile LuRen-rrm
quit
radio-5g-profile name LuRen-5g
rrm-profile LuRen-rrm
quit
ap-group name ap
radio 0
radio-2g-profile LuRen-2g
vap-profile LuRen-vap wlan 1
quit
radio 1	
radio-5g-profile LuRen-5g


vap-profile LuRen-vap wlan 1
quit
quit
ap-id 0
ap-group ap
ap-id 1
ap-group ap

```

### 验证

验证AP上线

```
[LuRen-AC]dis ap all
Info: This operation may take a few seconds. Please wait for a moment.done.
Total AP information:
nor  : normal          [2]
-------------------------------------------------------------------------------------------------
ID   MAC            Name           Group IP           Type            State STA 
Uptime
-------------------------------------------------------------------------------------------------
0    00e0-fc04-7ed0 00e0-fc04-7ed0 ap 172.16.1.133 AP3030DN nor   0 5M:45S
1    00e0-fc47-3a80 00e0-fc47-3a80 ap 172.16.1.40 AP3030DN  nor   0 5M:36S
-------------------------------------------------------------------------------------------------
Total: 2

```





## web

### 路由器的配置：

```
sysname LuRen-R0
int g0/0/0
ip add 10.1.1.2 30
int lo0
ip add 1.1.1.1 32
ospf 1
area 0.0.0.0
network 1.1.1.1 0.0.0.0
network 10.1.1.0 0.0.0.3

```



### 交换机配置

```
undo terminal monitor 
sys
sys LuRen-SW
vlan batch 10 100
int g0/0/1
port link-type trunk
port trunk allow-pass vlan 10 100
int g0/0/2
port link-type trunk
port trunk pvid vlan 10
port trunk allow-pass vlan 10 100
int g0/0/3
port link-type trunk
port trunk pvid vlan 10
port trunk allow-pass vlan 10 100

```



### AC的配置

```
sys
sysname LuRen-AC
int vlan 1
ip add 192.168.200.100 24
quit
http server enable

```

### Web登录

```
略
```
