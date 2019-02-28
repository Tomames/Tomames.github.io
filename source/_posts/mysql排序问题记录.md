---
title: mysql排序问题记录
categories: 编程
tags:
  - mysql
  - order by
abbrlink: 7cd11435
translate_title: mysql-sorting-problem-record
date: 2019-02-28 14:31:00
---

### mysql排序问题记录

今天看到一个面试题

`一张表中有id 1-10的10条数据，要根据指定的顺序查询出结果集，比如(5,4,8,7,9,2..)`

才发现原来mysql除了order by \`column\` desc|asc 还有 order by field(\`column\`, value1, value2, value3....)这种用法

```mysql
SELECT * FROM `works` ORDER BY FIELD(`id`, 5,4,8,7,9,2);
```

查询结果为

![](https://wx2.sinaimg.cn/large/8d2ab563gy1g0m4tqegk0j205z07kq2w.jpg)

FIELD()函数是将参数1的字段对后续参数进行比较，并返回1、2、3等等，如果遇到null或者没有在结果集上存在的数据，则返回0，然后根据升序进行排序。

因为其他id没在列出的结果集中，所以返回0，如果没有规定排序顺序的话，默认asc，所以排在前面

```mysql
SELECT * FROM `works` ORDER BY FIELD(`id`, 5,4,8,7,9,2) DESC, id ASC;
```

查询结果为

![](https://ws4.sinaimg.cn/large/8d2ab563ly1g0m50x7p13j205s07kweg.jpg)

1. 如果想让指定id的数据排在前面，又不愿意先对指定顺序倒序处理再查询，我的想法是用**union**

   ```mysql
   (SELECT * FROM `works` WHERE `id` IN (5,4,8,7,9,2) ORDER BY field(`id`, 5,4,8,7,9,2))
   UNION 
   (SELECT * FROM `works` WHERE `id` NOT IN (5,4,8,7,9,2))
   ```

	查询结果

   ![](https://ws4.sinaimg.cn/large/8d2ab563gy1g0m5chbs5zj205s07hweg.jpg)

	这和预想的不一样

2. 查完资料后说要用如下语句

   ```mysql
   SELECT * FROM (SELECT * FROM `works` WHERE `id` IN (5,4,8,7,9,2) ORDER BY field(`id`, 5,4,8,7,9,2)) t1 
   UNION 
   SELECT * FROM (SELECT * FROM `works` WHERE `id` NOT IN (5,4,8,7,9,2)) t2;
   ```

   结果还是一样

   ![](https://ws4.sinaimg.cn/large/8d2ab563gy1g0m5chbs5zj205s07hweg.jpg)

3. 查资料后发现没有**limit**导致order by被优化器干掉了

   变成了

   ```mysql
   (SELECT * FROM `works` WHERE `id` IN (5,4,8,7,9,2))
   UNION 
   (SELECT * FROM `works` WHERE `id` NOT IN (5,4,8,7,9,2))
   ```

   1和2，都被优化成了3  

   -_-||

4. 终极解决办法

   ```mysql
   SELECT * FROM (SELECT * FROM `works` WHERE `id` IN (5,4,8,7,9,2) ORDER BY field(`id`, 5,4,8,7,9,2) LIMIT 100) t1 
   UNION 
   SELECT * FROM (SELECT * FROM `works` WHERE `id` NOT IN (5,4,8,7,9,2) LIMIT 100) t2;
   ```

   查询结果

   ![](https://ws1.sinaimg.cn/large/8d2ab563ly1g0m62wttadj205s07emx4.jpg)



mysql版本**5.7.20-log**

---

UNION在进行表链接后会筛选掉重复的记录，所以在表链接后会对所产生的结果集进行排序运算，删除重复的记录再返回结果。

UNION ALL只是简单的将两个结果合并后就返回。这样，如果返回的两个结果集中有重复的数据，那么返回的结果集就会包含重复的数据了。

从效率上说，UNION ALL 要比UNION快很多

---

--摘抄自 别人的博客

