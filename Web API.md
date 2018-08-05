---
 学习目标:
  - 掌握API和Web API的概念
  - 掌握常见浏览器提供的API调用方式
  - 能通过API开发常见的页面交互功能
typora-copy-images-to: media
---

# Web API

## Web API介绍

### API概念

API（Application Programming Interface,应用程序编程接口）。是预先定义好函数或者方法，目的是实现某些特定功能。

- 任何开发语言都有自己的API
- 开发人员不需要关注内部实现细节
- API的特征输入和输出(I/O)
  -  Math.max(10,20,30)

### Web API概念 

web在不同语境下有不同意义，最常用的意义是指通过浏览器访问互联网上的资源就是web。

操作浏览器和页面元素的API(BOM和DOM)


## JavaScript的组成

![1496912475691](media/1496912475691.png)

### ECMAScript - 语法规范 

定义了JavaScript的语法规范，描述了语言的基本语法和数据类型。

ECMAScript是一套标准，定义了一种语言的标准与具体实现无关

### DOM - 文档对象模型

一套操作页面元素的API

DOM可以对页面中的元素进行添加、删除、修改、更新的操作。

### BOM - 浏览器对象模型

一套操作浏览器的API

通过它可以操作浏览器窗口，比如：弹出框、控制浏览器跳转、获取分辨率等

## DOM

学了DOM能做什么?

### DOM的概念

文档对象模型（Document Object Model）

文档就是存储内容的档案，文档对象模型中的文指**HTML文档**，文档对象模型就是将HTML文档抽象成了对象，文档中的元素（标签）也被抽象成对象，对象下面提供了很多属性或者方法用来操作HTML文档中的元素（标签）。

文档对象模型就是一组操作页面元素（标签）的API。

### 文档树

![1497154623955](media/01.gif)

- 文档（Document）：文档对象。
- 节点（Node）：页面中的所有内容都可以叫做节点，比如标签、属性、文本、注释。
- 元素（Element）：页面中的所有标签都可以叫做元素（在HTML里面叫做标签，DOM中叫做元素）。

## 获取页面元素

### 为什么要获取页面元素

若想控制页面中的元素，先要通过特定的DOM API获取这个元素。

### 根据id获取元素

```javascript
// 在文档的范围内获取id值为main的元素
var oDiv = document.getElementById('main');
console.log(oDiv);
```

 注意：通过id一次只能获取一个元素。

### 根据标签名获取元素

```javascript
// 在文档的范围内获取标签名字为div的元素
var aDivs = document.getElementsByTagName('div');
for (var i = 0; i < aDivs.length; i++) {
  var div = aDivs[i];
  console.log(div);
}
```

注意：**通过标签名字获取的是一组元素**，哪怕页面中只有一个元素，得到的结果仍然是一组元素。

思考：页面中有两对ul标签，只获取第一对ul标签中的li标签，怎么做？

## 事件

### 什么是事件？

- 浏览网站的人和网站之间交互的方式就是事件。
- JavaScript中的事件就是"当...时候做...事情"。当点击按钮的时候，用弹出框跟用户打个招呼。


### 事件的基本使用

需求：点击按钮弹出提示框，跟用户打个招呼

```html
<input type="button" value="按钮" id="btn">
<script>
 	var oBtn = document.getElementById("btn");
    oBtn.onclick = function(){
        alert("你好");
    }
</script>
```
### 其他添加事件方式（了解）

- 方式一

  ```html
  <input type="button" value="按钮" onclick="alert('你好')">
  ```

- 方式二

  ```html
  <input type="button" value="按钮" onclick="clickFn()">
  <script>
  function clickFn () {
  	alert("你好");
  }
  </script>
  ```

- 方式三

  ```html
  <input type="button" value="按钮" id="btn">
  <script>
  function clickFn() {
  	alert("你好");
  }

  var oBtn = document.getElementById("btn");
  oBtn.onclick = clickFn;
  </script>
  ```

