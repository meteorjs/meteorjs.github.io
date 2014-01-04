---
layout: post
title: "meteor-frontend-engineers"
date: 2014-01-04 22:23:00 +0800
comments: true
categories: meteor
---


作为我的日常网页开发工作的一部分，我订阅了一些前端的博客。我总是惊讶有那么多优秀的前端工程师对JavaScript和CSS深入了解，以及他们如何能够掌握现代Web浏览器的复杂性。 然而，我也不能不注意到，很多前端开发人员似乎限制了自己，好了，前端。

### 从前端到后端

一方面，这是有意义的。 随着新的JavaScript框架和浏览器快速迭代每天如雨后春笋般冒出来，客户端开发已经提供了足够让你啃一辈子了

然而，事实是，它往往很难建立一个Web网络应用不涉及后端操作，只是用数据库进行通信。

就目前而言，你的应用程序涉及到了后端操作意味着你必须学习一种后端语言如Ruby，Python或PHP。 而新的环境来与它自己的一套模式和规范，你不得不从头开始学。


但事情正在发生变化。 随着Node.js作为Javascript在服务器上的的出现，开发人员可以使用一种统一的语言编写他们的整个应用程序。

但是，今天我想谈谈还不是第三种方法：Meteor ，作为客户端和服务器之间的桥梁并逐渐崭露头角的JavaScript框架。

我大约一年前接触到了Meteor，并迅速喜欢上了它。 我去用它建了一个流行的开放源码应用程序，它使我的应用程序成为了一个非常成功的项目 ，并最终写一本书吧！

Meteor 可能仍然是比较年轻的，但我觉得很多前端工程师和设计师应该会很快爱上Meteor的.如果有机会用一用得话。

所以这里有一个简单的介绍给那些正在生活和呼吸的在浏览器世界以及还在服务器端挣扎的JavaScript 工程师们 !


### Meteor ?

让我们从基础开始：Meteor 是一种基于Node.js的一个JavaScript框架 不像大多数其他的JS框架，Meteor 在服务器和客户端上同时运行，这意味着你可以用一个单一的代码库管理您的整个应用程序，并且这两个部分可以无缝地互相工作。

Meteor 的另一个亮点在于它的reactive和实时性 ，这意味着服务器上的任何数据更改将会立即反映在客户端上，而无需您任何额外的工作。

在实践中，这意味着您可以构建单页面的JavaScript 程序，因为大多数的服务器 - 客户端的通信帮你做好了。


### 你已经知道它的大部分


为什么我认为Meteor是一场伟大的比赛对于前端工程师而言,原因是，典型的JS编译器将已经熟悉的Meteor 应用程序的代码，都是Javascript，即使他们以前从未看过Meteor的官方文档。

当然，Meteor使用JavaScript这种无处不在的语言的事实是一个很大的帮助：不同于使用Rails或者Django，你将不必学习新的编程语言。

更重要的是，Meteor 也包含一些你已经熟悉了（jQuery，underscore.js，Handlebars）的第三方类库，他们甚至都默认包含到了每一个新的Meteor 程序（尽管如此你也可以方便的使用你熟悉的替换这些）。

你不必重新学习这个新的模式，利用现有的Javascript知识， 你就可以很好的使用了。

### 简单设置

其中一个很大的障碍，跨越客户端/服务器的鸿沟是需要建立一个本地开发环境。

毕竟，你真正需要处理的客户端代码是一个网页浏览器，也许Apache的本地实例，如果你真的想要这样。

在另一方面，建立最完整开发境需要安装新的语言，框架，模块和可能的版本管理也是如此。

好消息是，Meteor 捆绑所有的东西在一个单一的命令超级容易安装。 只需键入：

``    curl https://install.meteor.com | /bin/sh ``

你可以打开一个终端窗口，你可以安装Meteor。

### **集合和反应性**


所以，如果你已经熟悉JavaScript和安装是如此简单， 你有什么真正需要学习建立一个Meteor 的应用程序？

对于我来说，这两个关键概念在刚学习Meteor，了解``Collections``和 ``Reactivity``。

Collections 是MongoDB的集合等同与SQL表，即存放类似的数据的一个集合。 Meteor暴露了一个强大的API来发布和订阅这些Collection， 利用这个API可以让你控制哪些数据的子集，可以提供给浏览器。


出于安全性以及性能方面的原因，你可能不希望把你的整个数据库发布到每一个客户端！ 所以，掌握好集合是第一步骤，以建立强大的流星应用程序。

Meteor 另一个关键的概念是Reactivity。 简单地说，任何改变Reactivity的数据源将被立即反映到使用这个数据源的应用程序。

这些数据源不仅包括存储在数据库中的数据，而且包括Session变量或路由过滤器。 更重要的是。你也可以自定义任何变量，使其具有Reactivity

Reactivity是非常强大的，感觉比较神奇。 所以有时候在使用时，可能会产生意想不到的影响。

所以这是存在一个学习曲线的，但掌握了这些概念之后你会很快自如的使用它，开发应用了。


### 一个例子

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

第一个代码示例包含两个Handlebars的模板。 第一个遍历所有的用户通知，包括通知的每个具体项。

然后，JavaScript代码可以简单的返回那个通知列表了。 在这种情况下， return Notifications.find({userId: Meteor.userId(), read: false});仅仅意味着“返回当前用户所有未读的通知”。

当然，这一切都是被动的，这意味着你的通知列表会自动只要底层数据有更新！

如果您想了解更多，Meteor [模块](http://www.discovermeteor.com/2013/02/20/a-look-at-a-meteor-template/)

### 为什么不试试看？

现在，我不想让它听起来像学习Meteor是一个平凡的事业。 这是一个新的框架，引入了许多功能强大的概念，像每一个新的技术，它有一个学习曲线。

但它不是一个巧合是Meteor提供了一个很容易上手的环境。

<p>所以，如果你有一两个小时在你前面，我建议你给它一个尝试。 赶快来试试吧！
