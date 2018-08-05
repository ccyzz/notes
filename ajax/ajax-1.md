# Ajax技术-1 #
# 1. 案例---新用户注册时用户名冲突问题 #
##   1.1 传统模式解决方案 ##
1) 在注册页面中输入用户名和密码，点击“注册”时，要将表单数据提交给后台

![1523691565801](assets/1523691565801.png)

2) 后台PHP页面，接收用户名和密码之后，要去验证用户名是否存在，如果存在则提示用户名存在，再跳转回注册页面，重新输入。

  当用户名存在时，提示用户名已被占用，再跳转回登录页

![1523691637131](assets/1523691637131.png)

 当用户名不存在时，正常注册新用户

![1523691696235](assets/1523691696235.png)





##   1.2 ajax解决方案 ##

当光标离开用户名文本框时，就已经验证了用户名是否存在，并给予提示了。

  用户名被占用： 

![1523691892998](assets/1523691892998.png)

  用户名可用 :

![1523691866761](assets/1523691866761.png)



##   1.3 两种模式对比 ##
 传统模式： 

  两个页面 ---  前端注册表单页（index.html）  和  后端数据验证页（index.php）

 ![1524655686232](assets/1524655686232.png)

 Ajax模式 :

  两个页面 --- 前端注册表单页（index.html）  和  后端数据验证页（index.php）

![1524656343611](assets/1524656343611.png)



  Ajax的优势：没有页面跳转，用户体验提升。

​    ajax就是在页面没有刷新或者没有跳转的情况下还能更新页面的某一部分数据



# 2.ajax快速入门 #
##    2.1 ajax概述  ##
- Ajax:  Asynchronous  javascript  and  xml (异步javascript和xml)。

- ==Ajax并不是一种新技术，而是已有技术的集合。JavaScript是核心载体==。
- Ajax优势：在不刷新页面的情况下，更新页面数据，提升用户体验。
- ==Ajax就像一个小秘书==，能够实现异步工作。



##    2.2 发送Ajax请求 ##

###      2.2.1 ajax核心对象---XMLHttpRequest对象 ###

  历史：
	1999年诞生，微软在IE5中集成了XMLHttpRequest对象，但是并没有受到人们重视。
	2005年，google在gtalk即时聊天工具中使用了该对象，从此之后Ajax技术开始受到人们的重视

 创建XMLHTTPRequest对象要分为 IE和==非IE两种方式(主流)==：

   IE浏览器（IE7之前） :  

​                  var xhr = new ActiveXObject('Msxml2.XMLHTTP');

   非IE浏览器（chrome、firefox、opare、safair、搜狗等，包括IE7+之后）:

​		  var xhr = new XMLHttpRequest();



案例：创建XMLhttpRequest对象，兼容IE7之前和主流浏览器

```
function getXhr(){
    var xhr = '';
    try {
        //使用主流浏览器的创建方法 (chrome firefox等)
        xhr = new XMLHttpRequest();
    } catch (e) {
        //e 叫做异常对象，里面会保存异常信息
        //使用IE7以下的创建方法
        xhr = new ActiveXObject('Msxml2.XMLHTTP');
    }
    return xhr;
}
```

访问结果:

![1526091923255](assets/1526091923255.png)

![1526091936917](assets/1526091936917.png)





###    2.2.2 核心方法

   XMLHttpRequest对象有了，可以发送Ajax请求了。发送请求需要两个方法:

   open(var1, var2, var3): 准备Ajax请求
     var1: 请求方式  get/post
     var2: 请求的后端程序地址
     var3: 异步(true)/同步(false)，可选参数，默认为true

   示例: xhr.open(‘get’, ‘./index.php’);   //准备以get方式请求后端的index.php文件

  send(var): 发送Ajax请求
     var: 分为两种情况。 如果是get请求，则填写null。 如果是post请求，则填写要发送到后端的数据。

   示例: xhr.send(null); 