### 移除元素的事件处理函数

```javascript
元素对象.事件名称 = function () {}
元素对象.事件名称 = null;
```

### 案例

- [为多个按钮注册点击事件，弹出内容](code/1.为多个按钮注册点击事件，弹出内容.html)

### 注意

元素对象身上天生就有各种事件，以属性的方式存在，默认值是null。

### 事件三要素

- 事件源:触发事件的元素
- 事件类型:事件的触发方式(例如鼠标点击或键盘点击)
- 事件处理程序:事件触发后要执行的代码(函数形式)

## 元素属性操作

元素对象的属性，一般对应元素(标签)的属性（DOM对象其实就是对标签的封装）

- 获取属性值：元素.属性名称
- 设置属性值：元素.属性名称 = 属性值

### 非表单元素属性

| 属性名称  | 含义                |
| ----- | ----------------- |
| id    | 元素的唯一标识           |
| href  | 链接的跳转地址           |
| src   | 图片或者script标签的资源路径 |
| title | 鼠标放到图片上显示的提示文字    |
| alt   | 图片加载失败时显示的文字      |

```html
<a href="http://www.baidu.com" id="link">百度</a>
<img src="images/demo.jpg" title="鼠标放到图片上显示的提示文字" alt="图片加载失败时显示的文字"/>
```

#### 案例

- [点击按钮，获取图片属性](code/5.点击按钮，切换img标签里的图片并设置图片的宽高，修改图片的alt和title属性.html)
- [点击按钮，切换img标签里的图片并设置图片的宽高，修改图片的alt和title属性](code/5.点击按钮，切换img标签里的图片并设置图片的宽高，修改图片的alt和title属性.html)
- [点击按钮，更改链接的文字和地址](code/6.点击按钮，更改链接的文字和地址.html)
- [点击按钮修改按钮的属性及内容](code/13.点击按钮修改按钮的属性及内容.html)
  - 事件处理函数中的this默认指向发生事件的元素
  - 当发生事件的时候 想改变发生事件的元素身上的属性或者样式，在函数内部就可以使用this

#### 美女相册

- [阻止a标签的默认行为](code/17.阻止a标签的默认行为.html)
- [给多个超链接注册点击事件并获取点击元素的属性值](code/18.给多个超链接注册点击事件.html)
- [美女相册](code/15.美女相册.html)

### 表单元素属性

| 属性名称     | 含义                       |
| -------- | ------------------------ |
| value    | 用于大部分表单元素的内容获取(option除外) |
| type     | 可以获取input标签的类型(输入框或复选框等) |
| disabled | 禁用属性                     |
| checked  | 复选框选中属性                  |
| selected | 下拉菜单选中属性                 |

#### 案例

- [获取文本框的值，给文本框赋值](code/7.给文本框赋值，获取文本框的值.html)
- [点击按钮修改多个文本框的值](code/8.点击按钮修改多个文本框的值.html)
- [点击按钮选择性别](code/9.点击按钮选择性别.html)
- [点击按钮选择兴趣](code/10.点击按钮选择兴趣.html)
- [点击按钮禁用文本框](code/11.点击按钮禁用文本框.html)
- [设置下拉框中的选中项](code/12.设置下拉框中的选中项.html)
- [搜索文本框文字显示隐藏效果](code/14.搜索文本框文字效果.html)
- [全选反选](code/16.全选反选.html)

### 自定义属性操作

在html中是允许开发人员自定义属性的，那么在js中是如果操作自定义属性的呢？

```html
<span id="" class="" index="" abc=""></span>
```

| 获取                    | 设置                        | 删除                       |
| --------------------- | ------------------------- | ------------------------ |
| 元素.getAttribute(属性名称) | 元素.setAttribute(属性名称,属性值) | 元素.removeAttribute(属性名称) |

#### getAttribute()和元素.属性名的区别

**在设置自定义属性的时候**

