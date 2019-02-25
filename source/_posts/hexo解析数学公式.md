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

按照指示下载[pandoc](https://github-production-release-asset-2e65be.s3.amazonaws.com/571770/3cf14180-24e7-11e9-8ab3-475ce7a9eafc?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAIWNJYAX4CSVEH53A%2F20190225%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20190225T073729Z&X-Amz-Expires=300&X-Amz-Signature=577af8c8450f8945c415402e0701129870b8f4028839c58b30f3cf029512afa8&X-Amz-SignedHeaders=host&actor_id=16967206&response-content-disposition=attachment%3B%20filename%3Dpandoc-2.6-windows-x86_64.msi&response-content-type=application%2Foctet-stream)装上

卸载hexo默认的markd，安装`hexo-renderer-pandoc`

```bash
npm uninstall hexo-renderer-marked --save
npm install hexo-renderer-pandoc --save
```

然后hexo g报错`unknown extension smart`

查完资料发现是pandoc版本问题（艰难）

明明下载的是2.6版本符合要求啊

然后在本地找啊找`Everything`发现之前装的Anaconda3中有了pandoc，版本1.9不符合要求（坑自己啊）

更改完之后执行是不报错了，但是解析出来的和想象中的相去甚远。应该是配置的问题

------------------割------------------