###    2.2.3 发送请求案例

 在index.html页面上创建按钮，点击该按钮时使用get方式请求后端的index.php页面

 发送Ajax请求流程:

   1) 创建XMLHttpRequest对象

   2) 调用open方法准备ajax请求

   3) 调用send方法发送ajax请求

```
<input type="button" value="触发" id="btn">
<script type="text/javascript">
//创建XMLHttpRequest对象，并且处理兼容性
function getXhr () {
    var xhr = null;
    try {
        //主流方法
        xhr = new XMLHttpRequest ();
    } catch (e) {
        xhr = new ActiveXObject ('Msxml2.XMLHTTP');
    }
    return xhr;
}  
//按钮上绑定点击事件，发送ajax请求
document.getElementById('btn').onclick = function(){
    var xmlhttp = getXhr ();
    // 调用open方法准备ajax请求
    xmlhttp.open ('get', 'send.php');

    // 调用send方法发送ajax请求
    xmlhttp.send (null);
}
</script>
```



访问结果: 点击“触发”按钮之后，发送一个新请求 --- send.php

![1526092705580](assets/1526092705580.png)



##    2.3 接收后端响应结果 ##

###      2.3.1 核心属性 --- readyState ###

   Ajax的整个过程有5个状态，对应readyState的5个值：0-4
     0: 创建ajax对象了
     1: 已经调用open方法了
     2: 已经调用send方法了
     3: 服务器端返回了部分数据
     ==4: 服务器数据返回完毕，ajax请求完毕==

###      2.3.2  核心事件 --- onreadystatechange ###
​    onreadystatechange事件:  readyState的值每次发生变化都会触发该事件。
        0-->1    1-->2    2-->3    3-->4 总共触发4次

###     2.3.3  其他重要属性 ---  responseText

​     以字符串形式接收后端程序的返回值。

​     以PHP为例: PHP程序最终会被解释程序一段字符串，responseText接收的就是这段字符串

###      2.3.4 响应案例 --- send.php ###
   send.php：返回字符串“Hello World”给前端

   send.html： 将Hello Wrold 显示在页面的div中

```
//按钮上绑定点击事件，发送ajax请求
document.getElementById('btn').onclick = function(){
    var xmlhttp = getXhr ();

    // 调用onreadystatechange事件，接收后端返回的数据
    // 判断 readyState=4 时，使用 responseText 来接收数据
    xmlhttp.onreadystatechange = function () {
        //alert (xmlhttp.readyState);
        if (xmlhttp.readyState == 4) {
            //alert (xmlhttp.responseText);
            document.getElementById('d').innerHTML = xmlhttp.responseText;
        }
    }

    // 调用open方法准备ajax请求
    xmlhttp.open ('get', 'send.php');

    // 调用send方法发送ajax请求
    xmlhttp.send (null);
}
```



整体结果:

![1526094024137](assets/1526094024137.png)



##   2.4 Ajax程序总结

  一般我们编写Ajax程序时需要两个页面，三大步骤:

![1525944330962](assets/1525944330962.png)

  两个页面:

   前端静态html页面 (php页面也行)，用来发送ajax请求，将结果渲染到页面上
   后端php页面 (jsp、asp也行)，用来接收前端请求，处理数据，并将结果返回给前端页面

 

  三大步骤:

  1) 前端静态页面发送ajax请求。
  2) 后端php页面，处理请求并返回结果
  3) 前端页面接收结果，显示在网页指定位置

 

  发送Ajax请求固定步骤:

   1) 创建XMLHttpRequest对象
   2) 调用open方法准备ajax请求
   3) 调用send方法发送ajax请求
   4) 调用onreadystatechange事件，判断readyState=4时，使用responseText接收后端返回数据



##   2.5 综合案例

 点击按钮，从student表中随机取出一条学生信息，显示在网页上。

 思路分析:
 1) 两个页面
   前端静态页面: student/index.html
   后端php页面: student/index.php

 ![1524658135608](assets/1524658135608.png)

 2) 三大步骤
  ① index.html页面设置一个按钮，用来触发ajax请求
  ② index.php随机取出一条学生数据，并返回给index.html页面
  ③ index.html接收到后显示在页面上



 代码实现:

 index.html

