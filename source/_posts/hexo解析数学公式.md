---
title: hexo解析数学公式
abbrlink: 1d69d5ac
translate_title: hexo-analytic-mathematical-formula
date: 2019-02-25 18:08:32
categories: 编程
tags: hexo
---

### hexo解析数学公式

今天写博客要用到数学公式

用Typora写好之后执行`hexo g && hexo s` 之后发现没有被解析成数学公式，原样输出了

遂开始百度

发现大家都说`hexo-renderer-pandoc`好

按照指示下载[pandoc](https://github.com/jgm/pandoc/releases/tag/2.6)装上

卸载hexo默认的markd，安装`hexo-renderer-pandoc`

```bash
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-pandoc --save
```

然后hexo g报错`unknown extension smart`

查完资料发现是pandoc版本问题（艰难）

明明下载的是2.6版本符合要求啊

然后在本地找啊找`Everything`发现之前装的Anaconda3中有了pandoc，版本1.9不符合要求（坑自己啊）

更改完之后执行是不报错了，但是解析出来的和想象中的相去甚远。

仔细阅读了next主题的配置文件

![](https://wx3.sinaimg.cn/large/8d2ab563gy1g0jk91043qj20mj031mx8.jpg)

原来是没有加载js文件

在`.md`文件头部加入`mathjax: true`然后重新生成就ok了
