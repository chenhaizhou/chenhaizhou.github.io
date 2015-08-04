---
layout: post
title: 移动Web 开发中的一些前端知识收集
tags:
 - Web
 - F2e
 - Html
 - Css3
 - Html5
---

要说移动Web 开发与传统的PC 端开发，感觉也没什么不同，但得益于苹果对于智能机的推动，CSS3+HTML5几乎可以毫无顾忌的使用，然后浏览器端考虑webkit内核的就差不多了。
<!--more-->

###webkit内核中一些私有的meta标签###

{% highlight html %} 
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
<meta name="apple-mobile-web-app-status-bar-style" content="black" />
<meta name="apple-mobile-web-app-title" content="测试APP">
{% endhighlight %} 

第一个meta标签是iphone设备中的safari私有meta标签，它表示：允许全屏模式浏览，在ios上，用户将网页添加到主屏后，再从主屏幕打开这个网页，可以隐藏浏览器的地址栏和下面的toolbar；

第二个meta标签表示：强制让文档的宽度与设备的宽度保持1:1，并且文档最大的宽度比例是1.0，且不允许用户点击屏幕放大浏览；

第三个meta标签也是iphone的私有标签，它指定的iphone中safari顶端的状态条的样式，其值有三个：default、black、black-translucent。

第四个meta标签是指在发送到屏幕的时候默认的命名。

![/images/mobile1.jpg](/images/mobile1.jpg)

另外还有其他一些meta标签，表示的意思直接见注释：

{% highlight html %} 
<meta content="telephone=no" name="format-detection" />  <!--告诉设备忽略将页面中的数字识别为电话号码-->
<meta content="email=no" name="format-detection" /> <!--不让android识别邮箱-->
{% endhighlight %} 


###自定义主屏上的图标###

用户添加到主屏后，如果网站没有图标，则默认主屏上的图标为当前网页的截图，你可以通过下面的代码指定在普通和retina屏幕上的icon：

{% highlight html %} 
<link rel="apple-touch-icon"  sizes="72x72"  href="apple-touch-icon.png">
<link rel="apple-touch-icon-precomposed"  sizes="72x72"  href="apple-touch-icon-precomposed.png">
{% endhighlight %} 

![/images/mobile2.jpg](/images/mobile2.jpg)

###添加初始化图片###

用户点击你桌面上的webapp的图标后，打开会加载浏览器（实际上是webkit webview模块），然后下载、解析、渲染，在这个过程中，ios允许我们使用一个初始化图片来替代白色的浏览器屏幕：

{% highlight html %} 
<link rel="apple-touch-startup-image" sizes="320x460" href="imgs/setupImg320.png" />
<link rel="apple-touch-startup-image" sizes="640x920" href="imgs/setupImg640.png" />
<link rel="apple-touch-startup-image" sizes="640x1096" href="Images/setupImg5.png" />
{% endhighlight %} 

你可以查看[《将你的网站打造成一个iOS Web App》](http://devework.com/ios-web-app.html)、[《iOS / Android 移动设备中的 Touch Icons》](http://devework.com/ios-android-touch-icons.html)这两篇文章了解更多。

###关闭iOS中键盘自动大写、自动更正、自动完成###

在iOS中，当虚拟键盘弹出时，默认情况下键盘是开启首字母大写的功能的，根据某些业务场景，可能我们需要关闭这个功能，移动版本webkit为input元素提供了autocapitalize属性，通过指定autocapitalize=”off”来关闭键盘默认首字母大写。还有的是自动更正、自动完成给你可以一并取消：

{% highlight html %} 
<input autocorrect=”off” autocomplete=”off” autocapitalize=”off”>
{% endhighlight %} 

###文件上传, 从相机捕获媒体###

{% highlight html %} 
<input type="file" accept="image/*;capture=camera" />
<input type="file" accept="video/*;capture=camcorder" />
<input type="file" accept="audio/*;capture=microphone" />
{% endhighlight %} 

###电话短信###

{% highlight html %} 
<a href="sms:18888886666,18888885555"> 发送短信给多个人 的链接
<a href="sms:18888886666?body=sms txt"> 发送短信附带内容 的链接
<a href="tel:18888886666">Call us at 18888886666</a> 拨打电话的链接
{% endhighlight %} 

###移除 iOS 默认的按钮样式###

在iOS 中，默认会将所有的按钮（input）强制加上一个圆角和渐变样式（IOS7的不知是怎样的了），要移除这个默认样式，用下面的代码（建议直接reset那里添加）：

{% highlight css %} 
input{-webkit-appearance:none;outline:none;}
{% endhighlight %} 

###iOS 浏览器横屏时会重置字体大小的问题###

iOS 浏览器横屏时会重置字体大小，设置 text-size-adjust 为 none 可以解决ios上的问题，但桌面版safari的字体缩放功能会失效，因此最佳方案是将 text-size-adjust 为 100% 。

{% highlight css %} 
-webkit-text-size-adjust: 100%;
-ms-text-size-adjust: 100%;
text-size-adjust: 100%;
{% endhighlight %} 

###CSS3的transition 闪屏问题###

使用css3动画的时尽量利用3D加速，从而使得动画变得流畅（可参考[《移动Web 开发中的 Off Canvas 导航》](http://devework.com/off-canvas.html)这篇文章）。动画过程中的动画闪白可以通过backface-visibility 隐藏。

{% highlight css %} 
-webkit-transform-style: preserve-3d;/*设置内嵌的元素在 3D 空间如何呈现：保留 3D*/
-webkit-backface-visibility: hidden;/*（设置进行转换的元素的背面在面对用户时是否可见：隐藏）*/
{% endhighlight %} 

###其他CSS的杂项###

{% highlight css %} 
-webkit-tap-highlight-color: transparent; /*Mobile上点击链接高亮的时候设置颜色为透明*/
-webkit-user-select: none; /*设置为无法选择文本*/
-webkit-touch-callout: none; /*长按时不触发系统的菜单（禁止ios弹出各种操作窗口）, 可用在图片上加这个属性禁止下载图片*/
-webkit-overflow-scrolling: touch;/*快速滚动和回弹，模拟原生app效果*/
{% endhighlight %} 

###click 事件###

ios的safari的click事件在短按屏幕时会有明显延迟（相对用户手离开屏幕那一刻大约300ms），因此建议采用 touchstart 事件。或者是说使用封装的 tap 事件来代替click 事件，所谓的 tap 事件由 touchstart 事件 + touchmove 判断 + touchend 事件封装组成。

###其他js杂项###

{% highlight js %} 
window.scrollTo(0,0); /*隐藏地址栏*/
window.matchMedia(); /*匹配媒体*/
navigator.connection; /*决定手机是否运行在WiFi/3G等网络*/
window.devicePixelRatio; /*决定屏幕分辨率(iPhone 4值为2, 而Nexus One值为1.5)*/
window.navigator.onLine; /*取得网络连接状态*/
window.navigator.standalone; /*决定iPhone是否处于全屏状态*/
{% endhighlight %} 

touch事件 (iOS, Android 2.2+): touchstart, touchmove, touchend, touchcancel

gesture事件 (Apple only, iOS 2+):  gesturestart, gesturechange, gesturend give access to predefined gestures (rotation, scale, position)

