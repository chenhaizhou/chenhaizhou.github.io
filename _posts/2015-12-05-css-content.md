---
title: CSS3的content属性详解
layout: post
tags:
  - Web
  - F2e
  - Css
  - Css3
---


CSS中主要的伪元素有四个：before/after/first-letter/first-line，在before/after伪元素选择器中，有一个content属性，能够实现页面中的内容插入。

<!--more-->

## 插入纯文字

content:"插入的文章"，或者content:none不插入内容

html:

{% highlight html %}
<h1>这是h1</h1>
<h2>这是h2</h2>
{% endhighlight %}

css

{% highlight css %}
h1::after{
    content:"h1后插入内容"
}
h2::after{
    content:none
}
{% endhighlight %}

预览：[https://jsfiddle.net/dwqs/Lmm1r08x/](https://jsfiddle.net/dwqs/Lmm1r08x/)

<iframe width="100%" height="300" src="//jsfiddle.net/dwqs/Lmm1r08x/embedded" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 嵌入文字符号

可以使用content属性的open-quote属性值和close-quote属性值在字符串两边添加诸如括号、单引号、双引号之类的嵌套文字符号。open-quote用于添加开始的文字符号，close-quote用于添加结束的文字符号。

修改上述的css:

{% highlight css %}
h1{
    quotes:"(" ")";  /*利用元素的quotes属性指定文字符号*/
}
h1::before{
    content:open-quote;
}
h1::after{
    content:close-quote;
}
h2{
    quotes:"\"" "\"";  /*添加双引号要转义*/
}
h2::before{
    content:open-quote;
}
h2::after{
    content:close-quote;
}
{% endhighlight %}

预览：[https://jsfiddle.net/dwqs/p8e3qvv4/](https://jsfiddle.net/dwqs/p8e3qvv4/)

<iframe width="100%" height="300" src="//jsfiddle.net/dwqs/p8e3qvv4/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 插入图片

content属性也可以直接在元素前/后插入图片

html:
{% highlight html %}
<h3>这是h3</h3>
{% endhighlight %}

css:
{% highlight css %}
h3::after{
    content:url(http://ido321.qiniudn.com/wp-content/themes/yusi1.0/img/new.gif)
}
{% endhighlight %}

预览：[https://jsfiddle.net/dwqs/c6qk6pkv/](https://jsfiddle.net/dwqs/c6qk6pkv/)

<iframe width="100%" height="300" src="//jsfiddle.net/dwqs/c6qk6pkv/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 插入元素的属性值

content属性可以直接利用attr获取元素的属性，将其插入到对应位置。

html:
{% highlight html %}
<a href="http:///www.ido321.com">这是链接&nbsp;&nbsp;</a>
{% endhighlight %}

css:
{% highlight css %}
a:after{
    content:attr(href);
}
{% endhighlight %}

预览：[https://jsfiddle.net/dwqs/m220nzan/](https://jsfiddle.net/dwqs/m220nzan/)

<iframe width="100%" height="300" src="//jsfiddle.net/dwqs/m220nzan/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 插入项目编号

利用content的counter属性针对多个项目追加连续编号.

html:
{% highlight html %}
<h1>大标题</h1>
<p>文字</p>
<h1>大标题</h1>
<p>文字</p>
<h1>大标题</h1>
<p>文字</p>
<h1>大标题</h1>
<p>文字</p>
{% endhighlight %}

css:
{% highlight css %}
h1:before{
    content:counter(my)'.';
}
h1{
    counter-increment:my;
}
{% endhighlight %}

预览：[https://jsfiddle.net/dwqs/2ueLg3uj/](https://jsfiddle.net/dwqs/2ueLg3uj/)

<iframe width="100%" height="300" src="//jsfiddle.net/dwqs/2ueLg3uj/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 项目编号修饰

默认插入的项目编号是数字型的，1,2,3.。。。自动递增，也能给项目编号追加文字和样式,依旧利用上面的html，css修改如下：

{% highlight css %}
h1:before{
    content:'第'counter(my)'章';
    color:red;
    font-size:42px;
}
h1{
    counter-increment:my;
}
{% endhighlight %}

预览：[https://jsfiddle.net/dwqs/17hqznca/](https://jsfiddle.net/dwqs/17hqznca/)

<iframe width="100%" height="300" src="//jsfiddle.net/dwqs/17hqznca/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 指定编号种类

利用content(计数器名，编号种类)格式的语法指定编号种类，编号种类的参考可以依据ul的list-style-type属性值。利用上述的html，css修改如下：

{% highlight css %}
h1:before{
    content:counter(my,upper-alpha);
    color:red;
    font-size:42px;
}
h1{
    counter-increment:my;
}
{% endhighlight %}

预览：[https://jsfiddle.net/dwqs/4nsrtxup/](https://jsfiddle.net/dwqs/4nsrtxup/)
<iframe width="100%" height="300" src="//jsfiddle.net/dwqs/4nsrtxup/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

## 编号嵌套

大编号中嵌套中编号，中编号中嵌套小编号。

html:
{% highlight html %}
<h1>大标题</h1>
<p>文字1</p>
<p>文字2</p>
<p>文字3</p>
<h1>大标题</h1>
<p>文字1</p>
<p>文字2</p>
<p>文字3</p>
<h1>大标题</h1>
<p>文字1</p>
<p>文字2</p>
<p>文字3</p>
{% endhighlight %}

css:
{% highlight css %}
h1::before{
    content:counter(h)'.';
}
h1{
    counter-increment:h;
}
p::before{
    content:counter(p)'.';
    margin-left:40px;
}
p{
    counter-increment:p;
}
{% endhighlight %}

预览：[https://jsfiddle.net/dwqs/2k5qbz51/](https://jsfiddle.net/dwqs/2k5qbz51/)

<iframe width="100%" height="300" src="//jsfiddle.net/dwqs/2k5qbz51/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

在示例的输出中可以发现，p的编号是连续的。如果对于每一个h1后的三个p重新编号的话，可以使用counter-reset属性重置，修改上述h1的css:

{% highlight css %}
h1{
    counter-increment:h;
    counter-reset:p;
}
{% endhighlight %}

这样，编号就重置了。

预览：[https://jsfiddle.net/dwqs/hfutu4Lq/](https://jsfiddle.net/dwqs/hfutu4Lq/)

<iframe width="100%" height="300" src="//jsfiddle.net/dwqs/hfutu4Lq/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

还可以实现更复杂的嵌套，例如三层嵌套。

html:
{% highlight html %}
<h1>大标题</h1>
<h2>中标题</h2>
<h3>小标题</h3>
<h3>小标题</h3>
<h2>中标题</h2>
<h3>小标题</h3>
<h3>小标题</h3>
<h1>大标题</h1>
<h2>中标题</h2>
<h3>小标题</h3>
<h3>小标题</h3>
<h2>中标题</h2>
<h3>小标题</h3>
<h3>小标题</h3>
{% endhighlight %}

css:
{% highlight css %}
h1::before{
    content:counter(h1)'.';
}
h1{
    counter-increment:h1;
    counter-reset:h2;
}
h2::before{
    content:counter(h1) '-' counter(h2);
}
h2{
    counter-increment:h2;
    counter-reset:h3;
    margin-left:40px;
}
h3::before{
    content:counter(h1) '-' counter(h2) '-' counter(h3);
}
h3{
    counter-increment:h3;
    margin-left:80px;
}
{% endhighlight %}

预览：[https://jsfiddle.net/dwqs/wuuckquy/](https://jsfiddle.net/dwqs/wuuckquy/)

<iframe width="100%" height="300" src="//jsfiddle.net/dwqs/wuuckquy/embedded/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

张大大有一篇利用counter实现计数的文章：小tip:CSS计数器+伪类实现数值动态计算与呈现

>原文：[http://www.ido321.com/1555.html](http://www.ido321.com/1555.html)