# Node

#### 1 补充模块导出

##### 1.1 导出多个成员：写法一（麻烦，不推荐）：

```js
// 导出多个成员：写法一
module.exports.a = 123
module.exports.b = 456
module.exports.c = 789
```

##### 1.2 **导出多个成员：写法二（推荐）**

 `module.exports` 提供了一个别名 `exports` （下面等价于上面的写法）。

```js
console.log(exports === module.exports) // => true
exports.a = 123
exports.b = 456
exports.c = 789
exports.fn = function () { 
}
```

##### 1.3 导出多个成员：写法三（代码少可以，但是代码一多就不推荐了）：

```js
// module.exports = {
//   d: 'hello',
//   e: 'world',
//   fn: function () {
//     // 
//     // 
//     // 
//     // 
//     // 
//     // 
//     // }
//  // }
```

##### 1.4 **导出单个成员：（唯一的写法）：**

```js
// 导出单个成员：错误的写法
// 因为每个模块最终导出是 module.exports 而不是 exports 这个别名
// exports = function (x, y) {
//   return x + y
// }


// 导出单个成员：必须这么写
module.exports = function (x, y) {
  return x + y
}
```



#### 2 [ES6-函数扩展](http://es6.ruanyifeng.com/#docs/function)

##### 2.1 函数参数默认值和剩余参数

```js
// function f(x, y = 10) {
//   console.log(x, y)
// }
// f(100)

// 函数的 rest（剩余） 参数
function sum(...args) {
  var sum = 0
  args.forEach(function (item, index) {
    sum += item
  })
  return sum
}
var ret = sum(1, 2, 3)
console.log(ret)
```

##### 2.2 箭头函数

- 箭头函数不能用于函数声明

- 箭头函数常用语函数表达式

- 匿名函数都是函数表达式

- 箭头函数不能当作构造函数 

  // 注意

  当函数体只有一句代码的时候，可以省略

  省略之后，不需要显示的 return 

  默认就是 return 后面的表达式的结果

  显示 return 了反而报错

  var add = (x, y) => x + y

##### 2.3 箭头函数this问题

- 箭头函数始终自动绑定外部环境 this 
- 箭头函数有时候不能乱用 
- 主要原因就是 this 指向问题 

```js
function Person(name, age) {
  this.name = name
  this.age = age
}

Person.prototype.sayHello = function () { // 这里不要使用箭头函数，使用了反而有问题
  console.log(this)

  // 这里的箭头函数反而就有意义了
  // 因为它绑定了外部环境的 this（就是我们期望的 Person 实例对象）
  setTimeout(() => { // 这里使用箭头函数才有意义
    console.log(this.age)
  }, 1000)
}
```



#### 3 [Express官网](http://expressjs.com/)

[Express中文网](http://www.expressjs.com.cn/)

原生的 http 模块在某些方面表现不足以应对我们的开发需求，所以我们就需要使用框架来加快我们的开发效率，框架的目的就是提高效率，让我们的代码更高度统一

##### 3.1 Express介绍

- Express 是一个基于 Node.js 平台，快速、开放、极简的 web 开发框架。
- 作者：tj
  - [tj 个人博客](http://tjholowaychuk.com/)
- 丰富的 API 支持，强大而灵活的中间件特性
- Express 不对 Node.js 已有的特性进行二次抽象，只是在它之上扩展了 Web 应用所需的基本功能
- 有很多[流行框架](http://expressjs.com/en/resources/frameworks.html)基于 Express

> [参考文档](http://expressjs.com/en/starter/installing.html)

```cmd
# 创建并切换到 myapp 目录
mkdir myapp
cd myapp

# 初始化 package.json 文件
npm init -y

# 安装 express 到项目中
npm i express
```

##### 3.2 [Hello World](http://nodejs.circle.ink/#/express?id=hello-world)

> [参考文档](http://expressjs.com/en/starter/hello-world.html)

```js
// 0. 加载 Express
const express = require('express')

// 1. 调用 express() 得到一个 app
//    类似于 http.createServer()
const app = express()

// 2. 设置请求对应的处理函数
//    当客户端以 GET 方法请求 / 的时候就会调用第二个参数：请求处理函数
app.get('/', (req, res) => {
  res.send('hello world')
})

// 3. 监听端口号，启动 Web 服务
app.listen(3000, () => console.log('app listening on port 3000!'))
```

##### 3.3 留言本案例(代码)

###### 3.3.1 准备

###### 3.3.2 配置路由

###### 3.3.3 使用模板引擎渲染页面

###### 3.3.4 处理表单提交

- 1 [接收表单数据](https://github.com/expressjs/body-parser)
- 2 数据校验
- 3 持久化存储(存到本地文件中)
- 4 发送响应

### 总结

##### 1 模块通信导出规则 exports

##### 2 exports 就是 module.exports 别名

##### 3  ES6 中函数的扩展

- 参数默认值
- 参数解构赋值
- rest 剩余参数
- 箭头函数
  - 语法
  - 绑定 this
    - ()=>{this}

##### 4 Express

##### **5 留言本案例流程**

###### **5.1准备**

```
1. 创建 myapp
2. cd myapp
3. npm init -y
4. npm i express
5. 创建 app.js
   GET / 响应 hello world
6. 创建 views 目录
7. 在 views 目录中新建 index.html、publish.html 文件
8. 将 data.json 拷贝到你的项目根目录下
```

###### 5.2 设计路由

| 请求方法 | 请求路径          | 备注                |
| ---- | ------------- | ----------------- |
| GET  | /             | 渲染响应 index.html   |
| GET  | /publish.html | 渲染响应 publish.html |
| POST | /publish.html | 处理表单 POST 提交请求    |
|      |               |                   |

```javascript
app.get('/', (req, res) => {
  // 渲染首页
})

app.get('/publish', (req, res) => {
  // 渲染发表页面
})

app.post('/publish', (req, res) => {
  // 处理发表请求
})
```

###### 5.3 使用模板引擎渲染页面

npm i art-template express-art-template

app.engine('html',require('express-art.template))

// 配置视图的后缀名 这里是art 存储在views/下  模板是aaa.art 替换为html

```javascript
app.get('/', (req, res) => {
  // 渲染首页
  res.render('index.html')
})

app.get('/publish', (req, res) => {
  // 渲染发表页面
  res.render('publish.html')
})

app.post('/publish', (req, res) => {
  // 处理发表请求
})
```

**将文件中数据动态渲染到** index.html 页面中

`app.js`:

```javascript
app.get('/', (req, res) => {
  fs.readFile('./data.json', (err, data) => {
    if (err) {
      throw err
    }
    data = data.toString() // 把二进制数据转为字符串
    data = JSON.parse(data) // 将 json 格式的字符串转为对象
    res.render('index.html', {
      posts: data.posts
    })
  })
})
```

`index.html`:

```html
<ul>
<% posts.forEach(function (item) { %>
  <li><%= item.name %>说：<%= item.content %></li>
<% }) %>
</ul>
```

###### **5.4处理表单提交**

```
1. 处理客户端表单
   method
   action
   表单控件的 name 属性
2. 提交测试
3. 服务端接收表单数据
   配置 body-parser 解析表单 POST 请求体
   参考文档:https://github.com/expressjs/body-parser
4. 处理表单数据
5. 持久化存储
  // 1. 先把文件读出来
  // 2. 把文件内容装成对象得到数组
  // 3. 往数组中 push 元素
  // 4. 再把数组对象转成字符串，重写到文件中
6. 服务端重定向
   res.redirect()
```





