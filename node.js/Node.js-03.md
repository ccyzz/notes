

#### 1 完善留言板案例

##### 1.1 [统一处理静态资源](http://www.expressjs.com.cn/4x/api.html):添加样式和公开静态资源

- `express.static`  是 Express 内置的唯一一个中间件。是基于 [serve-static](https://github.com/expressjs/serve-static) 开发的，负责托管 Express 应用内的静态资源。

- `./public` 参数指的是静态资源文件所在的根目录

  `app.js`

  ```js
  // 开放 public 目录资源，访问路径不能带有 /public 前缀
  // http://localhost:3000/css/main.css
  // app.use(express.static('./public/'))

  // 开放 public 目录资源，限定以 /a 开头前缀
  // app.use('/a', express.static('./public/'))

  // 开放 public 目录资源，限定以 /public 开头前缀
  app.use('/public', express.static('./public/'))
  // 不要这么干，危险
  // app.use(express.static('./'))
  ```

##### 1.2 处理项目中的第三方资源

`app.js`

```js
app.use('/node_modules', express.static('./node_modules/'))
```

`index.html`

```js
  <link rel="stylesheet" href="/node_modules/bootstrap/dist/css/bootstrap.css">
```

##### 1.3 提取路由模块和提取处理函数模块

`router.js`

```/js
/**
 * 把路由相关的代码放到 router.js 文件模块中
 * 路由模块：
 *    根据不同的请求规则分发到具体的请求处理函数
 *    路由模块不做任何请求响应处理，不为违背的原则
 */
const handler = require('./handler')
// 0. 加载 Express
const express = require('express')
// 1. 调用 express.Router() 得到一个路由实例
const router = express.Router()
// 2. 为 router 添加路由请求处理
// router.get('/', handler.showIndex)
// router.get('/index.html', handler.showIndex)
// router.get('/publish', handler.showPublish)
// router.post('/publish', handler.publish)
router
  .get('/', handler.showIndex)
  .get('/index.html', handler.showIndex)
  .get('/2', handler.showIndex2)
  .get('/publish', handler.showPublish)
  .post('/publish', handler.publish)

// 3 导出路由模块module.exports = router
// 4. 在app.js中导入router
// 5. 在app.js中使用：app.use(router)挂载路由
```

`handle.js`

```js
// 目的: 处理不同请求发起时 服务端要做的事情 ->
// 1 新建handle.js 
// 2 exports.showIndex = cb 导出模块
// 3 在router.js中导入handle.js模块
// 4 导入当前handle.js 所依赖的模块 这里是fs
// 5 在router.js中使用handle模块中的相应成员
const fs = require('fs')
exports.showIndex = (req, res) => {
    fs.readFile('./data.json', (err, data) => {
        if (err) {
            throw err
        }
        data = data.toString()
        data = JSON.parse(data)
        res.render('index.html', {
            posts: data.posts
        })
    })
}

exports.showPublish = (req, res) => {
    res.render('publish.html')
}

exports.handlePublish = (req, res) => {
    let body = req.body
    fs.readFile('./data.json', (err, data) => {
        if (err) {
            throw err
        }
        data = data.toString()
        data = JSON.parse(data)
        data.posts.unshift(body)
        data = JSON.stringify(data)
        fs.writeFile('./data.json', data, () => {
            res.redirect('/')
        })
    })
}
```

##### 1.4 [在Node中使用mysql](https://github.com/mysqljs/mysql)

// mysql:在node中操作mysql数据库的包

// 开始之前确保mysql安装和启动

###### 1.4.1 安装

```shell
npm install mysql
```

###### 1.4.2 Hello World

```js
// 1引包
var mysql = require('mysql');
// 2买一个冰箱(创建一个数据库连接)
var connection = mysql.createConnection({
    host: 'localhost', // 要连接的主机名
    user: 'root', // 要连接的数据库的用户名
    password: '123456', // 数据库密码
    database: 'sqldemo' // 数据库的名字
});
// 3打开冰箱门(开始连接)
connection.connect();
// 4把大象装冰箱(执行sql语句) 
// query(增删改查的sql语句)
// SELECT 1 + 1 AS solution:将1+1的结果起了个名字叫solution(并没有查表)
// 建议在表名两侧加上`` 防止连写时报错
connection.query('SELECT * FROM `posts`', function(error, results, fields) {
    if (error) throw error;
    // console.log('The solution is: ', results[0].solution);
    console.log(results);
    // [ RowDataPacket { solution: 2 } ] 是一个数组
});

// 5关闭冰箱门
connection.end();
```

###### 1.4.3 增删改查

`条件查询：`

```js
const name = "hello"
const mail = "2@123.com"
    // SELECT * FROM`posts` WHERE`name` =?
    // 不是标准的sql语句, mysql包[]中的元素值会把sql语句中的?都给替换掉
connection.query(
    'SELECT * FROM`posts` WHERE `name`=? OR `mail`=?', [name, mail], (err, results, field) => {
        if (err) {
            throw err
        }
        console.log(results);
    })
```

`添加`

```js
const posts = {
    name: 'asa',
    content: 'awadd',
    mail: '22@121.com',
    date: '2018-6-22'
}
这里会把对象转成字符串,再传入?的位置
const sqlStr = 'INSERT INTO `posts` SET ?'
connection.query(sqlStr, posts, (err, results, field) => {
    if (err) {
        throw err
    }
    console.log(results);
})
```

`删除`

```js
const sqlStr = 'DELETE FROM `posts` WHERE `id`=?'
connection.query(sqlStr, [10], (err, results) => {
    if (err) {
        throw err
    }
    console.log(results);
})
```

`修改`

```js
const sqlStr = 'UPDATE `posts` SET `mail`=? WHERE `name`=?'
connection.query(sqlStr, ['lzz@111.com', 'asd'], (err, results) => {
    if (err) {
        throw err
    }
    console.log(results);
})
```

##### 1.5 把文件中的数据存入数据库

```js
const fs = require('fs')
var mysql = require('mysql');
var connection = mysql.createConnection({
    host: 'localhost', // 要连接的主机名
    user: 'root', // 要连接的数据库的用户民
    password: '123456', // 数据库密码
    database: 'sqldemo' // 数据库的名字
});
// 执行query()会自动连接
// connection.connect();

fs.readFile('./data.json', (err, data) => {
    if (err) {
        throw err
    }
    data = data.toString()
    data = JSON.parse(data)
    const posts = data.posts
    console.log(posts);
    posts.forEach((item, index) => {
        const sqlStr = 'INSERT INTO `posts` VALUES(NULL,?,?,?,?)'
        connection.query(sqlStr, [item.name, item.content, item.mail, item.time],
            (err, result) => {
                if (err) {
                    throw err
                }
                console.log(`${index}`);
            })
    });
})
```

##### 1.6 提取数据处理模块

```js
const fs = require('fs')
const mysql = require('mysql')
const connection = mysql.createConnection({
    host: 'localhost', // 要连接的主机名
    user: 'root', // 要连接的数据库的用户民
    password: '123456', // 数据库密码
    database: 'sqldemo' // 数据库的名字
})

exports.showIndex = (req, res) => {
    connection.query('SELECT *FROM `posts` ORDER BY `id` DESC', (err, results) => {
        if (err) {
            throw err
        }
        res.render('index.html', {
            posts: results
        })
    })

    // fs.readFile('./data.json', (err, data) => {
    //     if (err) {
    //         throw err
    //     }
    //     data = data.toString()
    //     data = JSON.parse(data)
    //     res.render('index.html', {
    //         posts: data.posts
    //     })
    // })
}

exports.showPublish = (req, res) => {
    res.render('publish.html')
}

exports.handlePublish = (req, res) => {
    let body = req.body

    connection.query(
        'INSERT INTO `posts` VALUES(NULL,?,?,?,?)',

        [body.name, body.content, body.mail, body.time], (err, results) => {
            if (err) {
                throw err
            }
            res.redirect('/')
        })

    // fs.readFile('./data.json', (err, data) => {
    //     if (err) {
    //         throw err
    //     }
    //     data = data.toString()
    //     data = JSON.parse(data)
    //     data.posts.unshift(body)
    //     data = JSON.stringify(data)
    //     fs.writeFile('./data.json', data, () => {
    //         res.redirect('/')
    //     })
    // })
}
```



#### 2 常用的几个包

##### 2.1 nodemon

在开发过程中，每次修改完代码手动重启服务器很麻烦。这里我们可以使用一个第三方命令行工具：[nodemon](https://github.com/remy/nodemon) 来帮我们解决这个问题。

`nodemon` 是一个基于Node.js 开发的一个第三方命令行工具，使用它的第一步就是先安装：

```shell
npm install --global nodemon
```

基本使用：

```shell
nodemon app.js
```

只要是通过 `nodemon app.js` 启动的服务，则它会监视你的文件变化， 当文件发生变化的时候，自动帮你重启服务器。

##### 2.2 全局命令行工具

我们除了可以使用 npm 安装项目中适应的依赖包之外，还有一些包比较特殊，这种不是在项目 `require()` 使用的，而是 通过全局安装之后使用它提供的命令来完成某种功能，就像咱们学过的 git 一样。

凡是往全局安装的包我们称之为全局命令行工具，这种包安装完毕之后会在命令行中为你提供了一个命令来完成某种功能。

> - 提示：安装全局包必须加 `--global` 参数

##### 2.3 [http-server工具](https://github.com/indexzero/http-server)

##### 2.4 全局包到底安装到了哪里

我们可以通过在命令行输入来查看全局包的安装路径：

```
npm root -g
```

现在这个东西只需要了解就行了。

因为以前的 node 版本一旦升级就会导致丢失了全局包，新版的 node 已经没有这个问题了。

##### 2.5 [npm script](http://www.ruanyifeng.com/blog/2016/10/npm_scripts.html)

- npm 允许在`package.json`文件里面，使用`scripts`字段定义脚本命令。
- `build`命令对应的脚本是`node build.js`。 
- 

#### 3 Node项目

##### 3.0 项目目的

- 熟悉开发流程
- 加深理解node核心特点

##### 3.1 项目介绍/演示

- cNode社区

##### 3.2 项目初始化

###### 3.2.1 git仓库

1. 在 GitHub 上创建一个仓库 `ithub`

2. 使用 Git 将远程仓库下载到本地 `git clone 远程仓库地址`

    .gitignore 排除文件

###### 3.2.2 初始化目录结构

```shell
cd ithub/
npm init
npm i express
```



```shell
.
├── node_modules
├── controllers 控制器
├── public 静态资源
├── views 视图
├── app.js 应用程序启动入口
├── .gitignore Git忽略文件
├── package.json 项目包说明文件，用来存储项目名称，第三方包依赖等信息
├── package-lock.json
└── README.md 项目说明文件
```

`app.js`

```js
const express = require('express')
const app = express()
app.get('/', (req, res) => res.send('Hello World!'))
app.listen(3000, () => console.log('Example app listening on port 3000!'))
```

##### 3.3 划分路由和处理函数模块

###### 3.3.1 路由设计

| 请求方法 | 请求路径                   | 说明       |
| ---- | ---------------------- | -------- |
| GET  | /                      | 渲染首页     |
| GET  | /signin                | 渲染登陆页面   |
| POST | /signin                | 处理登陆请求   |
| GET  | /signup                | 渲染注册页面   |
| POST | /signup                | 处理注册请求   |
| POST | /signout               | 处理退出请求   |
| GET  | /topic/create          | 渲染发布话题页面 |
| POST | /topic/create          | 处理发布请求请求 |
| GET  | /topic/:topicID        | 渲染话题详情页面 |
| GET  | /topic/:topicID/edit   | 渲染编辑话题页面 |
| POST | /topic/:topicID/edit   | 处理编辑话题请求 |
| POST | /topic/:topicID/delete | 处理删除话题请求 |
| POST |                        | 发表评论     |
|      |                        | 修改评论     |
|      |                        | 删除评论     |
|      |                        | 个人主页     |
|      |                        | 基本信息     |
|      |                        | 设置       |

###### 3.3.2 提取路由(router.js)

```js
// 0. 加载 express
const express = require('express')

// 加载所有的处理函数模块
const index = require('./controllers/index')
const topic = require('./controllers/topic')
const user = require('./controllers/user')

// 1. 调用 express.Router() 创建一个路由实例
const router = express.Router()

// 2. 配置路由规则

// 首页路由
router
  .get('/', index.showIndex)

// 用户路由
router
  .get('/signin', user.showSignin)
  .post('/signin', user.signin)
  .get('/signup', user.showSignup)
  .post('/signup', user.signup)
  .post('/signout', user.signout)

// 话题相关
router
  .get('/topic/create', topic.showCreate)
  .post('/topic/create', topic.create)
  .get('/topic/:topicID', topic.show)
  .get('/topic/:topicID/edit', topic.showEdit)
  .post('/topic/:topicID/edit', topic.edit)
  .post('/topic/:topicID/delete', topic.delete)

// 3. 导出路由对象
module.exports = router

// 4. 在 app.js 中通过 app.use(路由对象) 挂载使之生效
```

###### 3.3.3 提取用户处理模块(controllers/)

`user.js`

```js
const mysql = require('mysql')
const moment = require('moment')

const connection = mysql.createConnection({
  host: 'localhost', // 要连接的主机名
  user: 'root', // 要连接的数据库的用户名
  password: '123456', // 数据库密码
  database: 'ithub' // 数据库
})

exports.showSignin = (req, res) => {
  res.render('signin.html')
}

exports.signin = (req, res) => {
  res.send('signin')
}

exports.showSignup = (req, res) => {
  res.render('signup.html')
}

exports.signup = (req, res) => {
  // 1. 接收获取客户端提交的表单数据
  //    配置 body-parser 插件用来解析获取表单 POST 请求体数据
  const body = req.body

  // 2. 数据验证
  //    普通数据校验，例如数据有没有，格式是否正确
  //    业务数据校验，例如校验用户名是否被占用
  //    这里校验邮箱和昵称是否被占用

  // 校验邮箱是否被占用
  connection.query(
    'SELECT * FROM `users` WHERE `email`=?', [body.email],
    (err, results) => {
      if (err) {
        return res.send({
          code: 500,
          message: err.message // 把错误对象中的错误消息发送给客户端
        })
      }
      if (results[0]) {
        return res.send({
          code: 1,
          message: '邮箱已被占用了'
        })
      }

      // 校验昵称是否存在
      connection.query(
        'SELECT * FROM `users` WHERE `nickname`=?',
        [body.nickname],
        (err, results) => {
          if (err) {
            return res.send({
              code: 500,
              message: err.message // 把错误对象中的错误消息发送给客户端
            })
          }

          if (results[0]) {
            return res.send({
              code: 2,
              message: '昵称已被占用'
            })
          }

          // 邮箱和昵称都校验没有问题了，可以注册了
          // 3. 当数据验证都通过之后，在数据库写入一条新的用户数据
          
          // 添加更新时间
          // moment 是一个专门处理时间的 JavaScript 库，这个库既可以在浏览器使用，也可以在 Node 中使用
          // JavaScript 被称之为全栈式语言
          // moment() 用来获取当前时间
          // format() 方法用来格式化输出
          body.createdAt = moment().format('YYYY-MM-DD HH:mm:ss')

          const sqlStr = 'INSERT INTO `users` SET ?'

          connection.query(sqlStr, body, (err, results) => {
            if (err) {
              // 服务器异常，通知客户端
              return res.send({
                code: 500,
                message: err.message
              })
            }


            // 注册成功，告诉客户端成功了
            res.send({
              code: 200,
              message: 'ok'
            })

            // 用户注册成功之后需要跳转到首页
            // 1. 服务端重定向（只对同步请求有效）
            // res.send('注册成功')
            // 2. 让客户端自己跳
            // res.redirect('/')
          })
        }
      )
    }
  )
}

exports.signout = (req, res) => {
  res.send('signout')
}
```

`topic`

```js
exports.showCreate = (req, res) => {
  res.render('topic/create.html')
}

exports.create = (req, res) => {
  res.send('create')
}

exports.show = (req, res) => {
  res.send('show')
}

exports.showEdit = (req, res) => {
  res.send('showEdit')
}

exports.edit = (req, res) => {
  res.send('edit')
}

exports.delete = (req, res) => {
  res.send('delete')
}
```

##### 3.4 导入静态资源

- views/ 

- public/ 

- ithub.sql

  使用了include 导入

##### 3.5 配置模板引擎-下载第三方包-开放静态资源-渲染页面

###### 3.5.1 模板引擎

​	art-template和express-art-template

`app.js`

```js
const express = require('express')
const bodyParser = require('body-parser')
const router = require('./router')

const app = express()

// 1. 配置模板引擎
// 2. 渲染页面
// 3. 开放静态资源
// 4. 下载第三方包
//     bootstrap@3.3.7
//     jquery
app.use('/public', express.static('./public/'))
app.use('/node_modules', express.static('./node_modules/'))

// 配置 body-parser 解析表单 POST 请求体
// 只有配置了该插件，就可以在请求处理函数中使用 req.body 来访问请求体数据了
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))
// parse application/json
app.use(bodyParser.json())

// 配置使用 art-template 模板引擎
app.engine('html', require('express-art-template'))
app.use(router)
app.listen(3000, () => console.log('running 3000...'))
```

###### 3.5.2 第三方包

```shell
npm i bootstrap@3.3.7 jquery
```

##### 3.6 用户注册-客户端表单处理

#####  

### 总结

#### 0 常用的几个包

- 全局命令行工具
  - 也是基于Node.js开发的第三方包
  - 安装的时候需要加上 `--global` 选项参数
  - 安装全局命令行工具在任何目录执行都可以
  - 通常这种包安装完成之后就会在命令行提供一个命令供我们使用
  - 全局命令行工具安装一次就可以了，以后需要用的时候直接打命令
  - nodemon
  - http-server
  - browser-sync
  - less
- npm scripts
  - 有时候命令太长记不住，所以给命令起别名

#### 1 留言板案例

- Express 统一处理静态资源
  - 参考文档：http://expressjs.com/en/starter/static-files.html
  - `app.use(express.static('要开放的资源目录路径'))`
  - `app.use('限定资源访问前缀标识', express.static('要开放的资源目录路径'))`
- 将留言本案例中的路由模块独立出来
- 提取路由模块和请求处理函数模块
- 在 Node 中操作 MySQL 数据库
- 将留言本案例改造为 MySQL 数据库版

------



#### 2 项目编写流程

##### 2.1 准备

1. 创建 github 仓库
2. 克隆到本地
3. 创建 `.gitignore`，并配置忽略 `node_modules` 目录
4. npm init -y
5. npm i express

##### 2.2 创建 `app.js` 编写一个响应 `hello world` 的服务

```javascript
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

##### 2.3 抽取路由模块 `router.js`

```javascript
// 0. 加载 express
const express = require('express')

// 1. 调用 express.Router() 创建一个路由实例
const router = express.Router()

// 2. 配置路由规则
router.get('/', (req, res) => {
  res.send('hello world')
})

// 3. 导出路由对象
module.exports = router

// 4. 在 app.js 中通过 app.use(路由对象) 挂载使之生效
```

##### 2.4 设计路由表

| 请求方法 | 请求路径                   | 说明       |
| ---- | ---------------------- | -------- |
| GET  | /                      | 渲染首页     |
| GET  | /signin                | 渲染登陆页面   |
| POST | /signin                | 处理登陆请求   |
| GET  | /signup                | 渲染注册页面   |
| POST | /signup                | 处理注册请求   |
| POST | /signout               | 处理退出请求   |
| GET  | /topic/create          | 渲染发布话题页面 |
| POST | /topic/create          | 处理发布请求请求 |
| GET  | /topic/:topicID        | 渲染话题详情页面 |
| GET  | /topic/:topicID/edit   | 渲染编辑话题页面 |
| POST | /topic/:topicID/edit   | 处理编辑话题请求 |
| POST | /topic/:topicID/delete | 处理删除话题请求 |
| POST |                        | 发表评论     |
|      |                        | 修改评论     |
|      |                        | 删除评论     |
|      |                        | 个人主页     |
|      |                        | 基本信息     |
|      |                        | 设置       |

##### 2.5 根据路由表配置路由规则

```javascript
// 首页路由
router
  .get('/', index.showIndex)

// 用户路由
router
  .get('/signin', user.showSignin)
  .post('/signin', user.signin)
  .get('/signup', user.showSignup)
  .post('/signup', user.signup)
  .post('/signout', user.signout)

// 话题相关
router
  .get('/topic/create', topic.showCreate)
  .post('/topic/create', topic.create)
  .get('/topic/:topicID', topic.show)
  .get('/topic/:topicID/edit', topic.showEdit)
  .post('/topic/:topicID/edit', topic.edit)
  .post('/topic/:topicID/delete', topic.delete)
```

##### 2.6 创建控制器（处理函数）模块

- 创建 `controllers` 目录
- 在 `controllers` 目录中根据业务划分创建处理函数模块

`index.js` 文件：

```javascript
exports.showIndex = (req, res) => {
  res.send('showIndex')
}
```

`user.js` 文件：

```javascript
exports.showSignin = (req, res) => {
  res.send('showSignin')
}

exports.signin = (req, res) => {
  res.send('signin')
}

exports.showSignup = (req, res) => {
  res.send('showSignup')
}

exports.signup = (req, res) => {
  res.send('singup')
}

exports.signout = (req, res) => {
  res.send('signout')
}
```

`topic.js` 文件:

```javascript
exports.showCreate = (req, res) => {
  res.send('showCreate')
}

exports.create = (req, res) => {
  res.send('create')
}

exports.show = (req, res) => {
  res.send('show')
}

exports.showEdit = (req, res) => {
  res.send('showEdit')
}

exports.edit = (req, res) => {
  res.send('edit')
}

exports.delete = (req, res) => {
  res.send('delete')
}
```

然后访问几个路由规则测试一下。

##### 2.7 拷贝静态资源到项目中

下载项目需要的静态资源：

把下面的目录拷贝到项目中：

- public/
- views/
- ithub.sql

##### 2.8 配置模板引擎，渲染发送页面

- 配置 `art-template` 模板引擎
- 渲染登陆、注册、首页、创建话题... 页面

页面渲染出来，发现没有样式。

##### 2.9 公开静态资源、下载第三方包

下载：

```shell
npm i bootstrap@3.3.7 jquery
```

公开：

```javascript
app.use('/public', express.static('./public/'))
app.use('/node_modules', express.static('./node_modules/'))
```

再测试页面样式。

##### 2.10 用户注册

##### `前端页面处理`

```
// 异步提交表单
// 1. 监听表单的 submit 提交事件
//    设置事件处理函数
// 2. 在事件处理函数中
//    阻止表单默认的提交行为
//    采集表单数据
//    表单验证
//      https://github.com/jquery-validation/jquery-validation
//      自己尝试一下这个 jQuery 表单验证插件
//    发起 ajax 异步请求
//    根据服务端响应结果做交互处理
```

```javascript
$('#signup_form').on('submit', handleSubmit)

function handleSubmit(e) {
  e.preventDefault()
  var formData = $(this).serialize()
  $.ajax({
    url: '/signup',
    type: 'post',
    data: formData,
    dataType: 'json',
    success: function (data) {
      console.log(data)
      switch(data.code) {
        case 200:
          window.location.href = '/'
          break
        case 1:
          window.alert('邮箱已存在，请更换重试')
          break
        case 2:
          window.alert('昵称已存在，请更换重试')
          break
        case 500:
          window.alert('服务器内部错误，请稍后重试')
          break
      }
      // 判断 data 中的数据是否成功，成功则跳转到首页
    },
    error: function () {
    }
  })
}
```

