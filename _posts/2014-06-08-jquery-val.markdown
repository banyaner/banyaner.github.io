---
layout: post
title:  "jQuery中html()、text()、val()的区别"
date:   2014-06-08 
categories: [javascript,jquery]
---
.html()用为读取和修改元素的HTML标签    对应js中的innerHTML

.html()是用来读取元素的HTML内容（包括其Html标签）

.html()方法使用在多个元素上时，只读取第一个元素
 
.text()用来读取或修改元素的纯文本内容  对应js中的innerText

.text()用来读取元素的纯文本内容，包括其后代元素

.text()方法不能使用在表单元素上
 
.val()用来读取或修改表单元素的value值

.val()是用来读取表单元素的"value"值

.val()只能使用在表单元素上

.val()方法和.html()相同，如果其应用在多个元素上时，只能读取第一个表单元素的"value"值，但是.text()和他们不一样，如果.text()应用在多个元素上时，将会读取所有选中元素的文本内容