---
title: php执行shell
categories: 编程
tags:
  - php
  - shell
  - linux
abbrlink: '45109102'
date: 2019-06-04 13:53:10
---

#### php执行shell

##### 需求

最近收到一个奇葩的需求：后台需要添加子站

![](https://ws4.sinaimg.cn/mw690/8d2ab563ly1g3p35bbkvdj208c08cdfw.jpg)

主站是`www.abc.com`，后台需要添加`huaji.abc.com`,`gaoxiao.abc.com`等子站，内容基本一致，就是修改下网站的`title,logo`等信息

##### 方案

php调用shell脚本，复制一个站点文件夹，并修改相应的网站配置和nginx配置，并重新加载nginx配置，域名手动解析

##### 难点

linux权限问题

##### 实现

服务器上新建一个shell脚本

`cp.sh`

```shell
#!/bin/sh

cp /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/huaji.conf 
sed -i 's/www.abc.com/huaji.abc.com/g' /etc/nginx/conf.d/huaji.conf 
sed -i 's/web\/html\/www/web\/html\/huaji/g' /etc/nginx/conf.d/huaji.conf

cp -r /web/html/www /web/html/huaji 
sed -i 's/www/huaji/g' /web/html/huaji/index.php

/usr/bin/sudo /usr/sbin/nginx -s reload
```

php中调用该脚本

```php
$shell = "bash /web/html/www/cp.sh";
exec($shell, $result, $status);
if(!$status){
    echo 'success';
}else{
    echo 'failed';
}
```

php-fpm的执行用户是apache

1. 需要给apache用户相应文件夹的读写权限

2. 增加apache的nginx脚本管理权限

   ```shell
   #1.设置sudo配置文件可写权限
   chmod u+w /etc/sudoers
   #2.增加apache用户的nginx脚本管理权限
   apache	ALL=(root)	NOPASSWD: /usr/sbin/nginx
   #3.关闭【强制控制台登录】执行
   #Defaults	requiretty
   #4.还原配置文件权限
   chmod u-w /etc/sudoers
   ```

##### 备注

网站安全性有待提升