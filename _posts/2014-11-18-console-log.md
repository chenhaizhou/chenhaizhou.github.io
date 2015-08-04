---
layout: post
title:  "美化console.log的文本"
date:   2014-11-18 17:48:25
tags:
  - Js
  - F2e
---

前段时间看到天猫的控制台信息

<!--more-->

使用非常简单有意思：

{% highlight js %}
console.log("%c css88.com", "font-size:20pt")
{% endhighlight %}

以%c开头，后面的文字就打印的信息，后面一个参数就是样式属性；
可以尝试多个样式，每碰到一个%c开头就会应用对应的样式：

{% highlight js %}
    console.log("%c 前端开发 %c css88.com %c 愚人码头", "color:red","","color:orange;font-weight:bold")
{% endhighlight %}

在样式中你可以通过css3属性写彩色字，文字描边等等：

{% highlight js %}
	console.log("%c有阴影的log", "text-shadow: 3px 1px 1px grey")
     
console.log('%c彩色文字啊 ', 'background-image:-webkit-gradient( linear, left top, right top, color-stop(0, #f22), color-stop(0.15, #f2f), color-stop(0.3, #22f), color-stop(0.45, #2ff), color-stop(0.6, #2f2),color-stop(0.75, #2f2), color-stop(0.9, #ff2), color-stop(1, #f22) );color:transparent;-webkit-background-clip: text;font-size:5em;');
{% endhighlight %}

甚至加背景图片：

{% highlight js %}
    console.log("%c ", "background: url(http://css88.b0.upaiyun.com/css88/2014/03/jquery.png) no-repeat center;padding-left:120px;padding-bottom: 200px;")
{% endhighlight %}

抽空可以看一下 api：[http://getfirebug.com/wiki/index.php/Console.log](http://getfirebug.com/wiki/index.php/Console.log)



