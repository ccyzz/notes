# Ajax技术-2 #
每日目标：
# 1.JSON #
##  1.1 什么是JSON？ ##
JSON:  JavaScript Object Notation 是一种轻量级数据交互格式。

数据交互: 每一种语言的编码都不一样，它们之间互不认识。但是现在的情况是不同的语言开发出的系统也需要进行数据交互，这时候就需要一种大家都认识的语言或者技术来实现。


##  1.2 JSON数据的声明和使用 ##
声明:  var json_obj = {"key1":"value1", "key2":"value2", ...};
key: 双引号包含的字符串
value: 数据--数值型、字符串、数组、json对象，也用双引号包含

```
var json1 = {"name":"zs", "age":20};
alert(json1.name + " " + json1.age); 

var json2 = {
    "name":"于谦",
    "age": 50,
    "hobby": ['抽烟', '喝酒', '烫头'],
    "friend": {"dadang":"郭德纲", "tudi":["郭麒麟", "孙悦"]}
};
alert(json2.name + json2.hobby[2] + " " + json2.friend.dadang);
alert(json2.friend.tudi[1]);

var json3 = [
    {"name":"zs", "age":20},
    {"name":"ls", "age":21},
    {"name":"ww", "age":19}
]
```



JSON的本质: JSON 是 JS 对象的字符串表示法，它使用文本表示一个 JS 对象的信息，本质是一个字符串。
var obj = {a:"hello", b:"world"};       // js对象
var obj = {"a":"hello", "b":"world"};    // json格式的js对象，也可以叫json对象
var str = '{"a":"hello", "b":"world"}';    // json，也叫json格式的字符串 ==必须外层单引号，内存双引号==
##  1.3 PHP数组转JSON格式的字符串 ##
 php提供了函数：  json_str  json_encode($arr);

案例1: 索引数组转JSON
案例2: 关联数组转 JSON
案例3: 二维数组转JSON



```
<?php 
//一维索引数组
$arr = ['aaa', 'bbb', 'ccc'];
// 会将一维索引数组转为 数组格式的字符串
// 最终的结构是  '["aaa", "bbb", "ccc"]'
echo json_encode($arr);
echo "<hr />";

//一维关联数组
$info = ['id'=>1, 'name'=>'zs', 'age'=>20];
// 会将一维关联数组转为 json格式的字符串
// '{"id":1,"name":"zs","age":20}'
echo json_encode($info);
echo "<hr />";

//二维数组
$list = [
    0=>['id'=>1, 'name'=>'zs', 'age'=>20],
    1=>['id'=>2, 'name'=>'ls', 'age'=>21],
    2=>['id'=>3, 'name'=>'ww', 'age'=>19]
];
// 会将二维数组转为数组，内部每个单元都是一个json字符串
// [{"id":1,"name":"zs","age":20},{"id":2,"name":"ls","age":21},{"id":3,"name":"ww","age":19}]
echo json_encode($list);
?>
```



##  1.4 JSON字符串转JSON对象 ##
json格式的字符串转为json对象:  JSON.parse(json_str);
参数: json格式的字符串

```
//将一维数组字符串转为一维数组
var str2 = '["aaa", "bbb", "ccc"]';
var json2 = JSON.parse(str2);
console.log(json2[1]);

//将json字符串转为json对象
var str1 = '{"id":1,"name":"zs","age":20}';
var json1 = JSON.parse(str1);
console.log(json1.id + " " + json1.name + " " + json1.age);

// 将json格式的数组字符串转为数组(内部是json对象)
var str = '[{"id":1,"name":"zs","age":20},{"id":2,"name":"ls","age":21},{"id":3,"name":"ww","age":19}]';
var json_arr = JSON.parse(str);
console.log(json_arr);
console.log(json_arr[1].name);
console.log(json_arr[2].age);
```



json对象转为json格式的字符串:  JSON.stringify(json_obj);
参数: json对象

```
var json = {"id":1001, "name":"zs", "age":20};
var str = JSON.stringify(json);
console.log(str);  // '{"id":1001, "name":"zs", "age":20}'
```



##  1.5 案例 --- 搜索用户名，显示用户列表信息 ##
 思路:
1) 创建一个搜索页面，定义好搜索框，表头。在搜索按钮上绑定点击事件。
2) 点击事件能够发送ajax请求，并将用户名文本框中的内容一起发送到后端php程序
3) 后端php程序接收用户名，链接MySQL进行模糊查询，再将数组返回给前端
4) 前端接收到后端php返回值之后，循环显示出来



search.html

```
<input id="txt" type="text" name="name">
<input id="btn" type="button" value="搜索"><br>
<table border="1" width="600">
    <thead>
        <tr>
            <th>id</th>
            <th>邮箱</th>
            <th>昵称</th>
            <th>年龄</th>
            <th>性别</th>
        </tr>
    </thead>
    <tbody></tbody>
</table>

<script type="text/javascript">
//1. 使用事件来触发ajax --- 点击事件
document.getElementById('btn').onclick = function () {
    //2. 获取用户输入的name值
    var name = document.getElementById('txt').value;
    //3. 发送ajax请求
    // ① 创建XMLHttpRequest对象
    var xhr = new XMLHttpRequest ();

    // ④ 调用onreadystatechange事件
    xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
            alert (xhr.responseText);
        }
    }

    // ② 准备ajax请求
    xhr.open ('get', 'search.php?name=' + name + '&_=' + Math.random());
    // ③ 发送ajax请求
    xhr.send (null);
}
</script>
```



search.php文件

```
<?php 
//1. 接收前端发送的数据
$name = $_GET['name'];

//2. 处理数据 --- 模糊查询
$conn = mysqli_connect('localhost', 'root', 'root');
mysqli_select_db($conn, 'exp');
mysqli_query($conn, 'set names utf8');

//拼接SQL语句并执行
$sql = "select * from ali_admin where admin_email like '%$name%'";
$result_obj = mysqli_query($conn, $sql);

//处理SQL执行结果 --- 将结果转为二维数组
//$arr = [
//  ['admin_id'=>3, 'admin_email'=>'ashe@alic.om', ...],
//  ['admin_id'=>5, 'admin_email'=>'zhao@alic.om', ...],
//];
$arr = array();
while ($row = mysqli_fetch_assoc($result_obj)) {
    //如果不给下标，下标默认从0开始，自增1
    $arr[] = $row;
}

//3. 将结果数据返回给前端
echo json_encode($arr);
?>
```



search.html

```
// ④ 调用onreadystatechange事件
    xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
            //alert (xhr.responseText);
            //将json格式的字符串转回json (数组，内部是json)
            var json_arr = JSON.parse(xhr.responseText);
            //console.log(json_arr);
            // 循环数组，组装成 tr td + 数据的字符串
            var str = '';
            for (var i = 0; i < json_arr.length; i++) {
                json_arr[i]
                str += "<tr>";
                str += "<td>" + json_arr[i].admin_id + "</td>";
                str += "<td>" + json_arr[i].admin_email + "</td>";
                str += "<td>" + json_arr[i].admin_nickname + "</td>";
                str += "<td>" + json_arr[i].admin_age + "</td>";
                str += "<td>" + json_arr[i].admin_gender + "</td>";
                str += "</tr>";
            }
            // 将拼接好的字符串写入tbody中
            document.querySelector('tbody').innerHTML = str;
        }
    }
```




# 2. 模板引擎 #
##  2.1 为什么要使用模板引擎 ##
通过上一案例我们发现，要渲染到网页上的数据是使用js循环拼接字符串，再将拼接好的字符串填入tbody标签中的。
   这种方式可读性差，出错不容易查找。

   模板引擎技术就是为了解决字符串拼接问题的。 ==模板引擎技术本质就是拼接字符串。==

   传统模式有两种拼接显示字符串的方式:
在后端程序中拼接好，然后以字符串形式返回
后端程序返回json对象，前端程序接收了之后，解析json进行拼接

模板引擎方式：
    模板引擎属于前端程序拼接字符串
    提前定义好要显示格式，所有数据位置以特殊标记表示出来。模板引擎会自动分析，并将数据填写到对应的位置。

