---
layout: post
title:  "(转载)IE、FF、Chrome浏览器中的JS差异介绍"
date:   2015-07-29
categories: FE
---
到目前为止各浏览器的HTML标准或是JS标准都还未统一,但作为一个开发人员，还是有需要了解JS在浏览器中的差异
因为浏览器公司各自为利益考虑，到目前为止各浏览器的HTML标准或是JS标准都还未统一。在平常的开发中，我们常使用的JS框架基本已经帮我们处理好了JS在各浏览器中的差异，但作为一个开发人员，还是有需要了解JS在浏览器中的差异。 

####FF、Chrome:没有window.event对象 
FF、Chrome:没有window.event对象，只有event对象,IE里只支持window.event,而其他主流浏览器两者都支持,所以一般写成：function handle(e){e = e || event;} 

####获取HTML元素 
IE：支持el.name 、el.getAttribute(name) 
FF、Chrome:基本属性支持el.name其余属性仅支持el.getAttribute(name) 

####自定义属性问题 
IE下，可以使用获取常规属性的方法来获取自定义属性，也可以使用 getAttribute() 获取自定义属性；Firefox下，只能使用 getAttribute() 获取自定义属性。 

####Ajax请求 
IE: new ActiveXObject() 
FF、Chrome：new XMLHttpRequest() 

####获取HTML元素 
IE：支持el.name 、el.getAttribute(name) 
FF、Chrome:基本属性支持el.name其余属性仅支持el.getAttribute(name) 

####innerText的使用 

FF不支持innerText，它支持textContent来实现innerText，不过textContent没有像innerText一样考虑元素的display方式，所以不完全与IE兼容。如果不用textContent，字符串里面不包含HTML代码也可以用innerHTML代替。 
```
if(document.all){ 
document.getElementById('element').innerText = "mytext"; 
} else{ 
document.getElementById('element').textContent = "mytext"; 
} 
```

####获取鼠标指针的位置 

计算出鼠标指针的位置对你来说可能是非常少见的，不过当你需要的时候，在IE和Firefox中的句法是不同的。这里所写出的代码将是最最基本的，也可能是某个复杂事件处理中的某一个部分。但他们可以解释其中的异同点。同时，必须指出的是结果相对于Firefox，IE会有更在的不同，这种方法本身就是有BUG的。 
在IE中这样写： 
```
var myCursorPosition = [0, 0]; 

myCursorPosition[0] = event.clientX; 

myCursorPosition[1] = event.clientY; 
```
在Firefox中这样写： 
```
var myCursorPosition = [0, 0]; 

myCursorPosition[0] = event.pageX; 

myCursorPosition[1] = event.pageY; 
```

####获取可见区域、窗口的大小 

有时，我们会需要找到浏览器的可视位置的大小，通常我们称之为"可见区域"。 
在IE中这样写： 
var myBrowserSize = [0, 0]; 
myBrowserSize[0] = document.documentElement.clientWidth; 
myBrowserSize[1] = document.documentElement.clientHeight; 
在Firefox中这样写： 
var myBrowserSize = [0, 0]; 
myBrowserSize[0] = window.innerWidth; 
myBrowserSize[1] = window.innerHeight; 


####CSS "float" 值 

访问一个给定CSS 值的最基本句法是：object.style.property，使用驼峰写法来替换有连接符的值，例如，访问某个ID为"header"的<div>的 background-color值，我们使用如下句法： 
document.getElementById("header").style.backgroundColor= "#ccc"; 
但由于"float"这个词是一个JavaScript保留字，因此我们不能用object.style.float来访问，这里，我们可以在两种浏览器中这么做： 
在IE中这样写： 
document.getElementById("header").style.styleFloat = "left"; 
在Firefox中这样写： 
document.getElementById("header").style.cssFloat = "left"; 

####元素的推算样式 

JavaScript可以使用object.style.property句法，方便地在外部访问和修改某个CSS样式，但其限制是这些句法只能取出已设的行内样式或者直接由JavaScript设定的样式。并不能访问某个外部的样式表。为了访问元素的"推算"样式，我们可以使用下面的代码： 
在IE中这样写： 
```
var myObject = document.getElementById("header"); 

var myStyle = myObject.currentStyle.backgroundColor; 
```
在Firefox中这样写：
``` 
var myObject = document.getElementById("header"); 

var myComputedStyle = document.defaultView.getComputedStyle(myObject, null); 

var myStyle = myComputedStyle.backgroundColor; 
```

####访问元素的"class" 

"class"是JavaScript的一个保留字，在这两个浏览器中我们使用如下句法来访问"class"。 
在IE中这样写：
``` 
var myObject = document.getElementById("header"); 

var myAttribute = myObject.getAttribute("className"); 
```
在Firefox中这样写： 
```
var myObject = document.getElementById("header"); 

var myAttribute = myObject.getAttribute("class");
```