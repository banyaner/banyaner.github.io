---
layout: post
title:  "移动开发基本知识"
date:   2015-08-13
categories: [h5]
---
###[移动开发规范概述]

#### 字体设置

使用无衬线字体
<pre>
body {
    font-family: "Helvetica Neue", Helvetica, STHeiTi, sans-serif;
}
</pre>
iOS 4.0+ 使用英文字体 Helvetica Neue，之前的iOS版本降级使用 Helvetica。中文字体设置为华文黑体STHeiTi。 需补充说明，华文黑体并不存在iOS的字体库中(http://support.apple.com/kb/HT5878)， 但系统会自动将华文黑体 STHeiTi 兼容命中系统默认中文字体黑体-简或黑体-繁
原生Android下中文字体与英文字体都选择默认的无衬线字体
    4.0 之前版本英文字体原生 Android 使用的是 Droid Sans，中文字体原生 Android 会命中 Droid Sans Fallback
    4.0 之后中英文字体都会使用原生 Android 新的 Roboto 字体
    其他第三方 Android 系统也一致选择默认的无衬线字体

#### 基础交互

设置全局的CSS样式，避免图中的长按弹出菜单与选中文本的行为
<pre>
a, img {
    -webkit-touch-callout: none; /* 禁止长按链接与图片弹出菜单 */
}
html, body {
    -webkit-user-select: none;   /* 禁止选中文本（如无文本选中需求，此为必选项） */
    user-select: none;
}
</pre>
(性能优化可以看原文)
一些问题
1. viewport设置

设置width=device-width  

*(不设 target-densitydpi值，默认为medium-dpi，即160dpi中密度屏幕显示效果）*
<pre>

设备               屏幕尺寸   物理像素    独立像素    devicePixelRatio缩放比例    屏幕密度
非视网膜iPhone      3.5英寸       320            320              1                  中密度（mdpi）
视网膜屏iPhone      3.5英寸        640           320              2                  极高密度（xhdpi）
Android QVGA屏幕    3英寸          240           320              0.75               低密度屏（ldpi）
Android HVGA屏幕    3.5英寸        320           320              1                  中密度（mdpi）
Android WVGA屏幕   4英寸           480           320              1.5                高密度屏（hdpi）
Galaxy Nexus        4.65英寸       720           360              2                  极高密度（xhdpi）
Galaxy Note         5.3英寸        800           400              2                  极高密度（xhdpi）

</pre>
设置target-densitydpi=device-dpi, width=device-width此时独立像素和物理像素相等 devicePixelRatio为1，即不进行缩放

2. Android设备border-radius存在锯齿

安卓自带浏览器及Webview对border-radius支持效果不一，大多数浏览器较差，有明显锯齿，尤其是在背景为图片的情况下；
若要兼容自带浏览器或是需要在Webview中打开的页面，且要求较高情况下，需要使用图片实现；
有一种补偿方式：
<pre>
.rounded{
   box-shadow:0 0 1px rgba(0,0,0,.5);//使用阴影来减轻锯齿；
   -webkit-box-shadow:0 0 1px rgba(0,0,0,.5);//以防支持性问题
}
</pre>

[移动开发规范概述]:http://alloyteam.github.io/Spirit/modules/Standard/
