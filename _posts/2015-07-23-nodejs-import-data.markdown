---
layout: post
title:  "使用excel内数据更新PostgreSQL数据库"
date:   2015-07-23 
categories: nodejs
---
- 1.将excel文件存储为.csv格式，本例中文件为（information-baike.csv）。
- 2.使用记事本打开information-baike.csv，存储为utf-8编码
- <img src="https://github.com/banyaner/banyaner.github.io/blob/master/img/nodejsimport.png?raw=true" alt="另存为utf-8编码">
- 3.使用nodejs写一段脚本执行导入
先安装pg  /参考：[https://github.com/brianc/node-postgres]
{% highlight ruby %}
 npm --save install pg
{% endhighlight %}
nodejs读取文件操作：[https://nodejs.org/api/fs.html#fs_fs_readfile_filename_options_callback]

代码：
{% highlight ruby %}
var pg = require('pg');
var fs = require('fs');
fs.readFile('E:/information-baike.csv',{
encoding:'utf-8'
}, function (err, data) {
var lines = data.split('\r\n');
for(var i=1;i<\lines.length;i++) {
var cols = lines[i].split(',');
var id = cols[0];
var bkId = cols[2];
var summary=cols[3];
updatePg(id,bkId,summary);
    }
});
function updatePg(id,bkId,summary) {
var client = new pg.Client('postgres://username:password@localhost/database');
var conn = client.connect();
client.query('UPDATE c_core_adminbdy SET baike_id=$1,summary=$2 WHERE id=$3',[bkId,summary,id],function() {
client.end();
    });
}
{% endhighlight %}

**补充**：
1.使用'UPDATE c_core_adminbdy SET baike_id=$1,summary=$2 WHERE id=$3',[bkId,summary,id]写法是为了避免sql注入以及内容中的双引号/单引号引起语句执行错误

2.\n 软回车：在Windows 中表示换行且回到下一行的最开始位置。相当于Mac OS 里的 \r 的效果。在Linux、unix 中只表示换行，但不会回到下一行的开始位……
\r 软空格：在Linux、unix 中表示返回到当行的最开始位置。在Mac OS 中表示换行且返回到下一行的最开始位置，相当于Windows 里的 \n 的效果。

[https://github.com/brianc/node-postgres]:https://github.com/brianc/node-postgres
[https://nodejs.org/api/fs.html#fs_fs_readfile_filename_options_callback]:https://nodejs.org/api/fs.html#fs_fs_readfile_filename_options_callback