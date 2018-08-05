# PHP核心编程-day4

每日目标

- 能够说出什么是Http协议
- 能够说出什么是请求什么是响应
- 能够列举常见的状态码
- 能够安装MySQL服务端和客户端
- 能够创建数据库
- 能够创建数据表

# 1. Http协议

##  1.1 Http协议概述

  协议: 就是事先的一种约定、规则、规范、标准。（租房合同、工作合同）。

  HTTP协议：HyperText Transfer Protocol 超文本传输协议，客户端（浏览器端）与WEB服务器端之间的交互协议。当浏览器和服务器进行数据交换时，html文件、图片、CSS、JS等都是基于HTTP协议进行传输的。

  HTTP协议有两个版本: 1.0 和 1.1，目前使用的基本都是1.1

  特点: 
	通常是基于 B/S 结构软件的。
	无连接: 浏览器向服务器发送一次请求，服务器响应一次，链接即结束。
	无状态: 也叫无记忆。 服务器不能记住哪个浏览器访问过。

  

无连接 -- 没有持久化链接

当浏览器地址栏输入 www.baidu.com/index.html，按下回车时。浏览器向服务器发送请求，服务器找到index.html文件返回给浏览器之后，本次链接断开。

如果在点击该页面中任何一个链接，则重新建立一次链接。客户端发送请求，服务器响应。之后又断开链接。 

##  1.2 请求和响应

HTTP协议主要分为两大部分: 
	请求:  访问服务器的任何一个文件都是一次请求
	响应:  服务器处理请求，将结果返回给浏览器。

1) 请求(==request== / http request)
	客户端(浏览器)向服务器索要数据时遵循的协议

 	请求分为3个部分:  请求行   请求头   请求主体

​	请求行:  请求方式、请求URL地址、协议版本号
	请求头:  主机域名，客户端(浏览器)的信息
	请求主体:  发送给服务器的数据，get和post都会通过请求主体将数据发送给服务器

 	可以使用Chrome tools 或者 firebug 来查看请求和响应的信息（F12）

 

案例1:  访问 localhost/day4/code/http/index.html 文件

![1525745078424](assets/1525745078424.png)



![1525745243765](assets/1525745243765.png)

请求行:  

   get:请求方式
   /php4/code/http/index.html： 请求文件的路径
   HTTP/1.1  协议版本号

请求头:

   HOST： 请求的主机
   CONNECTION: 链接方式



案例2:  index.html表单数据提交到index.php文件



POST方式提交数据

![1525745654577](assets/1525745654577.png)



GET方式提交数据

![1525745731838](assets/1525745731838.png)



案例3:  index.html页面中a标签跳转到get.php文件

![1525745830532](assets/1525745830532.png)



 

2) 响应(==response== / http response)

​	响应也分为3部分:  响应行   响应头   响应主体

​	响应行: 协议版本号、响应结果状态码
	响应头: 主要是服务器端的信息、内容类型（content-type）、内容长度（content-length）
	响应主体: 就是从服务器返回给客户端的数据

   

案例1: 访问 localhost/day4/code/http/index.html 文件时的响应信息

![1525746406716](assets/1525746406716.png)

响应行

HTTP/1.1  协议版本号
304  状态码
Not Modified 状态码的英文解释



响应头

  Date: 响应时间
  Server: 服务器信息



响应主体

![1525746528225](assets/1525746528225.png)



案例2: index.html表单数据提交到index.php文件的响应信息

![1525746682147](assets/1525746682147.png)

content-type: text/html    告诉浏览器，这次响应的内容可以使用html来解析
content-length: 52   本次相应的字符串的长度



案例3: index.html页面中a标签跳转到get.php文件的响应信息

 ![1525746883313](assets/1525746883313.png)

![1525746900795](assets/1525746900795.png)



##  1.3 状态码

 常见的状态码如下: 

 200 ok   -----   请求成功
 302 redirect|Found  ----- 重定向
 403 forbidden   -----  禁止访问 （没有权限访问）
 404 Not Found  -----  未找到页面
 500 internal server error  ----- 服务器内部错误 (可能是服务器本身有问题，或者代码错的太离谱)

 

案例:  a.html 一共发送了几次请求