```
<input type="button" value="随机获取" id="btn">
<div id="d"></div>

<script type="text/javascript">
function getXhr () {
    var xhr = null;
    try {
        //主流方法
        xhr = new XMLHttpRequest ();
    } catch (e) {
        xhr = new ActiveXObject ('Msxml2.XMLHTTP');
    }
    return xhr;
}  

//1. 在btn上绑定点击事件，用来触发ajax请求
document.getElementById('btn').onclick = function () {
    //2. 发送ajax请求
    //① 创建XMLHttpRequest对象
    var xmlhttp = getXhr ();

    //④ 调用onreadystatechange事件，判断readyState=4时，使用responseText接收后端返回数据
    xmlhttp.onreadystatechange = function (){
        if (xmlhttp.readyState == 4) {
            alert (xmlhttp.responseText);
            //document.getElementById('d').innerHTML = xmlhttp.responseText;
        }
    }

    //② 调用open 方法准备ajax请求
    xmlhttp.open ('get', 'index.php');

    //③ 调用send方法发送请求
    xmlhttp.send (null);
}
</script>
```

index.php

```
// 目标: 从student表中随机获取一条数据
//操作数据库的6步骤
//① 链接MySQL服务器
$conn = mysqli_connect('localhost', 'root', 'root');
//② 选择数据库
mysqli_select_db($conn, 'study');
//③ 设置字符集
mysqli_query($conn, 'set names utf8');
//④ 编写SQL并执行
$sql = "select * from student order by rand() limit 0,1";
$result = mysqli_query($conn, $sql);
//⑤ 处理SQL结果
$student_info = mysqli_fetch_assoc($result);
//⑥ 将结果转为字符串
$str = "我叫" . $student_info['sname'] . ", 今年" . $student_info['sage'] . "岁了";
//⑦ 关闭MySQL链接
mysqli_close($conn);

// 将得到的字符串返回给前端
echo $str;
```

index.html

```
//④ 调用onreadystatechange事件，判断readyState=4时，使用responseText接收后端返回数据
    xmlhttp.onreadystatechange = function (){
        if (xmlhttp.readyState == 4) {
            //alert (xmlhttp.responseText);
            document.getElementById('d').innerHTML = xmlhttp.responseText;
        }
    }
```

 





 

 关键点总结:

1) index.html 通过点击使用发送ajax请求
 ① 创建XMLhttpRequest对象
 ② 调用open方法准备ajax请求
 ③ 调用send方法发送ajax请求
 ④ 调用onreadystatechange事件，判断readyState=4时，使用responseText属性接收后盾返回数据

**==此时不要着急将数据渲染到网页上==**。

 2) index.php 随机取出一条学生数据，并返回给index.html页面
  ① mysqli操作MySQL服务器的6步骤
  ② 核心SQL:  select * from student order by rand() limit 0,1 
  ③ 将结果拼接成字符串再输出，因为前端是以字符串形式接收的

 3) index.html接收到后显示在页面上
  ① 创建一个div标签，用来显示接收到数据
  ② 获取div对象，再将数据显示到该标签中



# 3.POST和GET #
##    3.1 GET方式实现新用户注册---用户名检测 ##
 思路分析:

![1523697737792](assets/1523697737792.png)

 步骤:

1. get.html

​       1) 在用户名文本框上绑定失焦事件(onblur)
       2) 失焦事件函数
​            ① 获取用户名文本框内已填写的用户名
​            ② 发送ajax请求，并将已填写的用户名一起发送给后端php页面

2. get.php

    1) 接收前端发送过来的用户名
    2) 链接MySQL服务器，验证用户名是否被占用
    3) 将结果返回给前端

3. get.html

   修改get.html文件，区分接收到的结果。
   如果用户名已被占用，则在用户名文本框后面显示 ‘该用户已被占用’; 反之，则显示 ‘该用户名可用’





