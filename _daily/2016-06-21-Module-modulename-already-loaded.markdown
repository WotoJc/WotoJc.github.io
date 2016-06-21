---
layout: post
title: (PHP)Module ‘PDFlib’ already loaded in Unknown on line 0
date: 2016-06-20 11:50:23
---

今天在终端里面测试php遇到了一个问题。编译php命令时一直这样的错误。
<pre>
PHP Warning: Module ‘PDFlib’ already loaded in Unknown on line 0
</pre>
<code>
原因问题是：PHP版本升级了，不需要再次引用 PDFLib这个扩展。
解决办法：找到在phpinfo()中Scan this dir for additional .ini files的选项，在路径中找到ext-pdf.ini文件，将其删除即可。
</code>