![1525747583722](assets/1525747583722.png)

 在浏览器中输入  localhost/php4/code/http/a.html， 一共发送3次请求

 a.html
 a.css
 1.jpg

![1525747704981](assets/1525747704981.png)

 

执行过程:

![1525747949380](assets/1525747949380.png)

 1） 先请求 a.html, 服务器返回a.html的代码给浏览器，浏览器开始解释执行
 2） 执行到 link 标签时，发现需要a.css文件，此时再次请求服务器，服务器再将a.css返回给浏览器
 3） 执行到img标签时，发现需要1.jpg文件，此时再次请求服务器，服务器再将1.jpg返回给浏览器

 

##  1.4 header响应头设置

1) 设置响应类型

​    浏览器发起请求的方式是多样的，当发起请求后服务端会有对应的内容响应过来，浏览器会根据响应头Content-Type来对响应的内容进行解析

​    content-type主要的响应类型是  text/html   
    其他常见类型 :  text/css   text/javascript   image/png  image/jpeg  image/gif



案例1: php绘制的验证码 verify.php

 ![1525749186865](assets/1525749186865.png)



访问结果:

![1525749201872](assets/1525749201872.png)

将响应类型改为  image/png，再次查看

![1525749268829](assets/1525749268829.png)

![1525749284702](assets/1525749284702.png)







案例2: link标签发送请求 

![1525749529563](assets/1525749529563.png)



![1525749539749](assets/1525749539749.png)

 

 ① 页面中的  link 、script等标签，请求一定会发，服务器也会响应
 ② 如果请求的不是正常文件，则需要指定响应类型

```
  <link rel="stylesheet" type="text/css" href="link.php">
  需要在 link.php文件中，使用header将响应类型改为 text/css
```

   



2) 指定字符集

   中文字符集: utf-8   gbk   gb2312  

​     header(‘Content-Type:text/html;charset=utf-8’);
     header(‘Content-Type:text/html;charset=gb2312’);

 

   页面乱码问题处理方式：
     页面乱码是因为==文件编码==和==页面指定编码==不一致，所以解决该问题就是要统一文件编码和页面指定编码

​    ① 在页面中设置 header ，编码指定为 utf-8 ； <meta charset="utf-8">
    ② 将文件的编码格式，指定为utf-8 (使用sublime，将保存格式设置为 utf-8 或者 使用editplus另存为文件时，设置utf-8)

 

3) 页面重定向

​     header('location:页面地址');
     header('refresh:2;url=页面地址');

```
header('location:aa.php?color=blue&bgcolor=red');
header('refresh:2;url=aa.php?width=200&height=100');
```



# 2. MySQL数据库服务器

##  2.1 什么是数据库

存储数据的仓库。

常见的数据库: MySQL、 Oracle、 Sqlserver、 DB2。

##  2.2 MySQL简介

MySQL是一个关系型数据库管理系统，由瑞典MySQL AB 公司开发，目前属于 Oracle 旗下产品 

MySQL结构：

![1525750941079](assets/1525750941079.png)



在一个MySQL服务器中有多个数据库，每个数据库又有多个数据表。 数据是存储在数据表当中的。

数据表的结构和excel一模一样

![img](assets/wps459E.tmp.jpg) 

表结构:  

   和excel表的结构是一样的。
   每一列都是一类数据 --- 字段
   每一行代表一条数据 --- 记录

 

##  2.3 安装客户端

MySQL是一款C/S结构的软件。

常见的客户端: CMD 、 Navicat、 Sqlyog、 phpmyadmin等等



安装 Navicat

1）解压

![img](assets/wps45C1.tmp.jpg) 

 

2) 选择安装版本

![img](assets/wps45C2.tmp.jpg) 

3)选择安装路径

![img](assets/wps45C3.tmp.jpg) 

 D:/navicat

继续下一步...

 

4)链接服务器

![img](assets/wps45C4.tmp.jpg) 

 ① 点击“链接”按钮 ---  选择要链接的数据库种类

![img](assets/wps45C5.tmp.jpg) 

 

 ② 配置链接信息

   用户名： root       该用户是MySQL服务器系统的最高用户，拥有该系统的所有权限
   密码：  root             phpstudy中MySQL系统root用户的默认密码

![img](assets/wps45C6.tmp.jpg) 

 

  ③ 点击“localhost”结果

