# jQuery 基础

### 主要内容

1. 实现动画
2. 操作 DOM




### 重难点

1. 自定义动画函数**[ 难点 ]**
2. DOM 元素的添加和创建**[ 难点 ]**



## 1. 动画

jQuery 帮我们封装了非常强大的动画方法, 本节我们需要掌握的有

1. 显示与隐藏
2. 卷帘动画
3. 淡入淡出动画
4. 自定义动画



### 1.1 显示与隐藏

语法:

```javascript
jQuery 对象.show( [speed] );
jQuery 对象.hide( [speed] );
```

说明:

- show 方法表示将当前元素显示出来.
- hide 方法表示将当前元素隐藏起来.
- show 或 hide 如果不带参数, 表示立即显示与隐藏.
- show 或 hide 如果带有参数, 表示以特定的时长, 以动画的方式进行显示与隐藏.
- show 与 hide 支持两种参数
  - 数字, 表示动画播放的总时长. 单位毫秒.
  - 默认的速度字符串
    - `'faster'` 以 200 毫秒运行动画
    - `'slow'` 以 600 毫秒运行动画
    - 默认是 400 毫秒
- show 与 hide 在播放动画的时候, 总是从左上角显示或消失.

**案例与演示:** 点击按钮, 控制 div 的显示与隐藏.

演示效果:

![](./assets/showhide.gif)

参考代码:

```javascript
$( function () {
    $( '#btn1' ).click( function () {
        var speed = $( '#opt' ).val();
        $( '#box' ).show( speed === 'noanimal' ? undefined : speed );
    } );
    $( '#btn2' ).click( function () {
        var speed = $( '#opt' ).val();
        $( '#box' ).hide( speed === 'noanimal' ? undefined : speed );
    } );
} );
```



### 1.2 卷帘动画

语法:

```javascript
jQuery 对象.slideDown( [speed] );
jQuery 对象.slideUp( [speed] );
```

说明:

- 卷帘动画类似于卷闸门, 向上收起则消失, 向下打开则显示.
- 这两个方法可以带有控制速度的参数
  - 默认无参数是 400 毫秒播放完动画
  - 可以使用数字来控制动画的持续时间.
  - 可以使用默认的预制速度字符串: fast 与 slow

**案例演示:** 菜单案例

演示效果:

![](./assets/slideDown_slideUp.gif)

参考代码:

```javascript
$( function () {

    $( '#btn1' ).click( function () {
        var speed = $( '#txt' ).val().trim();
        if ( speed.length === 0 ) {
            speed = undefined;
        } else if ( !isNaN( speed - 0 )  ) {
            speed -= 0;
        }

        $( '#box' ).slideDown( speed );
    } );
    $( '#btn2' ).click( function () {
        var speed = $( '#txt' ).val().trim();
        if ( speed.length === 0 ) {
            speed = undefined;
        } else if ( !isNaN( speed - 0 )  ) {
            speed -= 0;
        }

        $( '#box' ).slideUp( speed );
    } );
} );
```



 



### 1.3 淡入淡出动画

语法:

```javascript
jQuery 对象.fadeIn( [speed] );
jQUery 对象.fadeOut( [speed] );
```

说明:

- 该方法的参数与 slide 系列方法一样.
- fadeIn 为淡入, 慢慢从无更改透明度到 1, 从而显示.
- fadeOut 为淡出, 慢慢调整透明度为 0, 从而消失.

**演示案例:** 控制 div 的显示与隐藏

效果展示:

![](./assets/fadeIn_fadeOut.gif)

参考代码:

```javascript
$( function () {

    $( '#btn1' ).click( function () {
        var speed = $( '#txt' ).val().trim();
        if ( speed.length === 0 ) {
            speed = undefined;
        } else if ( !isNaN( speed - 0 )  ) {
            speed -= 0;
        }

        $( '#box' ).fadeIn( speed );
    } );
    $( '#btn2' ).click( function () {
        var speed = $( '#txt' ).val().trim();
        if ( speed.length === 0 ) {
            speed = undefined;
        } else if ( !isNaN( speed - 0 )  ) {
            speed -= 0;
        }

        $( '#box' ).fadeOut( speed );
    } );
} );
```



### 1.4 自定义动画

jQuery 还提供了强大的自定义动画. 语法如下:

```javascript
jQuery 对象.animate( styles[, 持续时间][, 完成后触发函数] );
```

说明:

- animate 方法的用法很多, 这里介绍最常见的. 详细用法可以[参考官方文档](http://api.jquery.com/animate/).
- 参数 styles 是一个键值对, 表示动画完成后需要设置成的样式.
- 持续时间可选, 默认是 400 毫秒.
- 完成后触发函数可选, 表示动画结束后执行的函数.



**演示案例:** 移动与变化的盒子

演示效果:

![](./assets/animate.gif)



参考代码:

```javascript
$( function () {

    $( 'button' ).click( function () {
        $( '#box1' ).animate( { 
            left: '400px', 
        } );

        $( '#box2' ).animate( { 
            top: '400px', 
            width: '300px', 
            height: '200px',
        } );
    } );

} );
```









## 2. DOM 操作

DOM 操作是 Web 页面中非常中要的一部分. 本节我们的重点是

1. 创建 DOM 元素, 并实现追加
2. 移除指定元素
3. 克隆指定元素



### 2.1 创建 新的 jQuery 对象

语法:

```javascript
$( 'HTML 格式的字符串' )
```



### 2.2 添加 DOM 元素

#### 2.2.1 追加元素

语法:

```javascript
jQuery 对象.appendTo( target );
jQuery 对象.append( target );
jQuery 对象.prependTo( target );
jQuery 对象.prepend( target );
```



#### 2.2.2 插入元素

语法:

```javascript
jQuery 对象.insertBefore( target );
jQuery 对象.before( target );
jQuery 对象.insertAfter( target );
jQuery 对象.after( target );
```





### 2.3 移除 DOM 元素

语法:

```javascript
jQuery 对象.remove();
```





### 2.4 克隆

语法:

```javascript
jQuery 对象.clone();
```


















## 小结



















