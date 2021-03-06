# 核心知识点

- 地理定位
- 本地存储
- 缓存
- canvas

## 多媒体操作

```js
   ☞  volume   设置或返回当前音量大小 
   
   	   注意： 必须是介于 0.0 与 1.0 之间的数字。
       video.volume=0.2;

   ☞  playbackRate  设置播放过程中的播放速度
   	   取值： 1.0 正常速度  
       
       video.playbackRate=0.5;

   ☞  video.muted    设置播放过程中是否静音
   	   true 【静音】
       false 【正常】
       
   ☞ load()	  重新加载
   
   ☞ 全屏
		//进入全屏
 2 function requestFullScreen() {
 3     var de = document.documentElement;
 4     if (de.requestFullscreen) {
 5         de.requestFullscreen();
 6     } else if (de.mozRequestFullScreen) {
 7         de.mozRequestFullScreen();
 8     } else if (de.webkitRequestFullScreen) {
 9         de.webkitRequestFullScreen();
10     }
11 }
12 //退出全屏
13 function exitFullscreen() {
14     var de = document;
15     if (de.exitFullscreen) {
16         de.exitFullscreen();
17     } else if (de.mozCancelFullScreen) {
18         de.mozCancelFullScreen();
19     } else if (de.webkitCancelFullScreen) {
20         de.webkitCancelFullScreen();
21     }
22 }
```

## 地理定位

```reStructuredText
☞ 常见定位方式：
	 1. IP地址
     2. GPS
     3. 坐标/经度纬度

 ☞ H5中定位API
 		Geolocation API
     
     获取一次位置信息
     1. navigator.geolocation.getCurrentPosition(成功回调函数，失败回调函数)；
     如果成功：
     	coords.latitude  纬度
        coords.longitude 经度
        
        coords.accuracy	 位置精度
        coords.altitude  海拔
        coords.speed	 移动的速度
        coords.heading	  方向
        
     如果失败： 
		Permission denied      用户禁止定位
         Position unavailable   无法获取当前位置信息
         Timeout 		       操作超时
   
   
    2. navigator.geolocation.watchPosition(成功回调函数，失败回调函数)

 ☞ 地图定位案例
```

## web存储

```js
☞ HTML5 提供了两种在客户端存储数据的新方法：
  	   1. localStorage【长期存储，大小为约20M】
       2. sessionStorage【临时存储，大小为5M】
       
  ☞ localStorage
  	    增加：
        	localStorage.setItem(key,value);       注意： value值必须是字符串形式
        获取
        	localStorage.getItem(key)			  通过键获取对应的值
        删除
        	localStorage.removeItem(key)		  删除指定键的值
        清空
        	 localStorage.clear();

  ☞ 将字符串转为对象
  	  JSON.stringify(value)     将一个对象解析成字符串格式
      JSON.parse()			   将一个字符串转换为对象格式
```

## 缓存

```js
特点介绍：
    1、离线浏览，无网情况下也能正常访问；
    2、更快的加载速度，缓存在本地访问速度自然更快；
    3、减轻服务请求压力，文件缓存后不需要再次请求，只需要请求需要更新的文件。


1. 设置当前页面为缓存页面
 	
  <html lang="en" manifest="index.appcache">
      
  注意：
	  1. manifest 对应的文件后缀名必须是.appcache
	  2. 在Apache中添加“AddType text/cache-manifest  .appcache”


 2. 配置缓存文件： 例如 index.appcache
	
	  CACHE MANIFEST    - 在此标题下列出的文件将进行缓存
	  CACHE:		   -要具体缓存的文件有哪些
		./img1.jpg
		./img2.jpg
		./1.css
        ...
       
      NETWORK: 		在此标题下列出的文件需要与服务器的连接，且不会被缓存
      	 *
         ./1.js
         ./2.html
         
       FALLBACK:	 当如果访问的资源由于网络的原因无法正常打开，那么需要找其他替换文件代替显示
        ./1.css  ./2.css
        ./inex.htm   ./404.html
```

## canvas

### 什么是canvas

```js
	 是HTML5中用来展示图片的一个标签

     <canvas></canvas>

     http://echarts.baidu.com/       统计图

     img标签的区别

         img标签只是用来展示图片

         canvas可以自定义图片或者绘制图片，并且具有用户交互
```

### 课程的目标

```js
凡是能够看到的图形，都可以利用canvas来展示

    例如：  2d图形， 3d动画，视频，游戏（水果忍者）等等。。。。


            https://threejs.org/examples/     3d


            获取3d绘图工具
            var ctx1=canvas.getContext("webgl");



            统计图：http://echarts.baidu.com/  2d统计图

            http://www.html5tricks.com/demo/html5-canvas-firestorm/index.html
            
            切水果
            http://www.html5tricks.com/demo/html5-fruit-ninja/index.html

    备注：
        1.具备很强的交互性
        2.基于浏览器的图形引擎
```

### canvas基本使用(代码1)

