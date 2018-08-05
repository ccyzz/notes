# jQuery 基础

### 主要内容

1. 样式操作的方法
2. 属性操作的方法
3. 处理内部文本
4. 处理内部标签




### 重难点

1. 能处理行内样式
2. 能处理类样式
3. 带有函数参数的 css 方法**[ 难点 ]**
4. 能处理自定义属性
5. 能处理标准属性
6. 带有函数参数的属性
7. 能设置内部文本与标签





## 1. 样式操作

前面我们已经介绍了样式的 基本操作. 但是这还不够, 存在一定问题:

1. 每次都使用 css 方法, 设置一个样式值. 比较繁琐与麻烦. 是否存在一次性处理多个样式的办法?
2. 我们只介绍了如何处理行内样式, 是否存在类样式?

下面我们就来详细讨论一下 jQuery 中的样式操作.



### 1.1 深入 css 方法的使用

css 方法有很多种用法:

1. 传入一个样式属性名的字符串, 返回第 0 个元素的对应样式值.
2. 传入一个字符串数组, 返回对应样式值构成的对象.
3. 传入一个样式属性名的字符串和对应的样式值, 设置当前所有 DOM 的样式.
4. 传入一个样式属性名的字符串和一个函数, 设置当前所有 DOM 的样式
5. 传入一个键值对, 设置多个样式.





#### 1) 获得多个样式值

语法描述:

```javascript
jQuery 对象.css( styleNames );
```

说明:

- 该方法获得 jQuery 中 **第一个** DOM 对象的样式.
- 该方法可以获得行内样式, 也可以获得计算样式.
- 该方法的参数是一个字符串数组, 其中是需要获得的样式属性名.
- 该方法返回一个对象, 即对应的样式键值对.



#### 2) 设置多个样式

css 是可以一次设置多个样式的. 语法为:

```javascript
jQuery 对象.css( object );
```

说明: 

- 该方法需要一个对象作为参数
  - 对象的键为需要设置的样式名
  - 对应的值即为需要设置的样式值
- 该方法包含隐式迭代, 会对当前存储的所有 DOM 对象进行处理



#### 3) 按照不同要求设置不同元素的多个样式

考虑一个需求:

- 页面中有 7 个 div.
- 需要分别设置每一个 div 背景色不同. 

如何实现呢? 

法为:

```javascript
jQuery 对象.css( '需要设置的样式名', 迭代函数 );
```

说明:

- 迭代, 曾经介绍过, 就是循环. 事实上该方法就是对前面我们介绍的案例的封装.
- 该方法会遍历 jQuery 当前存储的每一个 DOM 元素, 并使用迭代函数来处理每一个 DOM 元素.
- 迭代函数有两个参数, 分别是: 当前 DOM 元素的索引, 以及 当前 DOM 元素对应样式的值( 原始值 ).
- 迭代函数的返回值, 即是需要设置的样式值. 
- 迭代函数中的 this 就是当前 DOM 元素.



参考数据:

```javascript
var colors = [ 'red', 'orange', 'yellow', 'green', 'cyan', 'blue', 'purple' ];
```





### 1.2 类样式方法

jQuery 提供了 4 个方法来操作类样式:

```javascript
jQuery 对象.hasClass( '类样式名' );
jQuery 对象.addClass( '类样式名' );
jQuery 对象.removeClass( '类样式名' );
jQuery 对象.toggleClass( '类样式' );
```

说明:

- hasClass 方法判断当前 DOM 元素中是否含有对应的 类样式名.
  - 值得注意的是, 当前 jQuery 对象中包含多个 DOM
  - 在所有的 DOM 元素中, 只要有一个元素含有对应的类样式, 就返回 true
  - 如果所有 DOM 元素都没有对应类样式, 则返回 false
- addClass 方法用于给当前所有 DOM 对象添加对应的类样式.
  - 还可以使用函数作为参数, 但是这样的用法较少.
- removeClass 方法用于给当前所有 DOM 对象移除指定样式.
  - 还可以使用函数作为参数, 但是这样的用法较少.
- toggleClass 方法用于切换某一个类样式.
  - 调用 toggleClass 方法, 如果元素含有该类样式, 则为其移除该类样式.
  - 如果元素没有对应类样式, 则为其添加对应类样式.



