---
title: RGOS日常管理操作
tags:
  - 锐捷
categories:
  - 学习
  - 锐捷
abbrlink: e25b2882
date: 2022-06-08 17:26:34
---



<div class="danger">


>  资料来源[锐捷](https://talent.ruijie.com.cn/certification/profession/)
>  非原创

</div>



## 前言

* 信息化发展的现状要求网络操作系统需要有新技术的植入，锐捷网络推出的新一代通用操作系统RGOS正 是技术创新的充分体现，它是为网络安全运行与管理而设计的完全模块化的支持多种平台的网络操作系统。 
* RGOS操作系统最主要的三大特性是模块化、安全性、开放性。自问世以来，锐捷网络通用操作系统RGOS， 已经在国内多个行业得到广泛应用，其实际应用价值深受广大用户的喜爱和称赞。以用户需求为中心，以 应用价值为导向，锐捷网络通过不断的技术创新，逐步将RGOS完善，新一代通用操作系统RGOS以其特有 稳定性、安全性等技术优势，更有助于提升用户使用价值，为用户创造更大的利益



## RGOS平台登陆方式

### RGOS平台概述

* RGOS全称“锐捷通用操作系统”，即网络设备的操作系统 
  * 基于RGOS开发的软件版本目前为11.x，又被称为11.x平台 
* 优势 
  * 模块化设计，方便运维管理 
  * 故障隔离，提升新功能开发测试效率和系统稳定性 
  * 对于硬件平台透明，兼容性高

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171419.png)





### 锐捷设备的常用登陆方式

* 本地登陆：
  * Console登陆：全新或配置清空的设备，需要使用Console口的方式 
* 远程登陆：
  * Telnet登陆：使用IP网络远程登陆，但传输的数据未加密 
  * SSH登陆：使用IP网络远程登陆，传输数据加密，安全性高，常用SSHv2版本 
  * Web登陆：使用网页登录，操作可视化较便捷 
  * 备注：大部分设备开局需要使用Console登陆，部分设备可直接Telnet/Web登陆
* 常用登陆软件：SecureCRT、Putty、超级终端（仅XP有）

![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171426.png)



### 使用Console登入

* Console概述
  * 通过配置线连接设备的Console接口 
  * 使用终端软件进行设备管理配置 
  * 初始化，带外管理 
* Console管理配置 
  * 波特率：9600 
  * 数据位：8
  * 奇偶校验：无
  * 停止位：1
  * 数据流控：无

![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171431.png)



### Telnet远程管理

* Telnet概述
  * 指通过Windows命令行提示符中的Telnet程序或其他第三方Telnet程序进行设备管理配置 
  * 远程管理，带内管理 
  * 依赖于设备IP地址与Telnet密码 
  * 数据不进行加密
* Telnet配置
  * 协议：Telnet 
  * 认端口：23  设备默认开启Telnet服务

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171435.png)



### SSH远程管理

* SSH概述
  * 使用支持SSH协议程序（SecureCRT）进行设备管理配置 
  * 远程管理，带内管理 
  * 依赖于管理IP地址 
  * 依赖于全局用户名密码
  * 数据加密 
* SSH管理配置
  * 协议：SSH
  * 默认端口：22

```
网络设备上开启SSH服务配置:
Ruijie(config)#enable service ssh-server
Ruijie(config)#crypto key generate {rsa|dsa}
```

![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171441.png)





### 登陆软件：SecureCRT

* 日志管理 
  * 根据锐捷网络技术服务部《JSZD-001技术服务部员工行为奖惩条例》工程师在网操作规范第3条：携带工程 师个人电脑进入客户网络时，需要提前做好电脑的杀毒等工作，以避免由于电脑病毒影响客户网络正常使用， 且开启记录功能（包括Telnet的控制台开启Log日志打印功能）

![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171446.png)



* CRT交互窗口
  * 当日志快速滚动刷屏无法键入时可以使用交互窗口 
  * 在交互窗口键入命令，可以在多个会话窗口执行

![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171451.png)



*  Xmodem使用 
   *  使用Xmodem上传脚本或者下载配置文件。以上传为例：

![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171458.png)

## CLI命令行操作

### CLI命令行基础

* 使用Console、Telnet、SSH登陆设备后，将会出现CLI命令行界面，类似DOS的命令行
* 相对CMD的命令行，RGOS的CLI使用便捷，具备丰富提示信息与错误信息