- 元素对象.setAttribute()  **是给元素（标签）设置属性**
- 元素对象.自定义属性名称 = 属性值   **是给元素对象设置属性**
- 一个设置在了**元素（标签）**上   一个设置在了**元素对象**上

在给元素设置 [自定义属性] 的时候 要用 [setAttribute] 系列
在给元素设置 [原有属性] 的时候  用 [ 元素.属性名称 = 属性值 ] 的方式 和 [ setAttribute ] 系列都可以

### 元素内容操作

| innerText         | innerHTML         |
| ----------------- | ----------------- |
| 元素.innerText      | 元素.innerHTML      |
| 元素.innerText = 内容 | 元素.innerHTML = 内容 |

#### innerText、innerHTML区别

| innerText  | **innerHTML** |
| ---------- | ------------- |
| 只获取文本内容    | 获取所有内容        |
| 不会解析内容中的标签 | 解析内容中的标签      |

- 获取内容

  ```html
  <div id="box">
      <p>啦啦啦</p>
  </div>
  <script>
  	var oBox = document.getElementById("box");
      console.log(oBox.innerText); // 啦啦啦
      console.log(oBox.innerHTML); // <p>啦啦啦</p>
  </script>
  ```

- 设置内容

  ```html
  <div id="box"></div>
  <script>
  	var oBox = document.getElementById('box');
  	oBox.innerHTML = '我是文本<p>我会生成为标签</p>';
  	oBox.innerText = '我是文本<p>我不会生成为标签</p>';
  </script>
  ```

#### 案例

- [点击按钮修改p标签中的内容](code/2.点击按钮修改p标签中的内容.html)
- [点击按钮修改多个p标签的文字内容](code/3.点击按钮修改多个p标签的文字内容.html)
- [点击按钮将div中的内容放置在p标签中](code/4.点击按钮将div中的内容放置在p标签中.html)

#### textContent

| 获取内容           | 设置内容                |
| -------------- | ------------------- |
| 元素.textContent | 元素.textContent = 内容 |

#### textContent与innerText的区别

| innerText    | textContent  |
| ------------ | ------------ |
| 不会获取隐藏元素中的内容 | 会获取到隐藏元素中的内容 |

```html
<div id="box">
    11111
    <div style="display:none">22222</div>
</div>
<script>
    var oBox = document.getElementById("box");
    console.log(oBox.innerText); // 1111
    console.log(oBox.textContent); // 1111 2222
</script>
```

注意：此方法也可以设置获取元素的内容，相当于innerText，但是该方法IE8以下不支持，**不推荐使用**。

## 元素样式操作

### 元素style属性

元素对象.style.样式名称 = "样式值"

- 设置的样式显示在标签行内
- 通过style属性设置元素素的宽高、位置 类型是字符串，需要加上px

```html
<div id="box"></div>
<script>
	var oBox = document.getElementById("box");
    oBox.style.width = "200px";
    oBox.style.height = "200px";
    oBox.style.backgroundColor = "red";
</script>
```

### 元素className属性

元素.className = "类名"

```javascript
var oBox = document.getElementById("box");
oBox.className = 'clearfix';
```

### 案例

- [点击按钮让元素显示隐藏（两个按钮 style）](code/21.点击按钮让元素显示隐藏（两个按钮）.html)
- [点击按钮让元素显示隐藏（一个按钮 className）](code/22.点击按钮让元素显示隐藏（一个按钮）.html)
- [图片切换二维码案例](code/23.图片切换二维码案例.html)
- [当前输入的文本框高亮显示](code/24.当前输入的文本框高亮显示.html)
- [列表隔行变色、高亮显示](code/26.列表隔行变色、高亮显示.html)


- [按钮的排他功能 要求是点击哪个按钮让哪个按钮显示"年薪百万"，让其他按钮显示"回家种地"](code/29.按钮的排他功能.html)


