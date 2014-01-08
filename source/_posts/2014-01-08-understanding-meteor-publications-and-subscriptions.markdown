---
layout: post
title: "Understanding Meteor Publications & Subscriptions"
date: 2014-01-08 14:43:00 +0800
comments: true
categories: pubsub
---

Meteor 处理应用数据的方式是这个框架最有价值的地方，但同时也是使用它最困难的事情之一，当你在刚开始使用的时候。

因此造成了很多的误解.例如，它的应用是不安全的，也不能处理大数据.

今天，我花点时间来澄清这些误解，介绍Meteor的 `Publications` 和  `` Subscriptions ``


### 以前的方式
***
开始前， 我们回顾下2011年之前Meteor还没有出现，你创建了一个简单的Rails 应用程序.当用户访问你的站点，浏览器发送一个请求给应用程序，其他的操作就全部在服务端了。

应用程序的第一个工作就是找到用户所需的数据。这可能是是12条搜索记录; 或者小明的个人信息;或者是小Q的最新动态等等

你可简单的认为就是书店店员从书架上找到你想要的书。

![Mou icon](/images/posts/understaning_1.jpg)
      想象下这个就是一个大的数据库


一旦拿到你想要的数据，应用程序第二个步骤就是把这些数据转换成我们熟悉的HTML(或者是json数据结构)

以书店作比喻，就像店员把书拿出来，然后包装起来放到一个漂亮的袋子里面，这就是经典的MVC模式中的视图部分。

最后应用程序把html代码发送给到浏览器，然后就悠然自得的等待下一个请求。

### Meteor 的方式
***

Meteor 最关键的创新在于不像Rails应用程序只在服务端运行。Meteor应用还包括客户端组件, 并运行在客户端（用户浏览器）.
 
就像是一个商店职员不但帮你找到了你要的书，还和你回家晚上陪你一起阅读（听起来是不是有点毛骨悚然）。

就是这种架构使Meteor做了很多很酷的东西，其中最主要的就是Meteor所谓的[database everywhere](http://docs.meteor.com/#sevenprinciples),简单来说就是Meteor会拷贝你数据库中的一个子集复制到客户端.

![data set](/images/posts/understaning_2.png)
      Pushing a subset of the database to the client.


其实这有两层重大的含义，首先不是发送HTML code到客户端, Meteor应用程序而是发送实际的原始数据到客户端，让客户端去处理这些数据([data on the wire](http://docs.meteor.com/#sevenprinciples)).

其次，你将马上访问到数据而无需等待服务器的返回。更酷的是你不要担心客户端与服务端的数据同步([latency compensation](http://docs.meteor.com/#sevenprinciples))

### 管理数据
***

我们的应用程序可能包含成千上万的数据， 有些可能是一些私密和敏感的数据。基于安全性和程序扩展性考虑， 我们不可能把整个数据库镜像都复制到客户端去。

所以如何告诉Meteor应该把哪些数据集发送到客户端去呢？

让我们忘记上面书店的比喻，看看下面的图表。

首先下面的数据存储在数据库里。想象下我们正在建立某种形式的论坛，下面的文档就像是用户提交的文章一样。
![database](/images/posts/understaning_3.png)
      All the posts contained in our database.

待续...
