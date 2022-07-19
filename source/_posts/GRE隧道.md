---
title: GRE隧道
tags:
  - eNSP
categories:
  - 学习
  - eNSP
abbrlink: ca260099
date: 2022-06-18 19:41:35
---

# GRE隧道



## 拓扑

![image-20220618193734151](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220618193734.png)

## 实验需求

 2台主机实现跨公网互访

## IP规划方案

PC

| 设备 | IP地址      | 子网掩码      | 网关          |
| ---- | ----------- | ------------- | ------------- |
| PC1  | 192.168.1.1 | 255.255.255.0 | 192.168.1.254 |
| PC2  | 192.168.1.2 | 255.255.255.0 | 192.168.1.254 |
| PC3  | 192.168.2.1 | 255.255.255.0 | 192.168.2.254 |
| PC4  | 192.168.2.2 | 255.255.255.0 | 192.168.2.254 |

设备连接

| 设备名称 | 接口    | IP            | 相连设备 | 接口   | IP        |
| -------- | ------- | ------------- | -------- | ------ | --------- |
| AR1      | g 0/0/0 | 192.168.1.254 | LSW1     | e0/0/3 |           |
| AR1      | g0/0/1  | 100.1.1.1     | AR2      | g0/0/0 | 100.1.1.2 |
| AR2      | g0/0/0  | 100.1.1.2     | AR1      | g0/0/1 | 100.1.1.1 |
| AR2      | g0/0/1  | 200.1.1.2     | AR3      | g0/0/0 | 200.1.1.3 |
| AR3      | g0/0/1  | 200.1.1.3     | LSW2     | e0/0/1 |           |
| AR3      | g0/0/0  | 200.1.1.3     | AR2      | g0/0/1 | 200.1.1.2 |



## 过程

（1）配置pc的IP，子网掩码，网关

PC1

![image-20220618190341213](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220618190341.png)

PC3

![image-20220618190358275](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220618190358.png)



配置路由IP

AR1

```
<Huawei>sys
[Huawei]sys LuRen-R1
[LuRen-R1]interface GigabitEthernet0/0/0
[LuRen-R1-GigabitEthernet0/0/0]ip address 192.168.1.254 255.255.255.0
[LuRen-R1-GigabitEthernet0/0/0]interface GigabitEthernet0/0/1
[LuRen-R1-GigabitEthernet0/0/1]ip address 100.1.1.1 255.255.255.0
```

AR2

```
<Huawei>sys
[Huawei]sys LuRen-R2
[LuRen-R2]interface GigabitEthernet0/0/0
[LuRen-R2-GigabitEthernet0/0/0]ip address 100.1.1.2 255.255.255.0
[LuRen-R2-GigabitEthernet0/0/0]interface GigabitEthernet0/0/1
[LuRen-R2-GigabitEthernet0/0/1]ip address 200.1.1.2 255.255.255.0
```

AR3

```
<Huawei>sys
[Huawei]sys LuRen-R3
[LuRen-R3]interface GigabitEthernet0/0/0
[LuRen-R3-GigabitEthernet0/0/0]ip address 200.1.1.3 255.255.255.0
[LuRen-R3-GigabitEthernet0/0/0]interface GigabitEthernet0/0/1
[LuRen-R3-GigabitEthernet0/0/1]ip address 192.168.2.254 255.255.255.0

```

配置GRE

AR1

```
[LuRen-R1]interface Tunnel0/0/1
[LuRen-R1-Tunnel0/0/1]ip address 1.1.1.1 255.255.255.255   
[LuRen-R1-Tunnel0/0/1]tunnel-protocol gre                 
[LuRen-R1-Tunnel0/0/1]source 100.1.1.1
[LuRen-R1-Tunnel0/0/1]destination 200.1.1.3

```

AR3

```
[LuRen-R3]interface Tunnel0/0/1
[LuRen-R3-Tunnel0/0/1]ip address 2.2.2.2 255.255.255.255
[LuRen-R3-Tunnel0/0/1]tunnel-protocol gre
[LuRen-R3-Tunnel0/0/1]source 200.1.1.3
[LuRen-R3-Tunnel0/0/1]destination 100.1.1.1
```

ospf

AR1

```
[LuRen-R1]ospf 1
[LuRen-R1-ospf-1]area 0.0.0.0
[LuRen-R1-ospf-1-area-0.0.0.0]network 1.1.1.1 0.0.0.0
[LuRen-R1-ospf-1-area-0.0.0.0]network 100.1.1.1 0.0.0.255
```

AR2

```
[LuRen-R2]ospf 1
[LuRen-R2-ospf-1]area 0.0.0.0
[LuRen-R2-ospf-1-area-0.0.0.0]network 100.1.1.0 0.0.0.255
[LuRen-R2-ospf-1-area-0.0.0.0]network 200.1.1.0 0.0.0.255
```



AR3

```
[LuRen-R3]ospf 1
[LuRen-R3-ospf-1]area 0.0.0.0
[LuRen-R3-ospf-1-area-0.0.0.0]network 2.2.2.2 0.0.0.0
[LuRen-R3-ospf-1-area-0.0.0.0]network 200.1.1.0 0.0.0.255
```

静态路由

AR1

```
[LuRen-R1]ip route-static 192.168.2.0 255.255.255.0 Tunnel0/0/1
```



AR3

```
[LuRen-R3]ip route-static 192.168.1.0 255.255.255.0 Tunnel0/0/1
```

检测pc1和pc3的互通性

PC1 ping  PC3

![image-20220618190608962](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220618190609.png)

PC3  ping PC1

![image-20220618190647522](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220618190647.png)



## FAQ

路由端口要划分正确详细，并且要写在拓扑图上。(配置时后容易排查问题)

IP规划要清晰（配置时对照IP规划不易出错）