- [选项卡切换效果](code/30.选项卡切换效果.html)


### 练习

- [点击按钮变色](code/20.%E7%82%B9%E5%87%BB%E6%8C%89%E9%92%AE%E5%8F%98%E8%89%B2.html)

- [点击按钮改变div的大小和位置](code/25.点击按钮改变div的大小和位置.html)

  ```javascript
  box.style.width = '200px';
  box.style.height = '200px';
  box.style.backgroundColor = 'red';
  box.style.position = 'absolute';
  box.style.left = '20px';
  box.style.top = '10px';
  ```

  ​

- [鼠标移入元素边框高亮效果](code/28.鼠标移入元素边框高亮效果.html)

  ```javascript
  box.onmouseover = function () {
      this.style.border = '20px solid red';
  }
  ```

- [检测用户名是否是3-6位，如果不满足要求高亮显示文本框](code/27.检测用户名是否是3-6位，如果不满足要求高亮显示文本框.html)

## 节点

节点可以使开发人员更加方便容易的选择到页面中的元素。

### 节点概念

节点（node）：页面中所有的内容都叫做节点，包括元素节点，属性节点，文本节点。

```html
<div>
	<p id="paragraph">css世界</p>
</div>
<ul id="list">
    <li>JavaScript高级程序设计</li>
    <li>JavaScript权威指南</li>
</ul>
<p>HTML权威指南</p>
```

### 节点关系

| 父级节点         | parentNode                 |
| ------------ | -------------------------- |
| 子级节点         | childNodes                 |
| **子级元素**     | **children**               |
| 第一个子级节点      | firstChild                 |
| **第一个子级元素**  | **firstElementChild**      |
| 最后一个子级节点     | lastChild                  |
| **最后一个子级元素** | **lastElementChild**       |
| 上一个兄弟节点      | previousSibling            |
| **上一个兄弟元素**  | **previousElementSibling** |
| 下一个兄弟节点      | nextSibling                |
| **下一个兄弟元素**  | **nextElementSibling**     |

#### childNodes和children的区别

| 节点集合childNodes | children元素集合 |
| -------------- | ------------ |
| 包含文本节点和元素节点    | 只包含元素节点      |

```html
<!--
	parentNode
	children
	children[0]
	children[children.length-1]
	previousElementSibling
	nextElementSibling
-->
```

### 节点属性

| 节点类型 nodeType | 对应值  |
| ------------- | ---- |
| 元素节点          | 1    |
| 属性节点          | 2    |
| 文本节点          | 3    |

| 节点名称 nodeName | 对应值        |
| ------------- | ---------- |
| **标签节点**      | **大写标签名字** |
| 属性节点          | 小写属性名字     |
| 文本节点          | #text      |

备注：通过getAttributeNode方法获取属性节点

### 案例

- [通过子节点方式为li元素添加背景颜色](code/50.使用子节点为元素添加背景颜色.html)
- [通过子节点方式为p元素设置背景颜色](code/31.通过节点方式更换p元素的背景颜色.html)
- [通过节点方式实现隔行变色](code/32.通过节点方式实现隔行变色.html)(练习)
- [点击小图更换大图](code/33.点击小图更换大图.html)


### 创建节点

**元素对象.innerHTML = 内容**
- 每次操作都会覆盖上次内容，若想追加则将 = 改成 +=

**var 元素对象 = document.createElement('标签名称')**

#### 案例 

- [动态创建列表](code/34.创建列表字符串版.html)



- [根据数据动态创建表格](code/36.根据数据动态创建表格.html)
- [模拟百度搜索框](code/37.模拟百度搜索.html)


### 节点操作

| 追加节点 | 父级元素. appendChild(元素对象)      |
| ---- | ---------------------------- |
| 插入节点 | 父级元素.insertBefore(插入谁,插在哪)   |
| 删除节点 | 父级元素.removeChild(要删除的元素)     |
| 替换节点 | 父级元素.replaceChild(新元素,被替换元素) |