![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171503.png)





### CLI模式

* 用户模式 
  * 字符光标前是一个“>”符号
  * 有限查看设备信息 
* 特权模式
  * 字符光标前是一个“#”符号 
  * 查看设备所有信息 
* 全局配置模式
  * 字符光标前由“(config)#”组成
  * 配置设备全局参数
* 接口配置模式
  * 字符光标前由“(config-if-xx)#”组成 
  * 配置设备接口参数



![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171509.png)



### CLI模式互换



![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171513.png)



### 命令行特性-分屏显示



* 在命令行界面，显示的内容如果超过一页的范围，则会进行分屏显示
* 在命令行出现“--more--”的提示
  * 使用“回车”键可以逐行显示 
  * 使用“空格”键可以翻页



![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171517.png)





### 命令行特性-命令缩写及获取帮助



![13](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171523.png)



### 命令行特性-错误提示

* 当命令输入错误时，系统将进行提示 
  *  “% Unrecognized host or address, or protocol not running”代表命令无法识别，没有这个协议
  *  “% Incomplete command.”代表输入命令不完整
  *  “% Invalid input detected at ‘^’ marker.”代表所示位置命令有错
  *  还有许多其他提示，建议在配置命令行的时候多关注控制台的消息以及错误提示

![14](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171528.png)







### 命令行特性-历史记录及TAB补全

* 在命令行配置过程中，使用“TAB”键可以对目前的命令进行补全操作
* 熟练应用“TAB”键以及“？”提示帮助，可以有效记忆命令



![15](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171533.png)





## 设备基本操作



### 设备命名



* 设备命名用于标识设备信息 
* 配置规范 
  *  一般参照客户规范 
  *  如果自定义：参考设备的地理位置、网络位置、设备型号、设备编号等因素，制定统一的命名规范 （ AABB-CC-DD ）
     * AA：表示该设备的地理位置
     * BB：表示设备的网络位置
     * CC：表示设备的型号
     * DD： 表示设备的编号

```
配置命令
Ruijie(config)# hostname wlzx-core-8610-1
```



### 配置网络设备的管理IP

配置管理IP后，能方便对设备进行远程管理 

二层交换机通过配置管理VLAN实现，并且交换机可以理解为一台终端，需要配置网关 

多层设备的任意一个三层接口IP可以作为管理IP

```
管理IP配置命令
Ruijie(config)#vlan 10
Ruijie(config)#int vlan 10
Ruijie(config-if-VLAN 10)#ip add 10.1.1.254 255.255.255.0 //管理IP地址
Ruijie(config-if-VLAN 10)#no shutdown
Ruijie(config)#ip default-gateway 10.1.1.200 //当前设备的网关，将被管理设备理解为一台PC终端
```



### 配置网络设备的登陆密码

* 通过配置密码可以设备管理的安全性

* 配置特权模式密码

  ```
  Ruijie(config)#enable secret level 15 0 ruijie
  ```

* 配置Telnet密码

```
Ruijie(config)#line vty 0 4
Ruijie(config-line)#password ruijie
```

* 配置全局用户密码

```
Ruijie(config)#username admin password ruijie
```

* 密文显示密码



```
Ruijie(config)#service password-encryption
```

### 常用show命令

| 命令                    | 解释              |
| ----------------------- | ----------------- |
| Show running-config     | 查看设备当前配置  |
| Show version            | 查看设备版本信息  |
| Show cpu                | 查看设备CPU利用率 |
| Show interface counters | 查看设备接口统计  |
| Show log                | 查看设备日志      |
| Show arp                | 查看设备ARP表     |
| Show mac-address-table  | 查看设备mac表     |
| Show clock              | 查看当前时间      |





### 查看当前时间



* Show命令是操作RGOS时，最常用的命令之一 
* 任意命令行模式均可以使用show命令查看当前设备的配置或状态
* 注意：show run命令查看的是当前设备的配置，不是保存的配置 
  * 配置查看：show run-config



![16](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171540.png)



### 设备状态查看

* 管道符应用概述
  * 在Show命令后面可以加上管道符“|”指定信息的输出
* 管道符类型 
  * | begin xyz ：输出信息从xyz开始
  * | exclude xyz：输出信息排除xyz 
  * | include xyz：输出信息包含xyz



![17](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171545.png)



