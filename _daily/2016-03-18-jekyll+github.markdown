---
layout: post
title: 利用jekyll+github配置静态网站
date: 2016-03-18 07:59:00
---

［仅供新手参考］

1.在github申请账户，建立一个你注册账户的名字相关的仓库. 例子: 我的账户是name,那么仓库的名字就是name.github.io
,name.github.io就是你的主页了。如果你想有一个个性一点的域名，那就去买一个，推荐godaddy，免登记。<br/><br/>
2.windows的话就去下载ruby + jekyll 的插件，os x 的Xcode是自带Ruby的，直接在终端下载就可以<br/><br/>
  <pre>gem install jekyll</pre>
3.这些都配置之后就可以jekull theme 上找一个模版练练手。点进去你选的那个之后就去fork一下，你把文件放到你的name.github.io里面也行，把你的仓库删掉，改你fork的那个名字也可以。
<br/><br/>当然你可以自己在本地建立一个测试。
  <pre>
    jekyll new blog   #创建你的站点
    cd blog    	      #进入blog目录 blog是你新建blog的路径
    jekyll serve      #启动你的http服务</pre>
4.开启服务器后，jekyll服务器端口是4000，访问主页http://localhost:4000。<br/><br/>
5.根据一般的文件结构，你写的文章会放在_post这个文件里，里面是一些用markdown格式写的文章
  <pre>year-month-day-title.markdown ＃文件格式</pre>
6.接下来码字,你的文章格式是这样的
<pre>
---
layout: post
title:  标题
date:   2016-03-18 23:44:59
---
内容
</pre>

<div class="ds-thread" data-thread-key="" data-title="利用jekyll+github配置静态网站" data-url="http://wotocheng.com/daily/2016-03-18-jekyll+github/"></div>
<!-- 多说评论框 end -->
<!-- 多说公共JS代码 start (一个网页只需插入一次) -->
<script type="text/javascript">
var duoshuoQuery = {short_name:"wotojc"};
(function() {
  var ds = document.createElement('script');
  ds.type = 'text/javascript';ds.async = true;
  ds.src = (document.location.protocol == 'https:' ? 'https:' : 'http:') + '//static.duoshuo.com/embed.js';
  ds.charset = 'UTF-8';
  (document.getElementsByTagName('head')[0]
   || document.getElementsByTagName('body')[0]).appendChild(ds);
})();
</script>
