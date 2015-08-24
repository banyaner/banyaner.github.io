---
layout: post
title:  "布移动端布局和字体自适应问题"
date:   2015-08-24
categories: h5
---
移动端字体大小对布局会产生很大影响，因此字体的自适应比pc端要求更高。此文总结下最近看到的解决方案，在此mark下。另外说到字体的自适应不得不提一下**移动端布局**的问题。
显然，移动端最好采用百分比方式进行布局，移动设备尺寸变化范围相对较小，采用百分比能更好满足需求，如果采用媒体查询，必然存在页面“留白情况”。所以个人觉得选择百分比更好。另外，我会利用css3属性设置box-sizing:border-box,以方便对margin、padding进行设置。移动端页面偶尔也会在PC端浏览，所以给htnml设置max-width,可以更好的在pc端浏览。css3中还有个flexbox.该属性可以很轻松的实现多列布局。关于flexbox可以看下[使用CSS3 Flexbox布局]
相关阅读：[移动端H5页面之iphone6的适配]
布局先说到这里。
-------------------------------------

Mobile平台的字体并没有统一的标准规范，大多数平台通常只有一种字体。所以，当你使用了一种字体，最好同时提供一些备选字体，这样当第一种字体不支持的时候，浏览器会选择第二种，依次查找，如果都不支持，浏览器会使用默认字体，下面是各个平支持的字体列表，
iOS browser
American Typewriter、American 、Typewriter Condensed、Arial、Arial Rounded MT Bold、Courier New、Georgia、Helvetica、Marker Felt、Times New Roman、Trebuchet MS、Verdana、Zapfino|
Android browser
Droid
Symbian/S60 S60
Sans
webOS
Arial、Coconut、Verdana

 *关于字体单位**，之前已总结过。下面说说如何实现自适应问题。

rem（font size of the root element）是指相对于根元素的字体大小的单位。设置html的font-size是一种字体自适应方法。关于html的font-size的设置有不同的方法，淘宝使用js计算html的font-size。另外也可以用media query
vw：viewpoint width，视窗宽度，1vw等于视窗宽度的1%。vh：viewpoint height，视窗高度，1vh等于视窗高度的1%。可以利用该单位作为字体单位实现自适应。但利用该单位有个缺点：但屏幕很大时字体也会很大，效果不好。目前还是推荐用媒体查询方式来做。
推荐一篇文章：[web app 变革之rem]
以下引自该文章。
####目前适配屏幕的方法

1、实现强大的屏幕适配布局：
最近iphone6一下出了两款尺寸的手机，导致的移动端的屏幕种类更加的混乱，记得一两年前做web app有一种做法是以320宽度为标准去做适配，超过320的大小还是以320的规格去展示，这种实现方式以淘宝web app为代表作，但是近期手机淘宝首页进行了改版，采用了rem这个单位，首页以内依旧是和以前一样各种混乱，有定死宽度的页面，也有那种流式布局的页面。
我们在在作页面布局的使用常用的单位是px，这是一个绝对单位，web app的屏幕适配有很多中做法，例如：流式布局、限死宽度，还有就是通过响应式来做，但是这些方案都不是最佳的解决方法。

例如流式布局的解决方案有不少弊端，它虽然可以让各种屏幕都适配，但是显示的效果极其的不好，因为只有几个尺寸的手机能够完美的显示出视觉设计师和交互最想要的效果，但是目前行业里用流式布局作web

2.固定宽度做法
还有一种是固定页面宽度的做法，早期有些网站把页面设置成320的宽度，超出部分留白，这样做视觉，前端都挺开心，视觉在也不用被流式布局限制自己的设计灵感了，前端也不用在搞坑爹的流式布局。但是这种解决方案也是存在一些问题，例如在大屏幕手机下两边是留白的，还有一个就是大屏幕下看起来页面会特别小，手机淘宝首页起初是这么做的，但近期改版了，可是天猫首页还没改版。

3.响应式做法
响应式这种方式在国内很少有大型企业的复杂性的网站在移动端用这种方法去做，主要原因是工作大，维护性难，所以一般都是中小型的门户或者博客类站点会采用响应式的方法从web page到web app直接一步到位，因为这样反而可以节约成本，不用再专门为自己的网站做一个web app的版本。

4.设置viewport进行缩放
<meta name="viewport" content="width=320,maximum-scale=1.3,user-scalable=no">
天猫的web app的首页就是采用这种方式去做的，以320宽度为基准，进行缩放，最大缩放为320*1.3 = 416，基本缩放到426都就可以兼容iphone6 plus的屏幕了，这个方法简单粗暴，又高效。说实话我觉得他和用接下去我们要讲的rem都非常高效，不过有部分同学使用过程中反应缩放过程中有些糊，具体我使用没怎么遇到过这种情况。

上面讲了一大堆目前大部分公司主流的一些web app的适配解决方案，接下来讲下rem是如何工作的。

上面说过rem是通过根元素进行适配的，网页中的根元素指的是html我们通过设置html的字体大小就可以控制rem的大小。怎么计算出不同分辨率下font-size的值？
首先假设我上面的页面设计稿给我时候是按照640的标准尺寸给我的前提下，（当然这个尺寸肯定不一定是640，可以是320，或者480，又或是375）来看一组表格。

<img src="http://520ued.com/uploads/20141218/1418903956.jpeg">

上面的表格蓝色一列是Demo3中页面的尺寸，页面是以640的宽度去切的，怎么计算不同宽度下font-site的值，大家看表格上面的数值变化应该能明白。举个例子：384/640 = 0.6，384是640的0.6倍，所以384页面宽度下的font-size也等于它的0.6倍，这时384的font-size就等于12px。在不同设备的宽度计算方式以此类推。

也可以JS去动态计算根元素的font-size，这样的好处是所有设备分辨率都能兼容适配，淘宝首页目前就是用的JS计算。但其实不用JS我们也可以做适配，一般我们在做web app都会先统计自己网站有哪些主流的屏幕设备，然后去针对那些设备去做media query设置也可以实现适配，例如下面这样：
 {%highlight ruby%} 
html {
    font-size : 20px;
}
@media only screen and (min-width: 401px){
    html {
        font-size: 25px !important;
    }
}
@media only screen and (min-width: 428px){
    html {
        font-size: 26.75px !important;
    }
}
@media only screen and (min-width: 481px){
    html {
        font-size: 30px !important; 
    }
}
@media only screen and (min-width: 569px){
    html {
        font-size: 35px !important; 
    }
}
@media only screen and (min-width: 641px){
    html {
        font-size: 40px !important; 
    }
}
{% endhighlight %}
上面的做的设置当然是不能所有设备全适配，但是用JS是可以实现全适配。具体用哪个就要根据自己的实际工作场景去定了。


[使用CSS3 Flexbox布局]:http://www.w3cplus.com/css3/css3-flexbox-layout.html
[移动端H5页面之iphone6的适配]:http://www.ghugo.com/mobile-h5-fluid-layout-for-iphone6/?from=timeline&isappinstalled=0
[web app 变革之rem]:http://520ued.com/article/549125815f85b6b44ca20b2b
