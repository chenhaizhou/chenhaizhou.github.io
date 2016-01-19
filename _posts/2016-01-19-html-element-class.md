---
title: 'HTML5元素分类与内容模型'
layout: post
tags:
 - Html
 - Html5
 - Web
 - F2e
---

HTML4中，元素被分成两大类: inline(内联元素)与block(块级元素)。但在实际的开发过程中，因为页面表现的需要，前端工程师经常把inline元素的display值设定为block(比如a标签)，也经常把block元素的display值设定为inline；之后更是出现了inline-block这一对外呈现inline、对内呈现block的属性。因此，简单地把HTML元素划分为inline与block已经不再符合实际需求。
<!--more-->

基于这种考虑，在HTML5中，标准制定者重新定义了HTML元素的分类，并根据这一新的分类定义了元素的内容模型(Content Model) — 对于一个元素而言，哪些子元素是合法的，而哪些子元素是非法的。

##元素分类

HTML5中，元素主要分为7类：

 - Metadata
 - Flow
 - Sectioning
 - Heading
 - Phrasing
 - Embedded
 - Interactive

这些分类集合互相之间也存在一定的交集(一个元素可以同时属于多个分类)，其交集关系呈现为：

![html5 nesting](/images/html5-nesting.png)

需要注意的是，HTML5中的这种元素分类与inline、block没有任何关系，任何元素都可以在CSS中被定义为display:inline或者display:block。另外，除了这7大分类，还存在一些较小的分类，如Script-Supporting元素等。

###Metadata（元数据元素）

顾名思义，Metadata元素意指那些定义文档元数据信息的元素 — 其作用包括：影响文档中其它节点的展现与行为、定义文档与其它外部资源之间的关系等。以下元素属于Metadata：

{% highlight html %}
base, link, meta, noscript, script, style, template, title
{% endhighlight %}

###Flow（流式元素）

所有可以放在body标签内，构成文档内容的元素均属于Flow元素。因此，除了base, link, meta, style, title等只能放在head标签内的元素外，剩下的所有元素均属于Flow元素。

{% highlight html %}
a， abbr， address， area（如果它是map元素的后裔）， article， aside， audio， b， bdi， bdo， blockquote， br， button， canvas， cite， code， command， datalist， del， details， dfn， div， dl，em， embed， fieldset， figure， footer， form， h1， h2， h3， h4， h5， h6， header， hgroup， hr， i， iframe， img， input， ins， kbd， keygen， label， map， mark， math， menu， meter，nav， noscript， object， ol， output， p， pre， progress， q， ruby， s， samp， script， section， select， small， span， strong， style（如果该元素设置了scoped属性）， sub， sup， svg， table，textarea， time， u， ul， var， video， wbr， text
{% endhighlight %}

###Sectioning（章节元素）

Sectioning意指定义页面结构的元素，具体包含以下四个：

{% highlight html %}
article, aside, nav, section
{% endhighlight %}

###Heading（标题元素）

所有标题元素属于Heading，也即以下6个元素：

{% highlight html %}
h1, h2, h3, h4, h5, h6
{% endhighlight %}

###Phrasing（段落元素）

所有可以放在p标签内，构成段落内容的元素均属于Phrasing元素。因此，所有Phrasing元素均属于Flow元素。在HTML5标准文档中，关于Phrasing元素的原始定义为：

> Phrasing content is the text of the document, as well as elements that mark up that text at the intra-paragraph level. Runs of phrasing content form paragraphs. 

对于这一定义，个人认为不应当使用“text”这一容易引起误解的词，事实上，一个元素即使不是文本，只要能包含在p标签中成为段落内容的一部分，就可以称之为Phrasing元素。

{% highlight html %}
a（如果其只包含段落式元素）， abbr， area（如果它是map元素的后裔）， audio， b， bdi， bdo， br， button， canvas， cite， code， command， datalist， del（如果其只包含段落式元素）， dfn， em， embed， i，iframe， img， input， ins（如果其只包含段落式元素）， kbd， keygen， label， map（如果其只包含段落式元素）， mark， math， meter， noscript， object， output， progress， q， ruby， s， samp， script，select， small， span， strong， sub， sup， svg， textarea， time， u， var， video， wbr， text
{% endhighlight %}

一个不太精确的类比是：HTML5中的Phrasing元素大致就是HTML4中所定义的inline元素。

Phrasing元素内部一般只能包含别的Phrasing元素。

关于Phrasing元素，Stackoverflow上有一个比较精彩的问答，可供参考：
[http://stackoverflow.com/questions/30233447/what-is-the-difference-between-phrasing-content-and-flow-content](http://stackoverflow.com/questions/30233447/what-is-the-difference-between-phrasing-content-and-flow-content)

###Embedded（嵌入元素）

所有用于在网页中嵌入外部资源的元素均属于Embedded元素，具体包含以下9个：

{% highlight html %}
audio, video, img, canvas, svg, iframe, embed, object, math
{% endhighlight %}

###Interactive（交互元素）

所有与用户交互有关的元素均属于Interactive元素。

{% highlight html %}
a， audio（如果设置了controls属性）， button， details， embed， iframe， img（如果设置了usemap属性）， input（如果type属性不为hidden状态）， keygen， label， menu（如果type属性为toolbar状态），object（如果设置了usemap属性）， select， textarea， video（如果设置了controls属性）
{% endhighlight %}

###Palpable

所有应当拥有子元素的元素称之为Palpable元素。比如，br元素因不需要子元素，因此也就不属于Palpable。

###Script-supporting

自身不做任何页面展现，但与页面脚本相关的元素，具体包括2个：

{% highlight html %}
script, template
{% endhighlight %}

##内容模型(Content Model)

根据以上元素分类，HTML5标准文档定义了任何元素的内容模型 — 对于该元素而言，何种子元素才是合法的。

比如，对于p元素而言，其内容模型为Phrasing, 这意味着p元素只接受Phrasing元素为子元素，而对于像div这样的非Phrasing元素则并不接受。类似的，li元素的内容模型为Flow，因此任何可以放置在body中的元素都可以作为li元素的子元素。

值得注意的是，HTML5标准文档在定义元素的内容模型时，会使用一类特殊的分类：透明内容模型(transparent) — 对于内容模型为透明(transparent)的元素而言，其子元素的合法性由其父元素所决定；如果其父元素的内容模型仍为透明，则查看其祖父元素的情况，并依此类推；如果向上推演至body标签仍未找到任何内容模型非透明的父级元素，则该透明元素内部可包含任何Flow元素。

##正确嵌套标签
元素的嵌套规则和页面头部申明的DTD有着千丝万缕的关系，通过了解HTML5的元素分类与内容模型，我们能更清楚的指导我们元素的嵌套关系。虽然大部分浏览器都有容错机制，写出来的代码在浏览器下表现没有什么异样，但作为一个专业开发人员，我们必须对待自己的代码应该一丝不苟，即使HTML5的胸襟很宽广，但我们更应该去遵从W3C，因为只有标准健壮的代码，才会有更好的扩展与兼容。