代码实现:

第一步:  get.html页面发送ajax请求

```
<form>
用户名:<input type="text" name="name" id="txt"><span id="s"></span><br>
密　码:<input type="password" name="pwd"><br>
</form>
//1. 获取用户名文本框，绑定失焦事件
document.getElementById('txt').onblur = function(){
    //2. 获取已经填写好的用户名
    var name = document.getElementById('txt').value;
    //3. 发送ajax请求，并将已填写的用户名一起发送给后端php页面
    //① 创建xhr
    var xmlhttp = getXhr ();

    //④ onreadystatechange事件监控readyState的值
    xmlhttp.onreadystatechange = function () {
        if (xmlhttp.readyState == 4) {
            // 将接收到的结果输出在终端
            console.log(xmlhttp.responseText);
        }
    }

    //② 准备ajax请求, 同时将文本框的用户名一起发送给后端get.php程序
    xmlhttp.open ('get', './get.php?name=' + name);
    //③ 发送ajax请求
    xmlhttp.send (null);
    
}

```



第二步: index.php 

```
三大步骤:
 1) 接收前端发送的数据
 2) 从ali_admin表中查询结果
 3) 将结果返回给前端

<?php 
//1. 接收前端提交的数据
$name = $_GET['name'];

//2. 链接MySQL进行验证
$conn = mysqli_connect('localhost', 'root', 'root');
mysqli_select_db($conn, 'study');
mysqli_query($conn, 'set names utf8');

//编写SQL语句并执行
$sql = "select * from ali_admin where admin_email='$name'";
$result = mysqli_query($conn, $sql);
//$result对象中至多只有一条数据，使用一次mysqli_fetch_assoc获取数组
$admin_info = mysqli_fetch_assoc($result);

//3. 返回验证结果
if (empty($admin_info)) {
    //如果为空说明用户名可用
    echo 1;
} else {
    //用户名不可用
    echo 2;
}
?>
```



第三步: index.html  显示后端返回的结果

```
//④ onreadystatechange事件监控readyState的值
    xmlhttp.onreadystatechange = function () {
        if (xmlhttp.readyState == 4) {
            //console.log(xmlhttp.responseText);
            
            var span_obj = document.getElementById('s');
            
            if (xmlhttp.responseText == 1) {
                //用户名可用
                span_obj.innerHTML = '用户名可用';
                span_obj.style.color = 'green';
            } else {
                //用户名不可用
                span_obj.innerHTML = '用户名已被占用';
                span_obj.style.color = 'red';
            }
        }
    }
```



关键点总结:

1. 用户名文本框绑定失焦事件

2. 发送ajax请求基本属于流程化操作

   1) 实例化XMLHttpRequest对象
   2) 调用open方法准备请求，==get方式发送将数据拼接在url地址之后即可==
     xhr.open('get', 'get.php?==n='name==);
   3) 调用send方法发送请求，==get方式只需要将 null  作为参数传入即可==
   4) 调用onreadystatechange事件，在readyState=4时使用responseText接收返回值。此步使用alert或者console.log先输出接收的结果即可，**==不要着急将结果显示在网页上==**。

3. 创建后端php程序，接收用户名进行验证

   核心SQL:  select * from ali_admin where admin_email = '$name';

   该SQL语句的执行结果只可能是两种： 0条数据    1条数据(因为admin_email字段唯一)

     0条数据: 说明没有该用户名 （没有被占用）

     1条数据: 说明已存在该用户名 （已被占用）

   根据SQL执行结果返回1或者2，1代表未被占用，2代表已被占用

4. 修改get.html文件，将结果显示在网页上

    获取用来显示结果的span标签，判断接收的结果为1还是2。如果为1，则将用户已被占用写入span标签；反之，则将用户可用写入span标签



