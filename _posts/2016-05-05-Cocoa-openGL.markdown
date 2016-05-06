---
layout: post
title: 初探Cocoa OpenGL
date: 2016-05-05 12:59:45
---

<h4>一、介绍</h4><br/>
在Cocoa 里面有两种画openGL的方法。如果你的程序没有很复杂的渲染需求的话，可以用这种NSOpenGLView的类。
NSOpenGLView类是一种轻量的NSView子类，提供非常方便的方法设置OpenGL drawing。一个NSOpenGLView 对象包含了一个 NSOpenGLPixelFormat 对象和一个NSOpenGLContext对象，其中NSOpenGLContext中的OpenGL调用可以被渲染。它提供访问和管理像素格式对象和渲染内容的方法。NSOpenGLView对象不支持子类，不过你可以通过OpenGL的glViewport方法分离多个视图渲染OpenGL。


<h4>二、实例</h4><br/>

无论你用没用过Xcode，只要跟着以下的步骤都可以画出一个三角形。<br/>
1、建立一个Cocoa项目，命名随意。<br/>
<img class="col three" src="{{ site.baseurl }}/img/opengl/1.png" alt="" title=""/>
2、添加OpenGL framework到你的项目当中。添加方法，点击一下项目文件，在中间的导航栏你会看道Build Phases，然后在Link Binary With Libraries点加号添加OpenGL framework。<br/>
<img class="col three" src="{{ site.baseurl }}/img/opengl/2.png" alt="" title=""/>
3、创建一个Objective-C class。注意在Subclass填写NSOpenGLView。创建完之后有两个文件MyOpenGLView.h,MyOpenGLView.m。<br/>

<div class="img_row">
<img class="col one" src="{{ site.baseurl }}/img/opengl/3.png" alt="" title=""/>
<img class="col two" src="{{ site.baseurl }}/img/opengl/4.png" alt="" title=""/>
</div>


在MyOpenGLView.h添加代码如下:<br/>

{% highlight ruby %}
#import <Cocoa/Cocoa.h>

@interface MyOpenGLView : NSOpenGLView
{
}
- (void) drawRect: (NSRect) bounds;
@end
{% endhighlight %}


在MyOpenGLView.h添加代码如下:<br/>
{% highlight ruby %}
#import "MyOpenGLView.h"
#include <OpenGL/gl.h>

@implementation MyOpenGLView

-(void) drawRect: (NSRect) bounds
{
    glClearColor(0, 0, 0, 0);
    glClear(GL_COLOR_BUFFER_BIT);
    drawAnObject();
    glFlush();
}

static void drawAnObject ()
{
    glColor3f(1.0f, 0.85f, 0.35f);
    glBegin(GL_TRIANGLES);
    {
        glShadeModel(GL_SMOOTH);

        glColor3f(1.0,0.0,0.0);
        glVertex2f(-1.0,-1.0);

        glColor3f(0.0,1.0,0.0);
        glVertex2f(0.0,-1.0);

        glColor3f(0.0,0.0,1.0);
        glVertex2f(-0.5,1.0);
        glEnd();
    }
    glEnd();
}
@end
{% endhighlight %}

4、右击MainMenu.xib，Opan As interface builder。在右方Document下找到OpenGLView，并将其拖进window界面当中，适当调整一下大小。然后，选中刚才拖进去的OpenGLView，在Custom class中的class设置MyOpenGLView。<br/>
<div class="img_row">
<img class="col two" src="{{ site.baseurl }}/img/opengl/6.png" alt="" title=""/>
<img class="col one" src="{{ site.baseurl }}/img/opengl/7.png" alt="" title=""/>
</div>
5、点击运行，查看结果<br/>
<img class="col one" src="{{ site.baseurl }}/img/opengl/8.png" alt="" title=""/>