```js
 1. 过程： 先找落笔点  ------> 结束点 -----> 描点连线



    -> 1. 在页面中放置一个canvas标签

       2. 通过canvas.getContext("2d");    获取到绘制2d图形的上下文（绘图工具）

          通过canvas.getContext("webgl"); 获取到绘制3d图形的上下文（绘图工具）
      
        

    -> canvas绘图的基本思路 （代码2）

         1. 先落笔（找点）            

         2. 移动方向和距离（位置）

         3. 描点连线
    
    -> 方法：

         ctx.moveTo(x,y)  绘图的起始位置（落笔点）    x,y代表横纵坐标

         ctx.lineTo(x,y)  绘制到点     x,y 绘制到点的坐标

         ctx.stroke()     描点连线

         参考：代码1演示绘制一条直线

         备注：
           ctx指的是绘图上下文对象（绘图工具）

    -> 关于canvas标签大小的设置

          1. canvas 默认大小是一个 300*150的标签

          2. 如果改变canvas标签的大小要使用标签属性 width 和height设置

          3. 不推荐使用css设置

          总结：
             1.如果我们使用属性设置大小，那么浏览器会按 
               照属性大小重新分配canvas标签的大小（绘图区域的实际大小）

            
             2.如果通过css样式设置canvas标签的大小，那么其实浏览器自是将标签的显示范围改成设置大小，而canvas标签实际的绘制区域大小还是300*150，导致图像会拉伸


    -> 关于坐标系的介绍
        
        与css中的坐标系一样。（绘图演示）

        原点位置：在画布的左上角

             X轴：水平从左向右为正

             Y轴：垂直从上向下为正
      

        练习：（课堂案例01）

           1. 从200,100的位置绘制宽为200高为150的矩形
           2. 准备一个600*400的画布，三等分这个画布，分别绘制正方形。直角三角形，梯形
              
    -> 绘制过程中的问题
        
          1. 最开始绘制的图像的线颜色会深一点？

          总结：
               由于在canvas中绘图是采取的是单图层绘图，在调用stroke方法的时候其实是在重复的描边，所以最开始的颜色会深一点


          2. 解决方式：

               -> 不要在绘制过程中重复使用stroke,只在图形结束后，进行一次描边即可

               -> 重新开启一个图层（例如重新打开一个画板）。在启用canvas的过程中默认会开启一个图层。如果需要开启新的图层使用 ctx.beginPath()

                  注意：如果在开启新图层后，前面的图形绘制如果没有描边，那么会没效果
```

### 绘制带样式的效果

```
  ☞   线颜色
  	strokeStyle="颜色值";  
  	
  ☞  设置线宽
   	linewidth="值";
  
  ☞  设置线的连接方式
      lineJoin: round | bevel | miter (默认)
      
  ☞  线帽（线两端的结束方式）
  	   lineCap: butt(默认值) | round | square 
  	   
  ☞ 闭合路径： ctx.closePath()
```

### 渐变方案

```js
->渐变：

      渐变（gradient）是指从一种颜色向另一种颜色变化的填充，在颜色相交的地方做融合。
      在Canvas中可以创建两种类型的渐变：线性的和径向的

    -> 线性渐变   
      var grd=ctx.createLinearGradient(x0,y0,x1,y1);

          x0-->渐变开始的x坐标
          y0-->渐变开始的y坐标
          x1-->渐变结束的x坐标
          y1-->渐变结束的y坐标

      grd.addColorStop(0,"black");      设置渐变的开始颜色
      grd.addColorStop(0.1,"yellow");   设置渐变的中间颜色
      grd.addColorStop(1,"red");        设置渐变的结束颜色

      ctx.strokeStyle=grd;
      ctx.stroke();

      备注：
         addColorStop(offse,color);
         中渐变的开始位置和结束位置介于0-1之间，0代表开始，1代表结束。中间可以设置任何小数

    -> 径向渐变
    
      ctx.createradialGradient(x0,y0,r0,x1,y1,r1);

            (x0,y0)：渐变的开始圆的 x,y 坐标

            r0：开始圆的半径

            (x1,y1)：渐变的结束圆的 x,y 坐标

            r1：结束圆的半径
```

### 填充效果

```
 	1. 绘制一个封闭的图形，如果要设置填充的效果，使用ctx.fll();
  
    2. 填充的样式通过 fillstyle设置
```

### 绘制虚线

```js
  原理：

     设置虚线其实就是设置实线与空白部分直接的距离,利用数组描述其中的关系

     例如： [10,10]  实线部分10px 空白部分10px

     例如： [10,5]  实线部分10px 空白部分5px

     例如： [10,5,20]  实线部分10px  空白5px  实线20px  空白部分10px 实线5px 空白20px....

    绘制：
     ctx.setLineDash(数组);
     ctx.stroke();

    注意：
        如果要将虚线改为实线，只要将数组改为空数组即可。
```

### 绘制网格

```
1 canvas 画布的高度   canvas.height

2 canvas 画布的宽度   canvas.width
```

### 绘制网格中的点

### 绘制网格连线

### 完成随机折线图







