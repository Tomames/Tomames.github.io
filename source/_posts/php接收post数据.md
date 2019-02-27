---
title: php接收post数据
categories: 编程
tags: php
abbrlink: 26d08677
translate_title: php-receives-post-data
date: 2019-02-27 15:43:37
---

### php接收post数据

今天开发接口接收合作方post过来的数据

在本地调试的时候是没有问题的，但是线上调试时接收不到客户数据

查了下资料

发现`$_POST`只能接收`Content-Type: application/x-www-form-urlencoded`提交的数据

用户提交的不是`Content-Type: application/x-www-form-urlencoded`标准数据类型的数据

所以要用`file_get_content("php://input")`接收

`GLOBLES['HTTP_RAW_POST_DATA']`在**php7**中已经**废弃**

**注意**

`php://input`不能用于接收**enctype="multipart/form-data"**的数据