注意：appendChild除了追加节点以外还可以**移动节点**。

[节点操作练习](code/54.节点操作练习.html)

## 事件详解

### 添加移除事件

元素对象.事件名称 = 事件处理函数;  

问题：不能为同一个元素的同一个事件绑定多个事件处理函数。

场景：为同一个元素的同一个事件绑定多个事件处理函数的场景

1. 元素在发生事件的时候要执行多件事，每一件事情之间没有任何关系。
2. 一个页面多个人开发，多个人都要为同一个元素绑定事件。

#### 标准浏览器（掌握）

| 添加事件 | 元素.addEventListener(事件名称，事件处理函数，捕获/冒泡)   |
| ---- | ---------------------------------------- |
| 移除事件 | 元素.removeEventListener(事件名称，事件处理函数，冒泡/捕获) |

#### IE低版本（了解）

| 添加事件 | 元素.attachEvent(事件名称，事件处理函数) |
| ---- | --------------------------- |
| 移除事件 | 元素.detachEvent(事件名称，事件处理函数) |

```javascript
function addEventListener(element, type, fn) {
  if (element.addEventListener) {
    element.addEventListener(type, fn, false);
  } else {
    element.attachEvent('on' + type,fn);
  }
}

function removeEventListener(element, type, fn) {
  if (element.removeEventListener) {
    element.removeEventListener(type, fn, false);
  } else {
    element.detachEvent('on' + type, fn);
  }
}
```

#### addEventListener、attachEvent区别

| addEventListener | attachEvent   |
| ---------------- | ------------- |
| 事件名称不加 on        | 事件名称加 on      |
| 事件处理函数是先绑定先执行    | 事件处理函数是先绑定后执行 |
| 三个参数             | 两个参数          |

### 事件对象

#### 事件对象概念

当事件发生的时候，和事件相关的信息都被保存在了这个对象中，比如发生了什么事件，谁触发了这个事件，事件发生的时候鼠标距离页面x轴的位置，y轴的位置......

只要发生事件，就有事件对象，无论是用哪一种方式绑定的事件。

#### 获取事件对象

事件处理函数中的第一个参数即是事件对象。

#### 事件对象常用属性

| 属性名称                  | 含义                 |
| --------------------- | ------------------ |
| pageX / pageY         | 鼠标相对于页面左上顶点的位置     |
| clientX / clientY（了解） | 鼠标相对于浏览器可视区左上顶点的位置 |
| target                | 事件源 发生事件的元素        |
| e.preventDefault()    | 阻止事件默认行为           |

#### 案例

1. [跟着鼠标飞的天使](code/38.跟着鼠标飞的天使.html)

### 事件冒泡（掌握）

#### 概念

当一个元素接收到事件的时候，元素会把他接收到的事件传给自己的**父级**，触发父级的相同事件，一直到window 。

```html
<div id="div1">
    <div id="div2">
        <div id="div3"></div>
    </div>
</div>
```

注意：

1. 事件冒泡只和元素的父子嵌套结构有关，和样式没有关系。
2. 如果父级没有绑定事件函数也会传递，只是不会有什么表现。

![02.png](media/02.png)

#### 事件委托

使用场景

1. 父子结构，当子元素的数量非常多时，为每一个子元素都绑定事件会影响程序的执行效率。
2. 当通过节点创建的方式向页面中追加元素时，是没有事件的。

#### 阻止事件冒泡

e.stopPropagation()

### 事件捕获（了解）

和冒泡的执行顺序相反，冒泡是从内向外，捕获是从外向内。

## BOM

### BOM的概念

BOM(Browser Object Model)  浏览器对象模型 一组操作浏览器的API

常见浏览器操作包含：刷新浏览器、后退、前进、获取地址栏中的url，控制浏览器的滚动条等。

### BOM对象window

window对象表示浏览器窗口，是浏览器中的顶级对象。

