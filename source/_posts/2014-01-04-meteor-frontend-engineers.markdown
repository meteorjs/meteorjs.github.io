---
layout: post
title: "meteor-frontend-engineers"
date: 2014-01-04 22:23:00 +0800
comments: true
categories: meteor
---

原文 <http://davidwalsh.name/meteor-frontend-engineers>


在我日常网页开发工作的同时，我也订阅了一些前端的博客。我总是感叹为啥前端工程师对JavaScript和CSS这些技术有这么深得造诣，同时他们也能够掌握这么复杂的浏览器。
然而，我也注意到很多前端开发人员似乎设定了自己，仅仅是个前端开发而已。

### **Front to Back**

从某方面看。 随着越来越多的JavaScript框架的出现以及现代浏览器的快速发展，客户端开发的技术已经足够让你啃一辈子了.

然而事实上，却很难建立一个只与数据库打交道而不涉及后端操作的Web应用。

就目前而言，涉及到了后端操作意味着你必须学习一种新的后端语言如Ruby，Python或PHP。而新的语言都有它自己的一套模式和规范，你不得不从头开始学习.

随着Node.js作为Javascript在服务器上的的出现，这情况正在发生变化。 开发人员可以使用一种的语言编写他们的整个应用程序。

于此同时，像[Parse](https://parse.com/) 和 [Firebase](https://www.firebase.com/) 这种后端服务提供商的出现，使得仅仅添加几行Javascript代码就可以应用具有持久化功能成为可能。

但是今天我说得是第三种方法：[Meteor](http://meteor.com)  一个在客户端和服务器之间建立纽带并开始崭露头角的JavaScript框架。

我大约一年前接触到了Meteor，并迅速喜欢上了它。 我去用它建了一个流行的开放源码应用程序，并成为了一个非常成功的项目 ，并最终促使我写了[一本书](http://www.discovermeteor.com/)！

Meteor 现在虽然比较年轻，如果有机会试试的话, 我认为会有很多前端工程师和设计师会很快爱上它的，

所以这篇文章送给那些正生活在浏览器世界以及还在服务器端使用JavaScript 工程师们 !


### **Meteor ?**

让我们从基础开始：Meteor 是一种基于Node.js的一个JavaScript框架 不像大多数其他的JS框架，Meteor是在服务器和客户端同时运行，这意味着你可以用一个统一的代码库管理您的整个应用程序，并且这两个部分可以无缝地工作。

Meteor 的另一个亮点在于它的 `reactive` 和 `real-time` ，这意味着服务器上的任何数据更改将会立即反映在客户端上，而无需您任何额外的工作。

在实践中，这意味着您可以构建单页面的JavaScript应用，因为大多数的服务器-客户端的通信工作帮你做好了。


### **你已经知道它的大部分**

为什么我认为Meteor是一场强大对于前端工程师而言, 因为，现有成熟的JS编译器都已经熟悉的Meteor 应用程序的代码，都是Javascript，即使他们以前从未看过Meteor的官方文档。

当然，Meteor使用JavaScript这种无处不在的语言的事实是一个很大的帮助：不同于使用Rails或者Django，你将不必学习新的编程语言。
更重要的是，Meteor 也包含一些你已经熟悉了（jQuery，underscore.js，Handlebars）的第三方类库，他们甚至都默认包含到了每一个新建的Meteor 应用（即使这样你也可以方便的替换这些使用你熟悉的）。

你不必重新学习这个新的模式，利用现有的Javascript知识， 你就可以很好的开始了。

### **简单设置**

不像其他的开发环境， 需要安装新的语言， 模块， 框架，以及自身的版本控制。
Meteor 已经把这些东西都做好了，只需要下面这个命令可以了。只要输入：

``    curl https://install.meteor.com | /bin/sh ``

你可以打开一个终端窗口，你可以安装Meteor。

### **Collections 和 Reactivity**

所以，如果你对JavaScript比较熟悉了以及安装也是如此简单， 那有什么真正需要学习的来建立一个Meteor 的应用程序？

对于我来说，刚学习Meteor时两个重要的概念，了解``Collections``和 ``Reactivity``。

Collections 是MongoDB的集合等同于SQL里面的表，即存放相同数据的一个集合。 Meteor暴露了一个强大的API来发布和订阅这些Collection， 利用这个API可以让你控制哪些数据的子集，可以提供给客户端。

出于安全性以及性能方面的原因，你可能不希望把你的整个数据库数据发布到每一个客户端！ 所以，建立强大的Meteor应用程序的第一步是掌握好Collection。

Meteor 另一个关键的概念是Reactivity。 简单地说，任何改变Reactivity的数据源将被立即反映到使用这个数据源的地方。

这些数据源不仅包括存储在数据库中的数据，而且包括Session变量或路由过滤器等等（后面文章会介绍）。 更重要的是, 你也可以自定义任何变量，使其具有Reactivity.

Reactivity是非常强大的也十分神奇。 所以有时候在使用时，可能会产生意想不到的影响。

所以这是存在一个学习曲线的，但掌握了这些概念之后你会很快自如的使用它来开发应用了。


### **一个例子**

现在你大概知道Meteor是怎么一回事，让我们一起来看看Meteor一个简单的示例代码。
下面是2个 [Microscope](https://github.com/DiscoverMeteor/Microscope)的模板(列出用户通知）的代码。
注意：如果你想知道， [Microscope](https://github.com/DiscoverMeteor/Microscope) 是我们开发的新闻应用程序。

{% codeblock %}
{% raw %}
<template name="notifications">
  <ul class="notification">
    {{#if notificationCount}}
      {{#each notifications}}
        {{> notification}}
      {{/each}}
    {{else}}
      <li><span>No Notifications</span></li>
    {{/if}}
  </ul>
</template>

<template name="notification">
  <li>
    <a href="{{postPagePath postId}}">
     <strong>{{commenterName}}</strong> commented on your post
    </a>
  </li>
</template>
{% endraw %}
{% endcodeblock %}

而这里是这些模块对应的Javascript代码：

{% codeblock %}
{% raw %}
  Template.notifications.helpers({
    notifications: function() {
     return Notifications.find({userId: Meteor.userId(), read: false});
    },
    notificationCount: function(){
     return Notifications.find({userId: Meteor.userId(), read: false}).count();
    }
  });

  Template.notification.events({
    'click a': function() {
      Notifications.update(this._id, {$set: {read: true}});
    }
  })

{% endraw %}
{% endcodeblock %}


我敢打赌，你已经知道这段代码的含义了
第一个代码示例包含两个[Handlebars](http://handlebarsjs.com/)的模板。 第一个遍历所有的用户通知，包括通知的每个具体项。
然后，JavaScript代码可以简单的返回那个通知列表了。 在这种情况下， `return Notifications.find({userId: Meteor.userId(), read: false});`仅仅意味着“返回当前用户所有未读的通知”。

当然，这一切都是被动的，这意味着你的通知列表会自动更新只要底层数据有更新！

如果您想了解更多，Meteor [模块](http://www.discovermeteor.com/2013/02/20/a-look-at-a-meteor-template/)

### **为什么不试试看？**

现在，我不想让它听起来像学习Meteor是一个平凡的事业。 这是一个新的框架，引入了许多功能强大的概念，像每一个新的技术，它有一个学习曲线。
但Meteor提供了一个很容易上手的环境。
所以，如果现在有2，3个小时的时间，我建议你赶快来试试吧！
