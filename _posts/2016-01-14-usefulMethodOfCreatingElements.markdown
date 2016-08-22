---
layout: post
title:  "使用jquery创建新元素的最好方式"
date:   2016-01-14
categories: [js]
---

  我使用一个对象来放置使用jquery进行 create、append、remove 的新新元素
```js
var el = {
    newDiv : $("<div>", {class: "container", id: "div1", align: "center", "text":"div中的内容"}),
    newImg : $("<img>", {class: "profile-image", id: "newImg", src: "path/to/img"}),
    newAnchor : $("<a>", {class: "link", id: "newAnchor"})
};
```
这里创建了一个el对象放置新元素，以便在代码中更方便使用。
###append元素
```js
el.newImg.appendTo(newDiv);
el.newAnchor.appendTo(newDiv);
el.newDiv.appendTo("body");
```
以上代码会将a表情和img放在div内，div放入body内。
###修改新元素css
```js
el.newDiv.css({
    "display" : "block",
    "position" : "relative",
});
```
###删除新元素
```js
el.newImg.remove();
el.newAnchor.remove();
```