### 接口描述

* 接口描述用于标识设备接口信息，方便在查看接口状态时识别接口的用途
* 配置规范
  * 根据客户的规范 
  * 自定义：to-对端设备名-对端接口名

```
配置命令
WLZX-core-8610-2(config)#int giga 6/1
WLZX-core-8610-2(config)#description to-wlzx-core-8610-1-giga6/1
```

### Banner配置

* 登陆设备时，输出提示或警告信息 
* 配置规范 
  *  客户规范 
  *  自定义



```
配置命令
Ruijie(config)#banner login ^
Enter TEXT message. End with the character 
'^'.
Warning: Unauthorized access are forbidden!!
Your behavior will be recorded!!

```



![18](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171550.png)





### 时间配置

* NTP协议的作用是让网络设备显示准确的时间，便于监控和维护
* 手工设置通过clock set设置时间



```
Ruijie#clock set hh:mm:ss day month year
```

* 自动设置/同步时间(依赖于NTP/SNTP服务器)

```
Ruijie（config）# {sntp|ntp} enable
Ruijie（config）# {sntp|ntp} server ip_addr
```



### SNMP配置



* Simple Notwork Management，网管软件通过该协议获取设备运行信息、配置设备、故障定位 
  * SNMP有版本v1/v2c/v3，默认使用v2c  v1/v2c 使用团体名进行认证 
  * V3版本的安全性更高



```
SNMP配置：
Ruijie(config)#snmp-server community ruijie {ro|rw}
//ro表示只读属性，网管软件通过该团体名只能获取相关信息
//rw表示可读写属性，网管软件通过该团体名可以执行设备配置操作
```

### 日志应用

* 日志记录了设备运行过程中的一些关键信息，在出现故障时显得尤为重要
* 日志功能默认开启，并将信息记录在内存中，重启后日志将丢失 
* 项目中，建议搭建syslog服务器，记录关键设备（汇聚/核心）日志信息

```
日志服务配置：
Ruijie(config)#service sequence-numbers 
Ruijie(config)#service sysname
Ruijie(config)#logging userinfo command-log
Ruijie(config)#logging server ip_addr
Ruijie(config)#logging source interface loopback 0
Ruijie#terminal monitor 
```

### 网络通讯测试

* Ping用于测试网络的连通性，可使用组合命令进行丰富的网络测试

```
Ruijie#ping 192.168.100.10 source 10.1.1.1 ntime 100 length 1500 timeout 3
//测试从源10.1.1.1到达192.168.100.10的连通性，连续ping100次，每个包长度1500字节，超时时间3秒
```

* Tracertoute用于显示数据包从源地址到目的地址， 所经过的所有网络设备 
  * 用于检查网络的连通性，在网络故障发生时，准确地定位故障发生的位置。

```
Ruijie#traceroute 192.168.100.10 source 10.1.1.1 probe 10 ttl 1 3 timeout 3
//测试从源10.1.1.1到192.168.100.10的连通性，并显示路径上的网络设备，探测数据包每跳最多跟踪10条路
由（例如负载均衡时，会朝多个方向进行探测），TTL值范围是1-3（最多3跳），超时时间3秒
```

## 系统文件管理

### RGOS的文件系统

* 如下图，flash中保存的数据时掉电不丢失的数据：
  * 配置文件：config.text 
  * RGOS系统文件：rgos.bin 
  * 日志文件：syslog.text
  * 其他运行文件
* 当设备开机运行时，会将RGOS以及配置文件全部都加载到内存中运行



![19](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Ruijie/20220608171557.png)





### 设备配置管理

* 设备启动时，从Flash介质中读取config.text文件，并作为当前设备的配置
* Running config是当前正在运行的配置，保存后，将写入config.text



| 命令                                           | 解释                                       |
| ---------------------------------------------- | ------------------------------------------ |
| Ruijie#write                                   | 保存配置                                   |
| Ruijie(config)#no command                      | 删除某条具体命令                           |
| Ruijie#delete config.text                      | 恢复出产配置（锐捷设备不需要单独删除VLAN） |
| Ruijie#copy flash:config.text flash:config.bak | 将当前保存的配置进行备份                   |
| Ruijie#copy flash:config.bak flash:config.text | 将备份配置恢复至当前保存的配置             |
| Ruijie#reload                                  | 重启设备（是否要保存，需要注意）           |
