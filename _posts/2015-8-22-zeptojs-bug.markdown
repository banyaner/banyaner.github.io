---
layout: post
title:  "zepto使用注意点"
date:   2015-08-22
categories: js
---

最近一直在写移动端的项目，主要用到的库是zeptojs，只知道可以把zeptojs当成jQuery来用，但是一些细节上还是不太清楚。在网上看了写资料，在此，小做总结。
 jQuery 的目标是兼容所有主流浏览器，所以它有大量代码对移动端的浏览器是无用或者低效的。而zepto只是针对移动端浏览器编写的，因此体积小、效率高、适应移动端设备的网速。它的API完全仿照jQuery，因此学习成本很低。


### 从哪里下载 Zepto（转载）

这个问题看起来很蠢，从官网下载不就行了嘛！可是你有没有发现下载链接上面有行小字呢？

There are more modules; a list of all modules is available in the README.
在这个 README 里面你会惊奇地发现，Zepto 源码中有 14 个模块，而官网提供的标准版里面只有 7 个模块！而且居然不包含对移动端开发非常重要的 touch 模块（提供对触摸事件的支持）！

所以我的建议是，不要从官网下载，而是从 Github 下载了源代码之后自己 Build 一个版本，这样你可以自行挑选适合的模块。比如我挑选的模块是这么几个：
polyfill，zepto，detect，event，ajax，form，fx 这7个就是标准版包含的模块

- fx_methods 有了这个模块之后，.show() .hide() 等几个方法才能支持动画了，比如 .show('fast')
- data 提供对 .data() 方法的完整支持，像 jQuery 一样用内存对象存储
- assets 移除 img 元素后做一些特殊处理，用来清理内存
- selector 更多的选择器的支持，后面会提到
- touch 对触摸事件的支持，比如 tap 事件

### 不要使用click，用tap代替

移动端click事件会有300ms左右的延迟。产生的原因是，在移动端存在双击缩放的功能，用户touchend事件发生后，浏览器会等300ms左右，等待用户是否会再次点击。如果没有点击，则判断为click事件。关于这个问题详细的解释可以看一下
[ghost click]。

### zepto对css选择器的支持
郑重提醒，:text :checkbox :first 等等在 jQuery 里面很常用的选择器，Zepto 不支持！
原因很简单，jQuery 通过自己编写的 sizzle 引擎来支持 CSS 选择器，而 Zepto 是直接通过浏览器提供的 document.querySelectorAll 接口。
这个接口只支持标准的 CSS 选择器，而上面提到的那些属于 jQuery 选择器扩展，所以仔细看看这个网页，注意一下这些选择器。

当然也有好消息，就是上面提到的 selector 模块，如果有这个模块的话，能够支持 部分 的 jQuery 选择器扩展，列举如下：

- :visible :hidden
- :selected :checked
- :parent
- :first :last :eq
- :contains :has

#### 关于Sizzle选择器引擎
我们知道浏览器最终会将HTML文档（或者说页面）解析成一棵DOM树。
为了操作到当中那个checkbox，我们需要有一种表述方式，使得通过这个表达式让浏览器知道我们是想要操作哪个DOM节点。
这个表述方式就是CSS选择器，它是这样表示的：div > p + .clr input[type="checkbox"]
表达的意思是，div底下的p的兄弟节点，该节点的class为clr，并且其属性type为checkbox。
常见的选择器：
- test表示id为test的DOM节点
- .clr表示class为clr的DOM节点
- input表示节点名为input的DOM节点
- div > p表示div底下的p的DOM节点
- div + p表示div的兄弟DOM节点p
- 
浏览器提供了一些接口可以让javascript取到DOM树的某个节点。

- document.getElementById(“test”)，获取id为test的DOM节点
- document.getElementsByTagName(“input”)，获取节点名为input的DOM节点
- document.getElementsByName(“checkbox”)，获取属性name为checkbox的DOM节点

高级的浏览器还提供document.getElementsByClassName，用来获取class为参数值的DOM节点。其实浏览器能够直接提供一个接口，我们直接把选择器表达式传进去，然后其返回一个符合规则的DOM节点列表。
其实在高级浏览器里边，这个接口是存在的，它就是：document.querySelectorAll。由于低级浏览器并未提供这些高级点的接口，所以才有了Sizzle这个CSS选择器引擎。Sizzle引擎提供的接口跟document.querySelectorAll是一样的，其输入是一串选择器字符串，输出则是一个符合这个选择器规则的DOM节点列表。想了解更多可以看下:[Sizzle选择器引擎之词法分析]

### 元素的尺寸计算
首先 Zepto 没有 .innerHeight() .outerWidth() 等四个方法，其次，它的 .height()/.width() 方法也不完善，对于 display:none 的元素，计算出的高宽都是 0
而这在 jQuery 里面是没有问题的，因为 jQuery 针对这种元素，会先设置其 css 样式设置为 position: "absolute", visibility: "hidden", display: "block" 
计算完高宽后再恢复，参见 https://github.com/jquery/jquery/blob/master/src/css.js#L460
如果遇到这种特殊情况，可以参考 jQuery 写一个类似的方法

### .show() 的动画效果
如果没有 fx_mehods 模块的话，.show() 方法是不支持动画的，不过有了这模块后，动画的支持还是有点小问题，比如这么一段 HTML

{% highlight ruby%}
<div style="background:black;opacity:0.7;display:none">
    test
</div>
{% endhighlight %}

如果你调用 $('div').show('fast') ，那么动画完成后你看到的不会是一个半透明的元素，而是全黑不透明的
因为 Zepto 的 .show() 动画实现的很简单，没有高宽的变化，而是将透明度从 0 逐渐变为 1，所以元素上原来设置的透明度就被替代了。
这种情况下，可以用 .fadeIn() 方法来替代 .show()

[ghost click]:http://ariatemplates.com/blog/2014/05/ghost-clicks-in-mobile-browsers/
[Sizzle选择器引擎之词法分析]:http://rapheal.sinaapp.com/2013/02/05/jquery-src-sizzle-tokenize/