##    3.2 POST方式实现新用户注册---用户名检测 ##
post和get两种方式的整体思路一致，只是细节上有所差别

 1) 使用open准备请求时，参数1需要设置为post，参数2只需要设置后端程序地址。
 2) 将需要传递到后端的数据拼接成一个独立的字符串，字符串的格式为
            ==var str = ‘key=value&key=value&....’;==    （内部结构跟get传参时的结构一致）
 3) 调用setRequestHeader方法将数据格式转为 application/x-www-form-urlencoded
 4) 将拼接好的数据字符串作为参数传入send方法
 5) 后端的php程序需要使用 $_POST来接收数据



代码实现:



关键点总结:
 1) 准备ajax请求时，参数1设置为post，参数2只用设置请求的后端文件路径
​    xhr.open('post', 'post.php');
 2) 将需要传递到后端的数据拼接成一个独立的字符串
​    var str = 'name='+name; 
 3) 调用setRequestHeader方法将数据格式转为 application/x-www-form-urlencoded
​    xhr.setRequestHeader('content-type', 'application/x-www-form-urlencoded');
 4) 发送请求时，要将之前拼接好的字符串作为参数放入send方法中
​    xhr.send(str);
 5)  后端的php要用 $_POST 来接收数据




# 4.GET缓存 #
##    4.1 什么是缓存？ ##
   浏览器的请求需要从服务器获得许多 css、img、js 等相关的文件，如果每次请求都把相关的资源文件加载一次，对 带宽、服务器资源、用户等待时间 都有严重的损耗。如果浏览器将css、img、js等文件在第一次请求成功后就保存在本机上，以后的每次请求就在本机获得相关的资源文件，那么就可以明显地加快用户的访问速度，同时可以节省各种资源(带宽、服务器资源、用户等待时间)。
##   4.2 GET缓存测试 ##
 ajax方式，get会有缓存问题。

 案例:
   index.html页面中创建一个按钮，点击该按钮时发送ajax请求，得到后端php程序返回的当前时间戳并显示。

![1523702454585](assets/1523702454585.png)

1. index.html

   点击“获取时间戳”按钮时，触发ajax请求，访问后端的getTime.php文件，得到时间戳并弹出显示

2. getTime.php

   输出当前时间戳即可。



 使用IE访问结果。 （其他浏览器不明显，IE缓存问题最严重）

 index.html 页面 用来发送ajax请求

```
<script type="text/javascript">
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4) {
        alert(xhr.responseText);
    }
}
xhr.open ('get', 'index.php');
xhr.send (null);
</script>
```



index.php 返回当前时间戳

```
<?php 
echo time();
?>
```

浏览结果:  使用ie

![1526112900404](assets/1526112900404.png)

永远是2773





##   4.3 解决方法 ##
 解决方法有两种:
  1) 前端方案:  在open准备ajax请求时，为请求的地址增加随机后缀。相当于每次请求都是新的地址
  2) 后端方案:  后端程序设置不允许缓存的头信息，php程序固定使用如下3句即可。
    header('cache-controller:no-cache');
    header('Pragam:no-cache');
    header('Expires:-1');



方式一: 推荐方式

```
<script type="text/javascript">
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4) {
        alert(xhr.responseText);
    }
}
//xhr.open ('get', 'index.php');
//为请求地址增加随机后缀
xhr.open ('get', 'index.php?_=' + Math.random())
xhr.send (null);
</script>
```



方式二:

```
<?php 
header('cache-controller:no-cache');
header('Pragam:no-cache');
header('Expires:-1');

echo time();
?>
```



# 5.同步和异步 #

##  5.1 同步/异步概念

  同步: ==顺序执行==  第一步---> 第二步 ---> 第三步 ....

  异步:  甲在完成一系列工作时，自己完成主工作。将一些分支工作交给乙，甲此时一直在完成自己的工作，并等待乙完成的结果。乙完成后将结果返回给甲。

![1523702943751](assets/1523702943751.png)

##  5.2 案例

   同时显示图片和弹出框

