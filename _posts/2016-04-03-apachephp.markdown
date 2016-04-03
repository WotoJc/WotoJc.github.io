---
layout: post
title: OS X 配置Apache+PHP服务器
date: 2016-04-02 00:00:00
---

－以10.11为例－
Mac OS X 已经集成了Apache、php的开发环境。<br/>
在你开机的时候，你可以访问http://localhost或者http://127.0.01,端口为80(在我的机子是80，不知道其他是不是),界面显示着it works.的字样。其实
这时候自带的apache是默认开启的，文件默认目录在/Library/WebServer/document/index.html.
<br/>
在terminal里开启关闭命令：
<pre>
sudo apachectl start      //开启
sudo apachectl stop       //关闭
sudo apachectl restart    //重启
</pre>

<br/>
接下来，手动配置已经集成的PHP。需要找到httpd.conf文件。
<pre>
/etc/apache2/httpd.conf
</pre>
然后查找到这条内容，并把前面的＃去掉<br/>
LoadModule php5_module libexec/apache2/libphp5.so
<br/>
接下来你用命令重启一下Apache，在默认目录下建一个php文件，添加
<pre
<?php phpinfo(); ?>
</pre>
看一下是否成功
