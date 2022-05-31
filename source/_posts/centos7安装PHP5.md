---
title: centos7安装PHP5
tags:
  - CentOS7
categories:
  - 教程
  - Linux
abbrlink: b394315b
date: 2022-04-09 15:31:35
---



参考：[centos7安装PHP5 - xiaoxiaoの - 博客园 ](https://www.cnblogs.com/xxdebug/p/14142266.html)

1. 创建文件夹

```
mkdir /usr/local/src/php 

cd /usr/local/src/php
```

![1](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130813652.png)





2. 安装依赖

```
yum install -y gcc gcc-c++ autoconf libjpeg libjpeg-devel libpng libpng-devel freetype freetype-devel libpng libpng-devel libxml2 libxml2-devel zlib zlib-devel glibc glibc-devel glib2 glib2-devel bzip2 bzip2-devel ncurses curl openssl-devel gdbm-devel db4-devel libXpm-devel libX11-devel gd-devel gmp-devel readline-devel libxslt-devel expat-devel xmlrpc-c xmlrpc-c-devel
```



![2](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130813996.png)



3. 解压下载的php源码包

   

```
tar -xzvf +你的包名.tar.gz
```

![3](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130814770.png)








4. 将解压的文件复制到/usr/local/src/php/下

   ![4](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130814486.png)

5. 进入/usr/local/src/php/php-5.5.25这个文件夹后在终端输入如下命令

```
cd /usr/local/src/php/php-5.5.25
```

![5](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130814634.png)



```
   ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --enable-fpm --enable-sysvsem --enable-sockets --enable-pcntl --enable-mbstring --enable-mysqlnd --enable-opcache --enable-shmop --enable-zip --enable-ftp --enable-gd-native-ttf --enable-wddx --enable-soap
```



![6](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130814784.png)



```
   make
```

![7](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130814067.png)

6. 安装

```
make install

cp /usr/local/php/etc/php-fpm.conf.default php-fpm.conf

cp /usr/local/src/php/php-5.3.28/php.ini-development /usr/local/php/etc/php.ini-development

cp /usr/local/php/etc/php.ini-development /usr/local/php/etc/php.ini

cp /usr/local/src/php/php-5.3.28/sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm
```

![8](https://luren-1310495826.cos.ap-beijing.myqcloud.com/blog/202204130814525.png)



7. 添加权限

```
chmod +x /etc/init.d/php-fpm
```

8. 添加开机启动

```
chkconfig --add php-fpm
```

9. 设置启动等级

```
chkconfig --level 35 php-fpm on

cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf

vim /usr/local/php/etc/php-fpm.conf 

找到 ;pid = run/php-fpm.pid 去掉前面的分号
```

10. 启动

```
service php-fpm start
```

11. 设置环境变量

```
    vim /etc/profile 
    
    底部添加
    PATH=$PATH:/usr/local/php/bin环境变量生效 source /etc/profile
```

12. 查看安装版本

```
php -v
```