##  2.2 常见模板引擎 ##
ArtTemplate：https://github.com/aui/artTemplate
velocity.js：https://github.com/shepherdwind/velocity.js
Handlebars：http://handlebarsjs.com
##  2.3 artTemplate快速上手 ##
基本使用步骤:
  1) 使用script标签引入arttemplate库文件
  2) 定义标签，用来显示最终解析好的模板信息
  3) 定义模板。使用 <%=key%>  或者 {{key}} 将所有数据位置标记出来
  4) 调用template函数，解析模板，并将要显示的数据填充到已标记的位置
  5) 将解析好的模板字符串填充到事先定义好的标签中(显示到网页上)

简单案例：拼接模板字符串 “我叫张三，今年20岁”，并输出在网页的div中

```
<!-- 定义显示最终结果的标签 -->
<div id="d"></div> 

<!-- 定义模板，script标签需要设置id属性 -->
<script type="text/template" id="tpl">
我叫<%=name%>,今年<%=age%>
</script>

<script type="text/javascript">
//参数1: 模板标签的id值
//参数2: 填充到模板中的数据，必须是一个json对象
//返回值: 解析好的字符串
var json_obj = {"name":"zs", "age":20};
var html = template('tpl', json_obj); //我叫zs,今年20
document.getElementById('d').innerHTML = html;
</script>
```



关键点：

1) 在定义模板时使用 script 标签， type=“text/==template==”  id="tpl"
2) 定义json对象，json对象中的key一定要和模板中的 <%=key%>
3) 调用template函数进行解析
    参数1: 模板的id值
    参数2: json对象
    返回值:解析好的字符串


##  2.4 循环结构 --- for ##
  关键点: 
1) template函数需要的参数是一个json对象，所以需要声明json对象，里面是数组

```
var json = {
    "userlist": [
        {"id":10001, "name":"zs", "age":20},
        {"id":10002, "name":"ls", "age":21},
        {"id":10003, "name":"ww", "age":20},
    ]
};
var html = template(‘id’, json);
```



2) 在定义模板时使用for进行循环

```
<% for(i = 0; i < userlist.length; i++) { %>
        <%=userlist[i].id %>
        <%=userlist[i].name %>
        <%=userlist[i].age %>
<% } %>
```



```
<div id="d"></div>  

<script type="text/template" id="t">
<% for (i = 0; i < userlist.length; i++) { %>
 <%=userlist[i].id%> --- <%=userlist[i].name%> --- <%=userlist[i].age%><br>
<% } %> 
</script>

<script type="text/javascript">
//定义要解析的数据 --- 一定要是json对象
var json = {
    "userlist": [
        {"id":10001, "name":"zs", "age":20},
        {"id":10002, "name":"ls", "age":21},
        {"id":10003, "name":"ww", "age":20},
    ]
};
//调用template函数将数据填充到模板中，再进行解析
var html = template ("t", json);
// 将解析好的字符串填入div中
document.getElementById('d').innerHTML = html;

</script>
```



##  2.5 选择结构 --- if else ##
  关键点: 定义模板时使用 if else

```
<script type="text/template" id="t">
<% if (userlist.length != 0) { %>
    <% for (i = 0; i < userlist.length; i++) { %>
     <%=userlist[i].id%> --- <%=userlist[i].name%> --- <%=userlist[i].age%><br>
    <% } %> 
<% } else { %>
    没有相关数据
<% } %>
</script>

<script type="text/javascript">
//定义要解析的数据 --- 一定要是json对象
var json = {
    "userlist": [
       
    ]
};
//调用template函数将数据填充到模板中，再进行解析
var html = template ("t", json);
// 将解析好的字符串填入div中
document.getElementById('d').innerHTML = html;

</script>
```



##  2.6 使用模板引擎改造1.5案例 ##
 目标: 使用模板引擎来代替原来的字符串拼接



定义模板

```
<script type="text/template" id="tpl">
<% if (list.length != 0) { %>
    <% for (i = 0; i < list.length; i++) { %>
    <tr>
        <td><%=list[i].admin_id%></td>
        <td><%=list[i].admin_email%></td>
        <td><%=list[i].admin_nickname%></td>
        <td><%=list[i].admin_age%></td>
        <td><%=list[i].admin_gender%></td>
    </tr>
    <% } %>
<% } else { %>
    <tr>
        <td colspan="5" align="center">没有相关数据</td>
    </tr>
<% } %>
</script>
```

 

