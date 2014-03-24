---
layout: post
title: "Understanding-Meteor-Internals"
date: 2014-03-24 13:47:34 +0800
comments: true
categories: Internals
---

### Meteor Interanls： Meteor是一个服务器也是一个客户端


Meteor程序，看上去就像典型的web程序一样,由浏览器，代理服务器，路由，和其他网络组件构成。事实上Meteor由两个主要的部分构成。 一部分运行在服务端,另一部分运行在客户端。 这两部分相互通信的方式就像现代Web程序一样(例如： Gmail，Trello)。

![internal](/images/posts/internal_1.png)

Meteor 的这种方式， 使得开发人员无需再担心客户端与服务端之间复杂的通信， 只关注自己的业务逻辑。



### Meteor 处理3种不同类型的请求


在Meteor内部，Meteor能够处理下面3种类型的请求:

  * 静态文件
  * DDP 消息
  * HTTP 请求


#### 静态文件
在项目的的``public`` 目录下得所有图片和其他资源文件都当做静态文件处理。当Meteor服务启动的时候，就能自动处理这些文件了。

#### DDP 消息
Meteor 程序客户端与服务端的通信就基于 DDP 协议的。客户端的所有数据订阅，Method 方法调用，Mongo数据库的操作等等这些都是通过DDP消息实现的，DDP是一个轻量级的协议。 通过这个``ddp-analyzer``这个工具，就可以看到DDP消息里面具体怎样操作的。

#### HTTP请求
虽然Meteor的官方文档并没有说明， 但是Meteor能够像其他应用程序一样处理Http请求的.例如： 通过Http请求实现文件上传。具体的你可以通过 [OverflowStack ](http://stackoverflow.com/questions/tagged/meteor)看看怎样手动处理Http请求。


### Meteor 包含两种类型的服务器


虽然Meteor只运行在一个端口，但内部其实是两个独立的服务器工作的。

  * HTTP 服务器
  * DDP 服务器


![internal](/images/posts/internal_2.png)

### HTTP 服务器
HTTP服务器主要用来提供静态文件和HTTP请求，内部是使用Node.js的 ``connect`` 模块

### DDP 服务器
DDP服务器处理所有的数据定义请求，MongoDB操作，Meteor 的Methods. Meteor 使用的是 [SockJs](https://github.com/sockjs/sockjs-node)作为传输协议。实际上，Meteor就是基于SockJs定制的。
后面的文章会详细介绍如何扩展这两种服务器。


### Meteor and MongoDB


  Meteor 是建立在MongoDB之上的，并且完全依赖与它，至少到目前为止是的， 可能在不远的将来会改变。由于MongoDb并不是一个实时的数据库。但Meteor却是实时的。Meteor使用下面的两种技术是之成为了可能。

  1. Polling MongoDB every ~10 secs

  2. Using MongoDB oplog

  轮训的代价是非常大的， 因此Meteor提供另一种配置(Oplog). 但因此需要跟多的配置， 并且在一些MongoDB托管服务中并不可用。后面的章节我会更加详细的讨论这个主题。
