---
layout: post
title:  "git ssh key"
date:   2014-08-06 
categories: [jekyll, git]
---
一 设置git的user name和email：
{% highlight ruby %}
$ git config --global user.name "banyaner"
$ git config --global user.email "1129103472@qq.com"
{% endhighlight %}
 
二 生成密钥
 
ssh-keygen -t rsa -C “1129103472@qq.com”
 
 按3个回车，密码为空。(不要输密码)
 
然后到.ssh下面将id_rsa.pub里的内容复制出来粘贴到github个人中心的账户设置的ssh key里面
三 测试
{% highlight ruby %}
$ ssh -T git@github.com
{% endhighlight %}
你将会看到：

  {% highlight ruby %}
  The authenticity of host 'github.com (207.97.227.239)' can't be established.
    RSA key fingerprint is 16:27:ac:a5:76:28:2d:36:63:1b:56:4d:eb:df:a6:48.
    Are you sure you want to continue connecting (yes/no)?
{% endhighlight %}
选择 yes
如果看到Hi后面是你的用户名，就说明成功了。
四 修改.git文件夹下config中的url

修改前
{% highlight ruby %}
    [remote "origin"]
    url = https://github.com/banyaner/banyaner.github.io.git
    fetch = +refs/heads/*:refs/remotes/origin/*
 {% endhighlight %}
修改后
{% highlight ruby %}
    [remote "origin"]
    url = git@github.com:banyaner/banyaner.github.io.git
    fetch = +refs/heads/*:refs/remotes/origin/*
{% endhighlight %}
五 成功
