---
layout: post
title:  "jekyll 2.5.3 | Error:Permission denied的处理"
date:   2015-08-25
categories: jekyll
---
今天向往常一样，开启jekyll serve，但出现了以下错误

{%highlight ruby%}
c:\devkit\my-awesome-site>jekyll serve
Configuration file: c:/devkit/my-awesome-site/_config.yml
            Source: c:/devkit/my-awesome-site
       Destination: c:/devkit/my-awesome-site/_site
      Generating...
                    done.
 Auto-regeneration: enabled for 'c:/devkit/my-awesome-site'
Configuration file: c:/devkit/my-awesome-site/_config.yml
jekyll 2.5.3 | Error:  Permission denied - bind(2) for 127.0.0.1:4000

{%endhighlight%}

使用jekyll --trace ,可以发现Ruby似乎试图上扣一个套接字,但在绑定套接字的时候出现了许可问题。jelyll默认端口是4000。我使用本地的另外一个没在运行的应用的端口（8080）来替代4000，结果可以正常运行了。可以使用netstat -an看哪些端口正在被使用。
修改端口的方式件是打开_config.yml，添加
{%highlight ruby%}
port:    8080
host:    127.0.0.1
{%endhighlight%}
_config.yml配置具体信息科参考：[jekyll配置]

[jekyll配置]:http://jekyllcn.com/docs/configuration/