![img](assets/wps45C7.tmp.jpg) 

 

左侧的localhost下的内容都是数据库名称。

  information_schema、mysql、performance_schema 这三个是系统数据库。
  其他的都是自建数据库

看到上图，说明已经使用navicat 客户端正常链接到了 MySQL服务器了。



# 3. 创建/删除数据库

操作数据库需要使用SQL语句（结构化查询语言）

1) 创建数据库

​    语法格式: create database 库名;

示例: create database study;   创建一个名为 study的数据库

​          create database cms:     创建一个名为 cms 的数据库

 

  ① 打开“查询编辑器” (编写SQL语句的地方)，编写SQL语句

 ![1525762542555](assets/1525762542555.png)

![1525762573654](assets/1525762573654.png)

  ② 执行创建数据库的SQL语句

  ![1525762677428](assets/1525762677428.png)



看到上图，就说名数据库已经成功创建

 

2) 删除数据库:

  语法格式:  drop database 库名;

  示例:     drop database school;  删除school库

​                drop database cms;      删除cms库

 ![1525762826110](assets/1525762826110.png)



# 4. 创建数据表

##  4.1 创建表的基本格式

语句格式:

CREATE TABLE 表名(
       列名     数据类型(长度)    完整性约束条件,
       列名     数据类型(长度)    完整性约束条件,
       ......
) engine=myisam default charset=utf8

 

==注意事项:==
   ==列名、数据类型、长度是必有的； 完整性约束条件可以没有==
   ==最后的 utf8 没有 -==



##  4.2 创建用户表 -- users 

案例:  在cms数据库中，创建一张users数据表，该表中只保存用户id和用户名两个字段

==一定要先选中 数据库，再去打开查询编辑器== 

![1525763239117](assets/1525763239117.png)



 编写并执行SQL语句

![1525763444100](assets/1525763444100.png)

![1525763481361](assets/1525763481361.png)

看到上图，说明数据表创建成功





出错示例:

![1525763903538](assets/1525763903538.png)







# 5. 数据类型和完整性约束条件

##  5.1 数据类型

1) 整型:  

​    tinyint（微整型 -128~~127）  
    int (-2,147,483,648  ~~ 2,147,483,647)

 2) 字符串:

​     varchar: 可变长度  varchar(30)    存储abc，varchar会占用3位长度
     char: 固定长度     char(30)    存储abc，char会占用30位长度
     char的执行速度比varchar快，所以能用char就用char

3) 枚举:  enum （单选）  /  set(多选)

​    enum(‘男’, ‘女’)   
    enum(‘武侠’, ‘玄幻’, ‘恐怖’, ‘都市言情’)

​    set('喜剧', '恐怖', '穿越', '玄幻'，'古装');

4) 日期:  

​    date :   年-月-日
​    datetime :  年-月-日  时:分:秒
    int: 存储时间戳

5) 文本:  text  大文本

6) float: 浮点型

​    float(6,2);   浮点型的总长度最大为6， 小数点后面2位    1234.56 正确      12345.67 错误

## 5.2 完整性约束条件

   进一步说明，当前字段能够存储什么样的数据

1) unsigned: 无符号，只用在整型字段上。

   tinyint unsigned  无符号微整型 0~~255 

2) auto_increment: 自增长，只用在整型字段上。 

3) primary key: 主键。 
      特点:  唯一 、 非空 、经常和auto_increment结合使用。 主键和自增长配合就能确定唯一的一条数据了。 主键一般都会选择整型字段。

4) unique: 唯一。 该列中不能出现重复值

5) not null: 非空。 该列中不能有 NULL 数据。	

## 5.3 创建管理员基本信息表 （ali_admin）

  所需字段:
    id:  整型 无符号  自增长  主键
    邮箱(账号、用户名):  字符串  唯一  非空
    昵称:  字符串  唯一
    密码:  字符串  非空  md5加密
    手机:  字符串
    性别:  枚举，男、女、人妖
    年龄:  微整型  无符号
    个人介绍: 文本
    添加时间:  整型，记录时间戳