- document是window对象的属性
- 全局变量是window对象的属性
- 全局函数是window对象的方法
- alert()
- prompt()
- confirm()
  ```javascript
  var num = 20;
  console.log(window.num); // 相当于给window对象添加了一个属性num,值是20

  function fn () {
      console.log("fn函数相当于是window对象下面的一个方法");
  }

  // 在平时使用的使用 window对象可以省略不写
  window.fn();

  window.innerHeight  // 浏览器窗口的高度
  window.innerWidth  // 浏览器窗口的宽度

  window.open('网址') // 打开新的浏览器窗口
  window.close() // 关闭当前浏览器窗口
  ```

注意：当调用window对象下的属性和方法时，可以省略window。

### 页面加载完成事件

```html
<script>
    // html页面中的代码都从上向下执行的
    // 当这行js代码执行的时候，按钮标签还没有被加载到页面中，所以js没有获取到按钮标签
    // 那么oBtn的值将会是null，null代表没有对象
    // null.onclick 获取null下面的属性将会报错
    var oBtn = document.getElementById("btn");
    oBtn.onclick = function(){
        alert("你好");
    }
</script>
<input type="button" id="btn">
```

```html
<script>
    // 当页面加载完成时，执行事件所对应的事件处理函数
    // 此时HTML标签都已经加载到页面中，下面代码就不会报错了
	window.onload = function(){
        var oBtn = document.getElementById("btn");
        oBtn.onclick = function(){
            alert("你好");
        }
    }
</script>
<input type="button" id="btn">
```
### location对象

location对象是window对象下的一个属性，包含当前URL 的信息

location可以获取或者设置浏览器地址栏的URL

| location.href               | 获取当前页面的url                  |
| --------------------------- | --------------------------- |
| location.href = 地址          | 跳转到目标链接地址 产生历史记录            |
| location.replace(地址)        | 跳转到目标链接地址 不会产生历史记录          |
| location.reload(true/false) | 重新加载当前页面 true强制刷新 false正常刷新 |
| location.search             | 获取查询字符串                     |

#### location有哪些成员？

- 使用chrome的控制台查看