```
<script type="text/javascript">
var xhr = new XMLHttpRequest();
xhr.onreadystatechange = function () {
    if (xhr.readyState == 4) {
        alert(xhr.responseText);
    }
}
//参数3:  true 异步(默认值) / false 同步
xhr.open ('get', 'index.php', false);
xhr.send (null);
</script>  
<img src="1.jpg">
```

同步： 先弹出时间戳对话框, 点击“确定”之后，才显示图片

![1526114470136](assets/1526114470136.png)

异步： 弹出对话框和显示图片一起执行的

![1526114523654](assets/1526114523654.png)



# 6.Ajax技术总结 #
 1) ajax的作用: 在页面不刷新情况下也能刷新页面的局部数据，提升用户体验
 2) ajax的核心对象 --- XMLHttpRequest
     分IE（低版本）和非IE（包括IE的高版本）两种方式来创建。
         低版本IE: var xhr = new ActiveXObject(‘Msxml2.XMLHTTP’);
         非IE(高版本IE):  var xhr = new XMLHttpRequest();
 3) 发送Ajax请求的方法
     ① open(var1, var2, var3);
      参数1: 请求方式  post  get
      参数2: 请求地址  
      参数3: 同步(false)、异步(true  默认方式)

​     ② send(var);
         get方式: null     xhr.send(null);
         post方式: 被转换了格式的字符串
             var str = “id=1&name=zs&age=30”;
             xhr.setRequestHeader(‘content-type’, ‘application/x-www-form-urlencoded’);
             xhr.send(str);

 4) ajax的核心属性  --- readyState
       ajax程序的状态 0-4 一共5个值。 当readyState==4时，已经正常接收到后台程序返回的数据了。

 5) ajax核心事件 --- onreadystatechange
      每当readyState发生改变时，就会触发该事件
       0->1   1->2   2->3  3->4   一共触发4次

 6) ajax重要属性 ---  responseText

​     responseText: 以字符串形式接收后端程序返回的数据

 7) ajax程序实现的一般实现方式

​     两个文件:
         前端html文件，用来发送ajax请求
         后端php（jsp、asp）文件，用来运算并将结果返回给前端

​    三大步：

​      ① 前端文件发送ajax请求

​         i.  使用事件来触发ajax请求
         ii. 获取需要发送给后端的数据 --- 例如：用户名验证时的用户名
         iii. 创建xhr对象
         iv. 调用open方法准备ajax请求
         v. 调用send方法发送ajax请求
         vi. 调用onreadystatechange事件，判断readyState=4时，使用responseText接收后端返回结果

​         **==第一步最重要的是能将ajax请求发送出去并且能够接收到后端的返回数据，不要着急编写将数据显示到网页上的程序==**

​      ② 动态文件，用来运算并将结果返回给前端

​      ③ 前端文件

​	 i. 接收后端返回结果
         ii. 获取前端标签对象，将结果渲染到标签中



# 7. 省市区三级联动

1) 数据表设计

![1523879532339](assets/1523879532339.png)

​    查询属于广东省的所有市：  select  * from china where pid=1   (==广东的id=1==)



​    无限分类的应用领域非常多，例如： 新闻栏目，公司部门等等。

 

  2) 思路分析:    ==先完成  省 -> 市；  再完成  市 -> 区==

 省  -->  市：

![1523881756859](assets/1523881756859.png)



步骤:

1. index.php

​     1) 从china表查询所有省数据，并显示在省下拉菜单中

​     2) 在省下拉菜单上绑定onchange事件，发送ajax请求

​	下拉菜单中的值每次发生变化时会触发onchange事件

​     3) onchange事件函数

​        ① 获取当前选中省的id

​        ② 发送ajax请求，并将省id一起发送给后端php页面，等待返回结果



2. getCity.php

   1) 接收省id

   2) 拼接SQL语句，查询该省下所有的市 

   3) 将查询结果拼接为option字符串返回给前端

      <option value="3">西安</option><option value="4">宝鸡</option>

   ​

3. index.php

    接收到后端返回的字符串，添加到市下拉菜单中



4. 市 -->  区  同理