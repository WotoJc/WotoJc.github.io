---
layout: post
title: 多边形的扫描转换算法与区域填充(利用js实现)
date: 2016-05-20 11:34:43
---
<h3>一、概念</h3>
<hr style="margin:10px">
<p>
1、顶点表示是用多边形的顶点序列来表示多边形。这种表示直观、几何意义强、占内存少，易于进行几何变换，但由于它没有明确指出哪些象素在多边形内，故不能直接用于面着色
</p>

<p>
2、点阵表示是用位于多边形内的象素集合来刻画多边形。这种表示丢失了许多几何信息，但便于帧缓冲器表示图形，是面着色所需要的图形表示形式。光栅图形的一个基本问题是把多边形的顶点表示转换为点阵表示，这种转换称为多边形的扫描转换。
</p>

<p>
3、光栅图形的一个基本问题是把多边形的顶点表示转换为点阵表示，这种转换称为多边形的扫描转换。
</p>
<img class="col three" src="{{ site.baseurl }}/img/FillDraw.png" alt="实现效果" title=""/>

<br/>
<h3>二、实现步骤</h3>
<hr style="margin:10px">
<p>1、获取封闭多边形的顶点</p>

<p>2、扫描顶点，生成边表NET</p>
<p>3、运行扫描线算法</p>
<p style="margin-left:20px">(1)计算扫描线与多边形各边的交点</p>
<p style="margin-left:20px">(2)把所有交点的x值递增顺序排序</p>
<p style="margin-left:20px">(3)每对交点代表扫描线与多边形的一个相交区间</p>
<p style="margin-left:20px">(4)把相交区间内的象素置成多边形颜色</p>

<h3>三、主要代码</h3>


<hr style="margin:10px">
<a href="https://github.com/WotoJc/PostFile/tree/master/FillPolygon" target="_blank" style="margin:10px">点击链接查看源代码</a>
{% highlight ruby %}

/**
* 边表对象，x 记录 x 的坐标纸，maxy 边的最高坐标y，dx 边的导数，line 扫描边值
*/
function  ET(a,b,c,d)
{
    this.x= a;
    this.dx= b;
    this.maxy = c;
    this.line = d;
}

// 点、活性边表、边表
var point = new Array();
var AET = new Array();
var NET = new Array();
// 多边形y的范围
var maxY = 0;
var minY = 1000 ;

// 画直线（这里填充图形没有用到点填充，直接用了线填充）
function drawLine(x, y,x1,y1) {
    var canvas = document.getElementById("board");
    var context = canvas.getContext('2d');
    context.lineWidth = 1;
    context.strokeStyle="black";
    context.moveTo(x,y);
    context.lineTo(x1,y1);
    context.stroke();
}

//画多边形
function draw(){
    //获取窗口鼠标坐标
    var x = event.clientX;
    var y = event.clientY;

    // 把顶点添加到数组
    var p = new Array(x,y);
    point.push(p);
    if(point.length > 1){
        var n = point.length;
        drawLine(point[n-2][0],point[n-2][1],point[n-1][0],point[n-1][1]);
    }
}

// 转换顶点到边表
function fillDraw(){
    //连接最后一条边
    var n = point.length;
    drawLine(point[0][0],point[0][1],point[n-1][0],point[n-1][1]);
    // 计算maxX 和maxY
    for (var i = 0 ; i < point.length ;i++){
        if( point[i][1] > maxY){
            maxY = point[i][1];
        }
        if( point[i][1] < minY){
            minY = point[i][1];
        }
    }

    // 生成边表
    initNET();

    // 根据活性边表 填充多边形
    fillPolygon();
}

function initNET()
{
    //获取顶点数
    var n = point.length;
    //添加边表
    for (var i = minY ; i <= maxY; i++)
    {
        for(var j = 0 ; j < point.length ; j++)
        {
            if(point[j][1] == i)
            {
                if(point[(j-1+n) % n][1] > point[j][1])
                {
                    var x = point[j][0];
                    var maxy = point[(j-1+n) % n][1] ;
                    var dx = ( point[(j-1+n) % n][0] - point[j][0] )/( point[(j-1+n) % n][1]- point[j][1])*1.0;
                    var et =  new ET(x,dx,maxy,i);
                    NET.push(et);
                }
                if(point[(j+1+n) % n][1] > point[j][1])
                {
                    var x = point[j][0];
                    var maxy = point[(j+1+n) % n][1] ;
                    var dx = ( point[(j+1+n) % n][0] - point[j][0] )/( point[(j+1+n) % n][1]- point[j][1])*1.0;
                    var et =  new ET(x,dx,maxy,i);
                    NET.push(et);

                }
            }
        }
    }
}

// 根据活性边表 填充多边形
function fillPolygon()
{
    for(var i = minY ; i < maxY ;i++)
    {
        //添加与扫描线相交的边到活性边表
        for(var j = 0 ; j < NET.length ; j++)
        {
            if (NET[j].line == i)
            {
                AET.push(NET[j]);
            }
        }

    //顺序排序
    AET.sort(sortet);

    //移除已完成的填充的活性边表
    for (var k = 0; k < AET.length; k++)
    {
        if ( AET[k].maxy <= i)
        {
            AET.splice(k,1);
            // 这里要注意 －－k 不然某些特别的多边形会出错
            --k;
        }
    }

    //填充图形
    if (AET.length > 0)
    {
          var can = false;
          for (var k = 0; k < AET.length - 1; ++k)
          {
              can = !can;
              if (can)
              {
                  // 得到起点和终点，成对出现
                  var start = AET[k].x;
                  var end = AET[k+1].x;
                  // 填充
                  drawLine(start,i,end,i);
              }
              AET[k].x += AET[k].dx;
          }
          var n = AET.length;
          AET[n-1].x += AET[n-1].dx;
      }
  }
}

//如果x不相等，则根据x比较，小的返回true，如果x相等，根据斜率比较，小的返回true
function sortet(a,b) {
    if(a.x != a.b)
        return a.x < b.x ? -1 : 1;
        return a.dx < b.dx ? -1 : 1;
}

{% endhighlight %}
