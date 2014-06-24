---
layout: post
title: "Getstart Meteor Development"
date: 2014-01-08 00:00:04 +0800
comments: true
categories: meteor
---

我之前就对Javascript比较感兴趣，面试我现在这家公司时，说是用Node.js 开发， 我就义无反顾的进来了。
在这边是用的基于Node.js 的一个框架 [Meteor](http://meteor.com), 用了4个多月了, 现在把我用的这些心得写下来, 同时我也会翻译一些国外优秀的Meteor文章 这是第一篇。


## 开始

它支持不同的[平台](https://github.com/meteor/meteor/wiki/Supported-Platforms)

* 安装Meteor

`` $ curl https://install.meteor.com | /bin/sh ``

* 新建应用

`` $ meteor create myapp ``

* 本地运行

{% codeblock %}
{% raw %}
$ cd myapp
$ meteor
  => Meteor server running on: http://localhost:3000/
{% endraw %}
{% endcodeblock %}

* 部署到网络（免费提供服务器)

`` $ meteor deploy myapp.meteor.com ``