**案例演示:** tab 选项卡

**演示案例:** 开关灯( 简单版 )  

**案例演示:** 重写鼠标高亮 demo









## 2. 属性操作

属性操作的方式与样式操作的方式类似. 下面我们先说明本节的主要内容.

- 操作标准属性( property )
- 操作自定义属性( arrtibute )
- 操作表单标签的 value
- 操作标签的 innerText
- 操作标签的 innerHTML



### 2.1 prop 方法

prop 方法前面已经介绍过其基本使用, 这里我们深入一下它的完整用法.

语法:

```javascript
jQuery 对象.prop( '属性名' );    // 获得对应的属性值, 已介绍过
jQuery 对象.prop( '属性名', '属性值' );    // 设置所有元素的对应属性值. 已介绍过 
jQuery 对象.prop( 键值对 );    // 批量设置属性
jQuery 对象.prop( '属性名', 迭代函数 );    // 根据函数返回值设置对应属性值 
```

说明:

- 属性操作与样式操作中的 css 方法的使用类似.
- 设置单个属性值与获得属性值前面已经介绍过, 这里不再介绍.
- prop 传入键值对的时候, 键名为属性名, 键值对应的就是属性值. 可以给所有 DOM 对象批量设置属性.
- 当传入迭代函数的时候与 css 的用法一样.
  - 函数有两个参数, 一个是当前元素的索引.
  - 另一个是对应属性的原始值.

**案例演示:** 点击按钮,. 批量设置属性. 



### 2.2 attr 方法

attr 方法对应于标准 DOM 方法中的 setAttribute 以及 getAttribute.

jQuery 中 attr 方法专门处理自定义属性. 但是也支持标准属性.

语法:

```javascript
jQuery 对象.attr( '属性名' );	// 获得对应属性值
jQuery 对象.attr( '属性名', '属性值' );    // 设置元素的属性
jQuery 对象.attr( 键值对 );    // 设置多个属性
jQuery 对象.attr( '属性名', 迭代函数 );    // 根据函数返回值设置属性
```

说明:

- 该方法的使用与 prop 的使用一模一样

下面我们就迭代函数的用法举例说明. 给每一个表格行中添加一个编号的自定义属性.

参考代码:

```javascript
$( function () {
    $( 'table tbody tr' ).attr( 'data-id', function ( i ) {
        return i;
    } );
} );
```



### 2.3 val 方法

设置表单属性的时候, 常常会对 value 属性进行操作, 因此 jQuery 提供了一个快捷的方法来操作 value

语法:

```javascript
jQuery 对象.val( '属性值' );		// 用于设置 value 属性
jQuery 对象.val();			  // 获得 value 属性的值
```



**案例演示:** 设置表单数据

参考代码:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        .txt, .select {
            margin: 20px;
        }
    </style>
    <script src="../jquery/jquery-3.3.1.js"></script>
    <script>
        $( function () {
            $( '.txt button' ).click( function () {
                
                $( '.txt input' ).val( '测试数据' );

            } );

            $( '.select button' ).click( function () {

                $( '.select select' ).val( 6 );

            } );

        } );
    </script>
</head>
<body>
    <div class="txt">
        <button>点击按钮, 设置文本框的值</button><br>
        <input type="text"><br>
    </div>
    <div class="select">
        <button>点击按钮设置下拉框</button><br>
        <select>
            <option value="1">html 基础</option>
            <option value="2">css 基础</option>
            <option value="3">javascript 基础</option>
            <option value="4">web api</option>
            <option value="5">javascript 高级与 oop</option>
            <option value="6">jQuery</option>
        </select>
    </div>
</body>
</html>
```



## 2.4 html 与 text 方法

对应于 DOM 中的 innerHTML 和 innerText, jQuery 也提供了相应的方法.

```javascript
jQuery 对象.html();		// 获得标签中的 innerHTML 的内容
jQuery 对象.html( 'html 字符串' );  // 设置所有对应 DOM 的 innerHTML
jQuery 对象.text();  		// 获得标签中的 innerText 的内容
jQuery 对象.text( '字符串' );	// 设置所有对应 DOM 的 innerText
```



**案例演示:** 动漫相册

演示效果:

![1524584845545](assets/1524584845545.png)










## 小结



