- [查MDN](https://developer.mozilla.org/zh-CN/)


#### 案例

获取到地址栏中问号后面的部分，并处理成对象的格式

```javascript
// ?name=zhangsan&age=20 => {name:'zhangsan',age:'20'}
var query = location.search;

// name=zhangsan&age=20
query = query.slice(1);

// ['name=zhangsan', 'age=20']
var ary = query.split('&');

// "name=zhangsan"    ['name', 'zhangsan']
var ary1 = ary[0].split('=');

// 'age=20'  ['age', '20'];
var ary2 = ary[1].split('=');

var result = {};

// result.name = 'zhangsan' 
result[ary1[0]] = ary1[1];

// result.age = '20'
result[ary2[0]] = ary2[1];

console.log(result);
```

### history历史记录对象

| history.back()    | 后退   |
| ----------------- | ---- |
| history.forward() | 前进   |

### 定时器

每隔一段时间做一些事情。

| 方法名             | 含义    | 特点              |
| --------------- | ----- | --------------- |
| setTimeout()    | 定时器   | 只执行一次           |
| clearTimeout()  | 清除定时器 | 清除setTimeout()  |
| setInterval()   | 定时器   | 重复执行            |
| clearInterval() | 清除定时器 | 清除setInterval() |

#### 案例

1. [协议按钮禁用](code/41.协议按钮禁用.html)
2. [qq头像](code/51.qq头像.html)

## 偏移量

### offset系列

- 元素.offsetParent 获取嵌套层级最近的有定位的父级
  - 定位：相对定位、绝对定位，固定定位
    - relative absolute fixed
  - 如果没有定位父级，则返回body
  - offsetParent和parentNode的区别
- 元素.offsetLeft 获取元素与定位父级在x方向上的距离
- 元素.offsetTop 获取元素与定位父级在y方向上的距离
  - 如果没有定位父级，则返回该元素与body的距离
- 元素.offsetWidth 获取元素的宽度
- 元素.offsetHeight 获取元素的高度
  - 包含边框和内边距

![1498743216279](media/1498743216279.png)

#### 案例

- [拖拽案例](code/42.拖拽.html)
- [弹出登录窗口](code/43.弹出登录窗口.html)
- [跟随鼠标移动的盒子限制范围](code/53.跟随鼠标移动的盒子(放大镜案例前奏).html)
- [放大镜](code/44.放大镜.html)

### scroll系列

- scrollWidth 元素实际内容宽度
- scrollHeight 元素实际内容高度
  - 如果内容宽度小于盒子宽度 得到的值实际上是盒子宽度
  - 如果内容高度小于盒子高度 得到的值实际上是盒子高度
- scrollLeft 获取滚动条向左滚动的距离
- scrollTop 获取滚动条向上滚动的距离
  - 获取浏览器滚动条滚动的高度

![1498743288621](media/1498743288621.png)

#### 案例 

- [回到顶部](code/49.回到顶部.html)
- [模拟滚动条](code/45.模拟滚动条.html)


### client系列

- 元素.clientLeft 获取元素左边框的宽度
- 元素.clientTop 获取元素上边框的宽度
- 元素.clientWidth 获取元素的宽度 （不包含边框）
- 元素.clientHeight 获取元素的高度（不包含边框）

![1498743269100](./media/1498743269100.png)

### 动画函数

包含匀速动画函数和变速度动画函数

1. 点击按钮让元素动起来
2. 设置目标点让元素停下

3. 解决重复点击定时器多开问题

4. 控制多个元素一起移动(两个按钮 两个元素)

5. 重复代码封装到函数总

6. 解决一个变量控制多个定时器问题

7. 实现变速动画(开始快结束慢)

   1. 关键点是步长 让每一次累加的步长慢慢变小即可
   2. 公式 (目标点 - 当前值) / 减速系数
   3. 当目标点大于当前值 向上取整
   4. 当目标点小于当前值 向下取整
8. 实现元素的任意样式变换(获取元素的任意样式)
9. 实现元素多个任意样式同时变换
10. 实现链式动画
11. 处理特殊值(透明度和层级)


    1. 小数在程序中直接计算不靠谱 所以要转成 整数放大100倍 赋值的时候再缩小100倍
    2. 层级不需要做动画 所以直接赋值即可

#### 获取元素任意样式

- window.getComputedStyle
- 元素.currentStyle

#### 案例

- [无缝轮播图](code/47.无缝轮播图.html)
- [筋斗云案例](code/筋斗云案例/筋斗云案例.html)
- [固定导航栏案例](code/固定导航栏案例/固定导航栏案例.html)
- [开机动画](code/开机动画/开机动画.html)
- [手风琴案例](code/手风琴案例/案例手风琴.html)

## 附录

### 兼容问题

- 事件对象

  ```javascript
  var oEvent = e || window.event;
  ```

- 阻止事件冒泡

  ```javascript
  window.event ? window.event.cancelBubble = true : e.stopPropagation();
  ```

- 阻止默认行为

  ```javascript
  window.event ? window.event.returnValue = false : e.preventDefault();
  ```

- 事件源

  ```javascript
  var element = window.event.srcElement ? window.event.srcElement : event.target; 
  ```

### 事件

### 常用事件

- onmouseover 鼠标移入
- onmouseout 鼠标移出
- onmousedown 鼠标按下
- onmousemove 鼠标移动
- onkeydown 盘键按下
- onkeyup 键盘抬起
- onfocus 获取焦点
- onblur 离开焦点
- onsubmit 表单提交
- onload 加载完成

- [JavaScript事件参考手册](http://www.w3school.com.cn/jsref/jsref_events.asp) 

