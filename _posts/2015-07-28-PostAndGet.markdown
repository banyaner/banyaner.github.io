---
layout: post
title:  "POST和GET请求的区别"
date:   2015-07-28 
categories: [FE]
---
GET 和 POST是两种常见的 HTTP 请求。

GET - 从指定的资源请求数据。

POST - 向指定的资源提交要被处理的数据。

###GET 请求

查询字符串（名称/值对）是在 GET 请求的 URL 中发送的：

/test/test.php?name1=value1&name2=value2

####有关 GET 请求的一些特点：

- GET 请求可以被缓存
- GET 请求保留在浏览器历史记录中
- GET 请求可被收藏为书签
- GET 请求不应在处理敏感数据时使用（即通常所说的数据不加密）
- GET 请求有长度限制
- GET 请求只应当用于从服务器取回数据

###POST 请求

查询字符串（名称/值对）是在 POST 请求的 HTTP 消息主体中发送的：

POST /test/test.php HTTP/1.1
Host: baidu.com
name1=value1&name2=value2

####有关 POST 请求的一些特点：

- POST 请求不会被缓存
- POST 请求不会保留在浏览器历史记录中
- POST 请求不能被收藏为书签
- POST 请求对数据长度没有要求
- POST 请求可以用于处理敏感数据

###POST和GET比较
<img src="https://github.com/banyaner/banyaner.github.io/blob/master/img/GetAndPost.png?raw=true">