---
layout: post
title: "理解Javascript函数调用和函数内部This引用"
date: 2014-06-24 14:51:07 +0800
comments: true
categories: Javascript Function
---

一直以来， 我对Javascript函数调用都很困惑， 不仅仅是函数调用， 很多人都抱怨函数内部的`this`引用也混乱。

在我看来， 只要是明白了函数的调用的原始本质，就会明白很多， 其实他们方式的函数调用只不过是在其原始调用的基础上增加一些语法糖而已。其实这也是ECMAScript所要描述的那样， 在这篇文章的重点就是讲这些。


** 函数调用本质 **


首先，我们看看函数调用的本质，就是一个函数调用 `call` 方法， 调用 `call` 方法是相对于直接通过括号`()`调用

1. 把第二个参数到最后一个参数当做一个参数列表(argList)。

2. 第一个参数就是函数内部This的值(thisValue).

3. 调用函数并且把thisValue设置到函数内部this， argList当做函数的参数列表。

例如：
{% codeblock %}
{% raw %}

    function hello(thing) {
      console.log(this + " says hello " + thing);
    }

    hello.call("Yehuda", "world") //=> Yehuda says hello world
{% endraw %}
{% endcodeblock %}


在console运行代码，你就会发现this值被设置成了"Yehuda", 参数thing设置成立了"world", 这就是javascript函数原始的调用， 然后你再看看其他的调用方式， 只是在这个的基础上加上语法糖而已。[the ES5 spec]('http://es5.github.com/#x15.3.4.4')


** 简单函数调用 **


很显然，每次调用函数都通过`call` 来调用会很烦， javascript允许我们直接使用`()`语法来调用函数, 当我们这样调用时， 实际上就像应用了下面的方式。

{% codeblock %}
{% raw %}
    function hello(thing) {
        console.log("Hello " + thing);
    }

    // this:
    hello("world")

    // desugars to:
    hello.call(window, "world");
{% endraw %}
{% endcodeblock %}


当上面的代码在ECMAscript5 的严格模式下， 行为会不一样。

{% codeblock %}
{% raw %}

    // this:
    hello("world")

    // desugars to:
    hello.call(undefined, "world");
{% endraw %}
{% endcodeblock %}


上面的版本可以这样表示: 一个函数调用 `fn(...args)` 就像 `fn.call(window [ES5-strict: undefined], ...args)`

注意函数表达式直接调用也是应用同样的道理`(function() {})() ` 和`(function() {}).call(window [ES5-strict: undefined)`是一样的。



** 对象方法函数 **


另一种比较常用的函数调用是作为对象方法的方式。`(person.hello())`, 像下面这种方式:
{% codeblock %}
{% raw %}

    var person = {
      name: "Brendan Eich",
      hello: function(thing) {
        console.log(this + " says hello " + thing);
      }
    }

    // this:
    person.hello("world")

    // desugars to this:
    person.hello.call(person, "world");
{% endraw %}
{% endcodeblock %}


在这个例子中hello是作为person对象的一个方法， 而上一个示例中hello是作为独立的方法。 如果在程序执行过程， 动态指定方法到一个对象里，情况是怎样的呢？
{% codeblock %}
{% raw %}


    function hello(thing) {
      console.log(this + " says hello " + thing);
    }

    person = { name: "Brendan Eich" }
    person.hello = hello;

    person.hello("world") // still desugars to person.hello.call(person, "world")

    hello("world") // "[object DOMWindow]world"
{% endraw %}
{% endcodeblock %}


注意, 函数里面的this的值并不是总固定的，它的值总是依赖于当时函数调用者。


** 使用函数原型的 bind 方法 **


因为有时我们想函数里面`this`能保持不变。因此，人们在很久以前就使用闭包的方式来保持函数里面`this`的值不变。
{% codeblock %}
{% raw %}

    var person = {
      name: "Brendan Eich",
      hello: function(thing) {
        console.log(this.name + " says hello " + thing);
      }
    }

    var boundHello = function(thing) { return person.hello.call(person, thing); }

    boundHello("world");
{% endraw %}
{% endcodeblock %}


尽管`boundHello`的调用方式转换为 `boundHello.call(window, "world")`, 但是在方法里面，我们通过闭包引用person，直接调用指定this值， 最终还是达到了我们的效果。

我们还可以优化下上面的代码， 使的更通用。
{% codeblock %}
{% raw %}

    var bind = function(func, thisValue) {
      return function() {
        return func.apply(thisValue, arguments);
      }
    }

    var boundHello = bind(person.hello, person);
    boundHello("world") // "Brendan Eich says hello world"
{% endraw %}
{% endcodeblock %}


为了明白上面的代码， 你需要了解两点： 首先`arguments`是一个类似数组的对象，但并不是一个真正的数组， 代表代码函数执行时，传递过来的参数列表。 其次是`apply` 方法其实它和`call`方法十分的相似，唯一的区别就是，它只接收一个类数组对象的参数并不像`call`那样一个个参数传递。

这里`bind` 方法只是简单的返回一个函数。当我们返回的函数被调用时，原始的函数被调用，并且原始的thisValue设置成this值。 arguments 当作参数列表。

由于这个方式普遍应用， ES5实现了这种行为, 并在Function原型上引进了这个`bind`方法
{% codeblock %}
{% raw %}

    var boundHello = person.hello.bind(person);
    boundHello("world") // "Brendan Eich says hello world"
{% endraw %}
{% endcodeblock %}


当你需要传递一个函数作为回调函数时，就特别有用。
{% codeblock %}
{% raw %}

    var person = {
        name: "Alex Russell",
          hello: function() { console.log(this.name + " says hello world"); }
    }

   $("#some-div").click(person.hello.bind(person));

    // when the div is clicked, "Alex Russell says hello world" is printed
{% endraw %}
{% endcodeblock %}


[英文原文]('http://yehudakatz.com/2011/08/11/understanding-javascript-function-invocation-and-this/')
