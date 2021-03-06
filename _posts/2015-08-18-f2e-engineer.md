---
layout: post
title: '[译]如何成为一个优秀的前端工程师'
tags:
 - Web
 - F2e
---

来自Google的前端工程师-Philip Walton 分享了自己关于如何成为优秀的工程师的一些观点。个人感觉很有价值，所以翻译成中文，方便大家阅读。水平有限，如翻译不妥之处请在评论中指出。

原文地址：[http://philipwalton.com/articles/how-to-become-a-great-front-end-engineer](http://philipwalton.com/articles/how-to-become-a-great-front-end-engineer)
<!--more-->


最近，我收到了读者的邮件，引发了我的一些思考。这是他在邮件中问我的问题：

> Hi Philip, is it okay to ask how you become a great front-end engineer? Any advice?

我不得不承认，我感到非常的惊讶，居然会被问到这样的问题，因为我从来没有想过自己是一个优秀的前端工程师。实际上，我在这个行业工作的前几年里，我真的不觉得我能够胜任我的工作。我接受了这些工作只是因为我以前没有意识到我知道得东西太少了，我能够得到这些工作只是因为以前面试我的人不知道问我什么问题。

话虽这么说，我最后还是把我自己的角色做得非常好，而且成为了团队里有价值的一员。当我最后离职的时候（下一个职位我还是无法胜任）我通常也会应试那些将要应聘我的职位的人。现在回想那些我面试过的应聘者，让我明白，看待知识的重要性。尽管我一开始在这个领域很薄弱。我现在的自己可能也不会雇佣以前的那个自己，尽管我知道随着工作经验的积累，成功也是可能的。

在Web领域，我工作的越久，越让我意识到不错的人和真正优秀的人的区别在于不是他们知道什么，而是他们如何思考。 很明显，知识是很重要的--特别是在某些情况下--但是，在一个快速变化的领域并非如此。你获取知识的方式比你知道那些知识更为重要。而且也许最重要的是：你如何利用你的知识去解决日常生活中的问题。

你可以找到大量谈论知识点、框架和工具的文章，这是知识都是获得一份工作所需要知道的。我想说一些不一样的。在这篇文章中，我会说一说前端工程师应有的心态，希望能够回答最开始的问题：怎样才能做到优秀？

## 不要只是解决问题，找到问题的根源所在

很多的人只是不断地写一些 CSS 和 Javascript 补丁直到他们发现这些东西可以正常的工作了，然后他们就不管了。我在代码审查的过程中看到了很多这样的做法。

我会经常问别人：“为什么你要在这里加一个 float: left ？” 或者 “这个 overflow:hidden 真的需要吗？”，然后他们会回答：“我不知道，但是如果我删掉他们，就出问题了”。

Javascript 也一样。我看到一些人用 setTimeout 来防止执行顺序上的问题，或者是一些人滥用 stopPropagation() 而没有考虑到它也许会影响页面中的其他事件处理函数。

我遇到过很多相同或类似的问题，如果你从来不花时间去了解问题的根源所在，你会发现你会一遍一遍的遭遇同样的困境。

花时间去深入的研究你的解决方案为什么可行看起来需要耗费很多的精力，但是我发誓它会在将来给你节约很多时间。对你现在工作的系统有一个全面的理解可以减少你将来的猜测和检查工作。

## 学会预测浏览器领域将来的变化

前端跟后端主要的不同是：后端的运行环境在你的控制之下。但是对于前端而言，相比于后端，它完全不在你的控制范围。你的用户所使用的平台或者设备随时都有可能改变，你的代码需要能够优雅地处理这种情况。

我记得早在2011年的时候，我在一个非常著名的 javascript 框架看到下面这样一段源码（大致如此）：

{% highlight js %}
var isIE6 = !isIE7 && !isIE8 && !isIE9;
{% endhighlight %}

我知道在现实生活中， feature detaction 并不能100%的有效，有些时候我们不得不采用这种不好的方式或者浏览器白名单的方式，但是任何时候你这样做的时候，你应该预测未来可能会发生什么，即使发生，你的代码也不应该出现bug。

对于我们大多数人而言，我们在现在的工作岗位上写的代码会比我们的任期更长。我8年前写的一些代码现在还会经常用到，这想起来既让人满足又让让感到可怕。

## 阅读官方文档

浏览器 bug 总是存在的，但是，当两个浏览器执行相同的代码表现却不一致时，人们经常假设，而不是亲自去检查，只是把他们认为的表现好的叫做“正常”浏览器，表现异常的叫“不正常”浏览器。但并非总是这样，当你做出了错误的假设时，任何你选择的解决方法在未来也许会失效。

一个现实的例子就是 flex items 的默认最小尺寸。根据官方文档， flex items 的 min-width 和 min-height 是 auto (而不是 0)，这意味着，它们的尺寸不会比它们的内容还要小。 但是在过去的8个月里，只有 Firefox 是唯一一个正确的实现这一标准的浏览器。

如果你遇到过这类跨浏览器的兼容性问题，而且你注意到你的页面在 Chrome，IE，Opera，和 Safari 上表现一致，唯独 Firefox 上表现不同时，你肯定会猜测这是 Firefox 自己的问题。

当两个或多个浏览器在渲染相同的代码表现不一致时， 你应该花一些时间去研究到底哪个浏览器才是正确的，然后用正确的方式写下你的代码。你的作品才会是面向未来的。

另外，优秀的前端工程师经常都是站在变化的最前列的，他们会在这些技术成为主流之前就采用这些技术，甚至为这些技术作出贡献。如果你凭借你自己的实力去查找官方文档，而且能够想象一个技术在你能够在浏览器中用它的之前将会是如何工作的，你将成为能够谈论这个官方标准会对开发造成什么影响的人。

## 阅读其他人的代码

阅读其他人的代码，无疑是成为一个更好的开发者的最好方式。

自己解决问题是学习的最好方式，但是如果这些问题都是你以前解决过的，你很快就会进入平稳期（很难有上升的空间）。阅读其他人的代码可以为你打开处理问题的新的思路。而且阅读和理解别人写的代码的能力也是在跟团队合作或者参与开源项目时至关重要的能力。

实际上，我认为在面试一个应聘者是只让他们写代码--新的代码，是最大的错误。我应聘的时候，从来没有被叫过去阅读一些已经存在的代码，在这些代码中找出问题，然后解决它。这是非常不好的，因为作为一个工程师，很多时候我们是在别人的代码上添加和改变一些代码。很少从头写一些新的。

## 跟比你聪明的人一起工作

在我印象你，比起后端开发者，更多的前端开发者希望成为一个自由职业者（全栈）。也许是前端工程师更趋向于自学，而后端工程师更趋向于学术。

自学并为你自己工作的问题是无法从比你聪明的人身上得到好处。没有人来跟你讨论观点或者帮你审查代码。

我强烈的建议，至少在你职业生涯初期，在一个团队中工作，特别是跟一群比你更聪明更有经验的人工作。

如果你已经结束你的职业生涯，现在只是为你自己工作，那么参与到开源中来。 贡献开源项目会给你很多与团队合作的机会。

## 重复造轮子

在商业上，重复造轮子是不好的，但是对于学习来说并非如此。你也许尝试从 npm 上获取预输入控件或者事件委托库，但是想象一下如果你自己尝试创造这些东西的话会学到更多。

我确定一些正在阅读这篇文章的人对此感到强烈反对。不要误会我的意思。我不是说你永远也不应该使用第三方库。使用优秀的库是非常明智的事情。

但是，在这篇文章中，我要说的是如何从一个不错的工程师成为一个优秀的工程师。大部分我认为的这个领域中优秀的工程师都是这些优秀的第三方库的维护者。

你也许从来没有构建够自己的 javascript 库，但是你依然能够在你的职业生涯中获得成功，但是，可能你从来没有理解到解决问题的核心。

在这个行业中，人们经常问起的一个问题是：我接下来应该做什么？ 如果你问了这个问题，为什么不去尝试重新创造一个你喜欢的 javascript 库或者 CSS 框架，而不是尝试一些新的工具或者写一个新的 app。 这样做的好处是，即使你遇到了困难，你也可以从目前已有的库中的源码找到答案。

## 把你学到的东西写下来

最后， 你应该把你学到的东西写下来。有太多的理由这样做了，但是，也许最重要的原因是这样可以强迫你更好地理解你所学的东西。如果你无法解释其原理，这是一个很好的机会说明你并没有完全搞懂它。很多时候你没有意识到你不懂，直到你把它写下来。

在我的经验中，书写、做一个演讲、以及写一些 demos 是强迫我自己完全弄懂一个东西的最好方式，从里到外。即使没有一个人会看你写的东西，但是做这件事的过程更有价值。

> 本文转载自 [http://blog.mcbird.cn/2015/08/15/How-to-Become-a-Great-Front-End-Engineer](http://blog.mcbird.cn/2015/08/15/How-to-Become-a-Great-Front-End-Engineer)