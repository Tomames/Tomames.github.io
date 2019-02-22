---
title: lnmp环境搭建
categories: 编程
tags:
  - php
  - nginx
  - linux
translate_title: lnmp-environment-construction
abbrlink: 110e9b1f
date: 2019-02-22 14:52:06
---
#### lnmp环境搭建

##### 安装nginx  

1. 添加nginx到yum源 

`rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm`

2. 安装nginx 

`yum -y install nginx`

3. 启动nginx

`systemctl start nginx`

4. 开机启动nginx 

`systemctl enable nginx`

##### 安装mysql

1. 下载yum rpm包

`wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm` 

2. 安装rpm包

`rpm -ivh mysql57-community-release-el7-11.noarch.rpm`

3. 安装mysql-server 

`yum -y install mysql-server`

4. 启动mysql

`systemctl start mysqld`

5. 开机启动mysql 

`systemctl enable mysqld`

6. 查询初始密码

`cat /var/log/mysqld.log|grep 'A temporary password'`

7. 初始密码登录

`mysql -u root -p`

8.修改root用户密码

`set password for root@localhost = password('123456');`

##### 安装php

1. 添加php到yum源

`rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm` 

`rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm`

2. 安装php及扩展

`yum install php71w php71w-fpm php71w-cli php71w-common php71w-devel php71w-gd php71w-pdo php71w-mysql php71w-mbstring php71w-bcmath`

3. 启动php

`systemctl start php-fpm`

4. 开机启动php

`systemctl enable php-fpm` 