解析模板

```
// ④ 调用onreadystatechange事件
    xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
            //alert (xhr.responseText);
            // 将json字符串转为数组(内部是json对象)
            var json_arr = JSON.parse(xhr.responseText);
            // 将数组包装成json对象
            var json_obj = {"list": json_arr};
            // 调用template函数
            var html = template ("tpl", json_obj);
            // 将解析好的字符串显示到tbody中
            document.querySelector('tbody').innerHTML = html;
        }
    }
```





##  2.7 模板引擎原理简介(了解) ##
 核心原理:使用正则替换模板引擎中的标记
 例如: 我叫<%=name%>，今年<%=age%>岁
  使用正则表达式找到模板字符串中的<%=name%>和<%=age%>,再用真实的数据进行替换。

 核心方法:  
  ① reg.exec(str);
  reg: 正则表达式
  str:  字符串
  函数作用: 从str字符串中找到复合reg正则表达式的对象，如果没有则返回null

  ② str.replace(str1, str2);
  函数作用: 在str字符串中找到str1字符串，然后用str2字符串进行替换
  例如:
  str = “abcdefg”;
  str1 = “bc”;
  str2 = “zs”;
  console.log(str.replace(str1, str2)); //  azsdefg

  掌控每一步中变量的内容。

 

# 3.Ajax无刷新表单提交 #
## 3.1 DOM+Ajax表单提交 ##
通过DOM操作将表单中每个域的值取出，在拼接为字符串，发送到后端程序中。

dom.html

```
用户名: <input id="txt" type="text" name="name"><br>
密　码: <input id="pwd" type="text" name="pwd"><br>
<input type="button" value="提交" id="btn">

<script type="text/javascript">
document.getElementById('btn').onclick = function () {
    //获取表单的内容
    var name = document.getElementById('txt').value;
    var pwd  = document.getElementById('pwd').value;

    //发送ajax请求
    var xhr = new XMLHttpRequest ();
    xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
            alert(xhr.responseText);
        }
    }
    xhr.open ('post', 'dom.php');
    // 拼接参数字符串
    var str = 'name=' + name + '&pwd=' + pwd;
    // setRequestHeader重设请求格式
    xhr.setRequestHeader('content-type', 'application/x-www-form-urlencoded');
    xhr.send (str);
}
</script>
```

dom.php

```
<?php 
print_r($_POST);
?>
```



访问结果:

![1526288244244](assets/1526288244244.png)



## 3.2 FormData表单对象 ##
FormData对象优势就是能够一次性将表单中的所有数据全部取出，包括文件域的文件对象。

1)创建表单 --- form标签很重要， method和action不重要
   每个表单域需要设置name值

2)发送ajax请求
①将表单数据取出 --- FormData
i.获取form表单对象:        var fm = document.getElementById(‘mainForm’);  //DOM对象
ii.实例化FormData对象:  var fd = new FormData(fm);

②发送ajax请求
   使用FormData对象以后，必须使用post方式来发送ajax请求。
   将FormData对象，作为参数传入 send方法中  xhr.send(fd);

③ 使用FormData对象提交表单时，不需要设置 setRequestHeader方法



fd.html

```
<form id="mainForm">
用户名: <input type="text" name="name"><br>
密　码: <input type="text" name="pwd"><br>
头　像: <input type="file" name="pic"><br>
<input type="button" value="提交" id="btn">
</form>

<script type="text/javascript">
document.getElementById('btn').onclick = function () {
    //使用formdata获取表单数据
    //1. 获取form表单对象 -- DOM
    var fm = document.getElementById('mainForm');
    //2. 实例化formdata对象
    var fd = new FormData (fm);

    //3. 发送ajax请求
    var xhr = new XMLHttpRequest ();
    xhr.onreadystatechange = function () {
        if (xhr.readyState == 4) {
            alert(xhr.responseText);
        }
    }
    //formdata必须使用post请求
    xhr.open ('post', 'fd.php');
    //将formdata对象作为参数传入
    xhr.send (fd);
}

</script>
```



fd.php

```
<?php 
print_r($_POST);
print_r($_FILES);
?>
```



访问结果:

![1526289108350](assets/1526289108350.png)