```
create table ali_admin(
  admin_id int UNSIGNED auto_increment primary key comment '管理员id 主键 自增长',
  admin_email varchar(50) UNIQUE not null comment '管理员邮箱 唯一，非空',
  admin_nickname varchar(30) unique not null comment '管理员昵称',
  admin_pwd  char(32) not null comment '密码,md5加密后',
  admin_tel  char(11) comment '手机号',
  admin_gender enum('男', '女', '人妖') comment '性别',
  admin_age tinyint UNSIGNED comment '年龄',
  admin_sign text comment '个人介绍',
  admin_addtime int UNSIGNED comment '添加时间，时间戳'
)engine=myisam default charset=utf8;
```

![1525766782075](assets/1525766782075.png)



# 6. 创建关系型数据库

## 6.1关系型数据库简介

案例: 创建一个数据表，能够保存学生的基本信息(学号、姓名、年龄等)和学生每一科的考试成绩

1)  一张表的形式

![img](assets/wps45E4.tmp.jpg) 

缺点: 重复数据太多（数据冗余）

 

2) 关系型数据库:

![1525767829233](assets/1525767829233.png)

 

缺点: 表多
优点:
   数据耦合性低
   每个数据表都能够独立管理



   

## 6.2 创建栏目表 （ali_cate）

 所需字段:
   id:  微整型 无符号  自增长  主键
   栏目名称:  字符串  唯一  非空
   栏目别名:  字符串  唯一  非空
   添加时间:  日期型 date

![1525768975536](assets/1525768975536.png)

​    

## 6.4 创建文章表 （ali_article）

 所需字段 
   id: 整型 无符号  自增长  主键
   文章标题: 字符串  唯一
   文章内容: 文本
   文章作者: 整型 无符号 关联ali_admin表的admin_id字段
   所属栏目: 微整型 无符号  关联ali_cate表的cate_id字段

![1525769598490](assets/1525769598490.png)

## 6.5 ali_admin、ali_cate 和 ali_article的关系 

模拟数据

ali_admin:

![1525769776815](assets/1525769776815.png)



ali_cate:

![1525769788671](assets/1525769788671.png)



ali_article:

![1525769758579](assets/1525769758579.png)

第三条数据是错误数据，因为 article_adminid的值在admin表中是不存在的。 article_cateid的值在cate表中也不存在。



![1525770070841](assets/1525770070841.png)



# 7. 添加数据

 格式:  insert into 表名(字段名1，字段名2,....)  values(值1，值2，....)

 注意: ==字段的顺序要和值的顺序是完全匹配的==
           ==自增长类型的主键，可以使用null来填充；MySQL会自动填充数据==
           ==如果每个字段都有数据，那么表名后面可以不跟字段名，但是value里面的顺序必须正确==

 

 案例1: 向ali_cate表中添加一条数据

方式1:

 ![1525770617105](assets/1525770617105.png)



方式2:

![1525770719106](assets/1525770719106.png)



方式3：

![1525770800022](assets/1525770800022.png)



 案例2: 向ali_admin表中添加一条数据

 

 

下午内容总结:

1. 创建/删除数据库

   create  database  库名;
   drop   database 库名;

2. 创建表

   create table  表名(

   ​     列名   数据类型(长度)    完整性约束条件,
        列名   数据类型(长度)    完整性约束条件,
        ......

    )

    数据类型:

    1) 整型：
         微整型  tinyint     -128~127
         整型      int            -21亿 ~ 21亿

     2) 字符串：
          varchar  可变长度
          char    固定长度

     3) enum/set    单选/多选

   ​        enum('男','女','人妖')

      4) 文本： text

      5) 日期：

   ​	date： 年-月-日
           datetime： 年-月-日 时:分:秒
   	int： 时间戳

   ​    6) 浮点型：

   ​        float(5, 2);  

3. 完整性约束条件

    1) 无符号： unsigned 

   ​       tinyint  unsigned     0-255

    2) 自增长： auto_increment

   ​	和整型配合使用

     3) 主键： primary key

   ​         唯一、非空，经常和auto_increment配合使用。可以在数据表中表明唯一的一条数据

     4) 唯一： unique

     5) 非空： not null

4. 关系型数据库

5. 数据添加

   语法格式:  insert into  表名(字段名1，字段名2，....)  values(值1，值2，....);

   注意:

      值和字段需要一一对应
      自增长字段可以使用null来代替
      如果字段是全字段则可以省略  insert into  表名  values(值1，值2，....);















