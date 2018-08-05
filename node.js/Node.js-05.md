

### 1 [中间件](http://www.expressjs.com.cn/guide/using-middleware.html)

#### 1.1 中间件基本概念

Express 的最大特色，也是最重要的一个设计，就是中间件。

一个 Express 应用，就是由许许多多的中间件来完成的。

为了理解中间件，我们先来看一下我们现实生活中的自来水厂的净水流程--

自来水厂从获取水源到净化处理交给用户，中间经历了一系列的处理环节，

我们称其中的每一个处理环节就是一个中间件。

这样做的目的既提高了生产效率也保证了可维护性。 

`一个简单的中间件例子：打印日志`

```js
app.get('/', (req, res) => {
  console.log(`${req.method} ${req.url} ${Date.now()}`)
  res.send('index')
})

app.get('/about', (req, res) => {
  console.log(`${req.method} ${req.url} ${Date.now()}`)
  res.send('about')
})

app.get('/login', (req, res) => {
  console.log(`${req.method} ${req.url} ${Date.now()}`)
  res.send('login')
})
```

在上面的示例中，每一个请求处理函数都做了一件同样的事情：

请求日志功能（在控制台打印当前请求方法、请求路径以及请求时间）。

针对于这样的代码我们自然想到了封装来解决：

```js
app.get('/', (req, res) => {
  // console.log(`${req.method} ${req.url} ${Date.now()}`)
  logger(req)
  res.send('index')
})

app.get('/about', (req, res) => {
  // console.log(`${req.method} ${req.url} ${Date.now()}`)
  logger(req)
  res.send('about')
})

app.get('/login', (req, res) => {
  // console.log(`${req.method} ${req.url} ${Date.now()}`)
  logger(req)
  res.send('login')
})

function logger (req) {
  console.log(`${req.method} ${req.url} ${Date.now()}`)
}
```

这样的做法自然没有问题，但是大家想一想，我现在只有三个路由，如果说有10个、100个、1000个呢？那我在每个请求路由函数中都手动调用一次也太麻烦了。 

```js
const express = require('express')
const app = express()
// app.use(请求处理函数)
// app.use('路径标识', 请求处理函数)
// app.get
//    .post
//    ... HTTP Method

// 不关心请求路径和请求方法
// 任何请求都会进入
app.use(function (req, res, next) {
  console.log(`请求方法：${req.method} 请求路径：${req.path}`)
  // res.send('use')

  // next 是一个函数，目前不需要传递参数
  // 调用 next 就是在调用下一个（能匹配的）中间件
  next()
})

app.get('/signin', function (req, res) {
  res.send('signin')
})

app.get('/', function (req, res) {
  res.send('home')
})

app.get('/signup', function (req, res) {
  res.send('signup')
})

app.listen(3000, () => console.log('running...'))
```

上面代码执行之后我们发现任何请求进来都会先在服务端打印请求日志，然后才会执行具体的业务处理函数。

#### 1.2 中间件执行规则

```js
const express = require('express')

const app = express()

// 每个中间件处理函数都可以接收三个参数
//    req 请求对象
//    res 响应对象
//    next 一个函数，下一个能匹配的中间件


// app.use(中间件处理函数)
// app.use('标识', 中间件处理函数)
// app.get()
//    .post()
//    .HTTP_METHOD

// 任何请求进来都会从代码最开始的中间件进行匹配调用

app.use(function (req, res, next) {
  console.log(1)
  next()
})

// 限定必须以 /a/ 开头的才会进来
// 这里的规则不是 === /a/
// 而是 startsWith('/a/')
app.use('/a/', (req, res, next) => {
  res.send('/a/')
})

app.use(function (req, res, next) {
  console.log(2)
  next()
})

app.get('/', (req, res, next) => {
  console.log('/ 1')
  next()
})

app.use((req, res, next) => {
  console.log(3)
  next()
})

app.get('/a', (req, res, next) => {
  console.log('/a')
})

app.listen(3000, () => console.log('running...'))
```

`补充:在这里为 req 或者 res 动态的挂载的成员，在后续请求处理函数中都可以使用`

```js
const express = require('express')

const app = express()

// 在这里为 req 或者 res 动态的挂载的成员，在后续请求处理函数中都可以使用
app.use(function (req, res, next) {
  req.foo = 'bar'
  req.body = {
    foo: 'bar'
  }
  req.session = {}
  next()
})

app.get('/', (req, res) => {
  res.send(req.body)
})

app.listen(3000, () => console.log('running...'))
```



#### 1.3 中间件的应用

##### 1.3.1 模拟express-static实现

```js
const express = require('express')
const fs = require('fs')

const app = express()

// app.use(express.static('./node_modules/'))
// app.use(express.static('./express-demo/'))

// app.use((req, res, next) => {
//   // /accepts/README.md
//   // /bytes/package.json
//   // /debug/.eslintrc

//   const filePath = './node_modules' + req.path

//   fs.readFile(filePath, function (err, data) {
//     if (err) {
//       // 这里的错误意味着文件找不到
//       // 我当前中间件就处理不了了
//       // 我这里处理不了，进入下一个能匹配你的中间件去处理
//       return next()
//     }
//     res.end(data)
//   })

//   // res.send(filePath)
// })

// app.use((req, res, next) => {
//   // /accepts/README.md
//   // /bytes/package.json
//   // /debug/.eslintrc

//   const filePath = './express-demo' + req.path

//   fs.readFile(filePath, function (err, data) {
//     if (err) {
//       // 这里的错误意味着文件找不到
//       // 我当前中间件就处理不了了
//       // 我这里处理不了，进入下一个能匹配你的中间件去处理
//       return next()
//     }
//     res.end(data)
//   })

//   // res.send(filePath)
// })

// 调用 static 方法，返回了一个函数
// 返回值函数就作为 app.use() 方法的参数了
// app.use(static('./node_modules/'))
app.use(static('./express-demo/'))

function static(path) {
  return function (req, res, next) {
    const filePath = path + req.path
    fs.readFile(filePath, function (err, data) {
      if (err) {
        return next()
      }
      res.end(data)
    }) 
  }
}

app.listen(3000, () => console.log('running...'))
```

##### 1.3.2 统一打印日志

```js
const express = require('express')
const fs = require('fs')

const app = express()

app.use(logger)

function logger(req, res, next) {
  console.log(`${req.method} ---- ${req.path}`)
  next()
}

app.use(static('./express-demo/'))

function static(path) {
  return function (req, res, next) {
    const filePath = path + req.path
    fs.readFile(filePath, function (err, data) {
      if (err) {
        return next()
      }
      res.end(data)
    }) 
  }
}

app.listen(3000, () => console.log('running...'))
```

##### 1.3.3 路由也是中间件



#### 1.4 中间件的组成

中间件函数可以执行以下任何任务：

- 执行任何代码
- 修改 request 或者 response 响应对象
- 结束请求响应周期
- 调用下一个中间件

#### 1.5 中间件的分类

- 应用程序级别中间件
- 路由级别中间件
- 错误处理中间件
- 内置中间件
- 第三方中间件

##### 1.5.1 应用级别的中间件

###### 1.5.1.1 不关心请求路径：

```
var app = express()

app.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
})
```

###### 1.5.1.2 限定请求路径：

```
app.use('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})
```

###### 1.5.1.3 限定请求方法：

```
app.get('/user/:id', function (req, res, next) {
  res.send('USER')
})
```

###### 1.5.1.4 多个处理函数：

```
app.use('/user/:id', function (req, res, next) {
  console.log('Request URL:', req.originalUrl)
  next()
}, function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})
```

###### 1.5.1.5多个路由处理函数：

```
app.get('/user/:id', function (req, res, next) {
  console.log('ID:', req.params.id)
  next()
}, function (req, res, next) {
  res.send('User Info')
})

// handler for the /user/:id path, which prints the user ID
app.get('/user/:id', function (req, res, next) {
  res.end(req.params.id)
})
```

###### 1.5.1.6 最后一个例子：

```
app.get('/user/:id', function (req, res, next) {
  // if the user ID is 0, skip to the next route
  if (req.params.id === '0') next('route')
  // otherwise pass the control to the next middleware function in this stack
  else next()
}, function (req, res, next) {
  // render a regular page
  res.render('regular')
})

// handler for the /user/:id path, which renders a special page
app.get('/user/:id', function (req, res, next) {
  res.render('special')
})
```

##### 1.5.2 路由级别中间件

创建路由实例：

```js
var router = express.Router()
```

示例：

```js
var app = express()
var router = express.Router()

// a middleware function with no mount path. This code is executed for every request to the router
router.use(function (req, res, next) {
  console.log('Time:', Date.now())
  next()
})

// a middleware sub-stack shows request info for any type of HTTP request to the /user/:id path
router.use('/user/:id', function (req, res, next) {
  console.log('Request URL:', req.originalUrl)
  next()
}, function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
})

// a middleware sub-stack that handles GET requests to the /user/:id path
router.get('/user/:id', function (req, res, next) {
  // if the user ID is 0, skip to the next router
  if (req.params.id === '0') next('route')
  // otherwise pass control to the next middleware function in this stack
  else next()
}, function (req, res, next) {
  // render a regular page
  res.render('regular')
})

// handler for the /user/:id path, which renders a special page
router.get('/user/:id', function (req, res, next) {
  console.log(req.params.id)
  res.render('special')
})

// mount the router on the app
app.use('/', router)
```

另一个示例：

```js
var app = express()
var router = express.Router()

// predicate the router with a check and bail out when needed
router.use(function (req, res, next) {
  if (!req.headers['x-auth']) return next('router')
  next()
})

router.get('/', function (req, res) {
  res.send('hello, user!')
})

// use the router and 401 anything falling through
app.use('/admin', router, function (req, res) {
  res.sendStatus(401)
})
```

##### 1.5.3 错误处理中间件(演示)

```js
const express = require('express')
const fs = require('fs')

const app = express()

app.get('/', (req, res, next) => {
  // JavaScript 有一个专门捕获异常的语法
  // try-catch
  // 我们把可能出错的代码写到 try 语句块中
  // 如果 try 中代码一旦出错，则会进入 catch 块
  // catch 接收一个参数 err ，就是 try 中出错的错误对象
  try {
    JSON.parse('dsadsa')  
  } catch (err) {
    // res.send({
    //   code: 500,
    //   message: err.message
    // })
    // handleError(res, err)
    // 当你调用 next 中间件并传递了参数的时候
    // 则会找到下一个具有四个参数的中间件调用
    // (err, req, res, next)
    next(err)
  }

  // 发送 500 告诉客户端服务器内部错误
})

app.get('/abc', (req, res) => {
  fs.readFile('dsadsa', (err, data) => {
    if (err) {
      // 发送 500 告诉客户端服务器内部错误
      // res.send({
      //   code: 500,
      //   message: err.message
      // })
      handleError(res, err)
    }
    res.end(data)
  })
})

app.use(function (req, res, next) {
  res.send('end')
})

// 对于一个 Express 应用来讲，写一个错误处理中间件就够了
app.use(function (err, req, res, next) {
  res.send('end')
})

// function handleError(res, err) {
//   res.send({
//     code: 500,
//     message: err.message
//   })
// }

app.listen(3000, () => console.log('running...'))
```

```js
app.use(function (err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})
```

##### 1.5.4 内置中间件

- [express.static](http://expressjs.com/en/4x/api.html#express.static) serves static assets such as HTML files, images, and so on.
- [express.json](http://expressjs.com/en/4x/api.html#express.json) parses incoming requests with JSON payloads. **NOTE: Available with Express 4.16.0+**
- [express.urlencoded](http://expressjs.com/en/4x/api.html#express.urlencoded) parses incoming requests with URL-encoded payloads. **NOTE: Available with Express 4.16.0+**

官方支持的中间件列表：

- <https://github.com/senchalabs/connect#middleware>

##### 1.5.5 第三方中间件(演示)

- [morgan](https://github.com/expressjs/morgan) 日志中间件
- [serve-index](https://github.com/expressjs/serve-index) 处理目录列表中间件
- [errorhandler](https://github.com/expressjs/errorhandler) 错误处理中间件

[第三方中间件列表](http://expressjs.com/en/resources/middleware.html)

------

### 2 项目完善

#### 2.1 利用中间件配置404页面

`app.js`

```js
// 挂载路由...
// 当前面没有任何一个中间件能处理该请求的时候，则会进入这个中间件
// 注意：一定要在路由之后，否则整站都是 404 了
app.use((req, res, next) => {
  res.render('404.html')
})

// ...
```

#### 2.2 发布话题

- 处理前端页面
- 处理服务端

##### 2.2.1 处理创建话题的客户端

`topic/create.html`

```javascript
 <script>
    $('#form').on('submit', handleSubmit)
    function handleSubmit(e) {
      e.preventDefault()
      var formData = $(this).serialize()
      $.post('/topic/create', formData, function (data) {
        if (data.code === 200) {
          window.location.href = '/'
        }
      })
    }
  </script>
```

##### 2.2.2 读取话题列表,渲染首页

`controller/index.js`

```javascript
// 我们把话题相关的数据库操作方法都封装到当前模块
const db = require('../controllers/db-helper')

/**
 * 获取所有话题列表
 * @param  {Function} callback 回调函数
 * @return {undefined}            没有返回值
 */
exports.findAll = callback => {
  const sqlStr = 'SELECT * FROM `topics`'
  db.query(sqlStr, (err, results) => {
    if (err) {
      return callback(err)
    }
    callback(null, results)
  })
}

/**
 * 插入一个话题
 * @param  {Object}   topic    话题对象
 * @param  {Function} callback 回调函数（返回值都是通过回调函数来接收）
 * @return {undefined}            没有返回值
 */
exports.create = (topic, callback) => {
  const sqlStr = 'INSERT INTO `topics` SET ?'
  db.query(
    sqlStr,
    topic,
    (err, results) => {
      if (err) {
        return callback(err)
      }
      callback(null, results)
    }
  )
}

/**
 * 根据话题id更新话题内容
 * @param  {Object}   topic    要更新话题对象
 * @param  {Function} callback 回调函数
 * @return {undefined}            没有返回值
 */
exports.updateById = (topic, callback) => {
  const sqlStr = 'UPDATE `topics` SET `title`=?, `content`=? WHERE `id`=?'
  db.query(
    sqlStr,
    [
      topic.title,
      topic.content,
      topic.id
    ],
    (err, results) => {
      if (err) {
        return callback(err)
      }
      callback(null, results)
    }
  )
}

/**
 * 根据话题id删除某个话题
 * @param  {Number}   id       话题id
 * @param  {Function} callback 回调函数
 * @return {undefined}            没有返回值
 */
exports.deleteById = (id, callback) => {
  const sqlStr = 'DELETE FROM `topics` WHERE `id`=?'
  db.query(
    sqlStr,
    [
      id
    ],
    (err, results) => {
      if (err) {
        return callback(err)
      }
      callback(null, results)
    }
  )
}
```

`index.html`

```html
   <div class="media-body">
        <h4 class="media-heading"><a href="/topic/{{ $value.id }}">{{ $value.title }}</a></h4>
        <p>sueysok 回复了问题 • 2 人关注 • 1 个回复 • 187 次浏览 • {{ $value.createdAt }}</p>
   </div>
```

`models/topic.js`

```js
// 我们把话题相关的数据库操作方法都封装到当前模块
const db = require('../controllers/db-helper')
/**
 * 获取所有话题列表
 * @param  {Function} callback 回调函数
 * @return {undefined}            没有返回值
 */
exports.findAll = callback => {
  const sqlStr = 'SELECT * FROM `topics` ORDER BY `createdAt` DESC' // 降序排序
  db.query(sqlStr, (err, results) => {
    if (err) {
      return callback(err)
    }
    callback(null, results)
  })
}
// 
```

#### 2.3 查看话题

##### 2.3.1 渲染话题详情页

`router.js`

```js
// 话题相关
router
  .get('/topic/create', topic.showCreate)
  .post('/topic/create', topic.create)
  // 这是一个动态的路径标识
  // 什么意思？
  // /topic/:topicId 可以匹配 /topic/*
  // /topic/a
  // /topic/b
  // /topic/c
  // /topic/123
  .get('/topic/:topicId', topic.show)
  .get('/topic/:topicId/edit', topic.showEdit)
  .post('/topic/:topicId/edit', topic.edit)
  .get('/topic/:topicId/delete', topic.delete)
```

`models/topic.js`

```js
/**
 * 根据id获取一个话题
 * @param  {Number}   id       话题id
 * @param  {Function} callback 回调函数
 * @return {undefined}            没有返回值
 */
exports.findById = (id, callback) => {
  const sqlStr = 'SELECT * FROM `topics` WHERE `id`=?'
  db.query(sqlStr, [id], (err, results) => {
    if (err) {
      return callback(err)
    }
    callback(null, results[0])
  })
}
```

`controllers/topic.js`

​	[markdown->html](https://github.com/markedjs/marked)

```js
exports.show = (req, res, next) => {
  // 我们可以通过 req.query 来获取查询字符串中的参数
  // const {id} = req.query
  const {topicId} = req.params
  
  // req.params 用来获取动态的路径参数
  // req.query 用来获取查询字符串 ?xxx
  // console.log('req.params === ', req.params)
  // console.log('req.query === ', req.query)

  Topic.findById(topicId, (err, topic) => {
    if (err) {
      return next(err)
    }
    // 在渲染之前把话题的内容 markdown 格式的字符串转成 html 格式字符串
    topic.content = marked(topic.content)
    res.render('topic/show.html', {
      topic
    })
  })
}
```

`views/topic/show.html`

```html
  {{ if !topic }}
  <p>此话题不存在或已被删除。</p>
  <a class="btn btn-success" href="/">返回首页</a>
  {{ else }} 
<article class="markdown-body">
        <h1>{{ topic.title }}</h1>
        {{ if topic.userId === sessionUser.id }}
        <a href="/topic/{{ topic.id }}/edit">编辑</a>
        <a href="/topic/{{ topic.id }}/delete" id="delete_topic">删除</a>
  {{ /if }}
        <hr>
        <!-- 
          art-template 为了安全起见，默认是不渲染 HTML 格式的文件，它会帮你原样输出
          如果你希望能输出渲染 HTML，则需要使用一种特殊的模板语法。
         -->
        {{@ topic.content }}
 </article>
```

#### 2.4  公共的模板成员

`使用 `app.locals` 结合中间件挂载公共的模板数据`

`app.js`

```js
// 前面的代码略...

// app 有一个 locals 属性对象
// app.locals 属性对象中的成员可以直接在页面模板中访问
// 我们可以把一些公共的成员，多个页面都需要的模板成员放到 app.locals 中
// 我们每个页面都需要session中的user
// 所以我们就把这个 session 中的 user 添加到 app.locals 中
// 这个中间件的职责就是往 app.locals 中添加公共的模板成员
// 注意：一定要在配置 session 中间件之后，和挂载路由之前
app.use((req, res, next) => {
  app.locals.sessionUser = req.session.user

  // 千万不要忘记调用 next()，否则请求进来就不往后走了
  next()
})
// 后面的代码略...
```



#### 2.4 删除话题

` views/topic/show.html`

```js
// 处理客户端的交互
<script>
    $('#delete_topic').on('click', handleDelete)
    function handleDelete (e) {
      e.preventDefault()
      if (!window.confirm('Are you sure?')) {
        return
      }

      var url = $(this).attr('href')

      $.ajax({
        url: url,
        type: 'get',
        dataType: 'json',
        success: function (data) {
          if (data.code === 200) {
            window.location.href = '/'
          }
        }
      })
    }
  </script>
```

`controllers/topic.js`

```js
exports.delete = (req, res, next) => {
  // 1. 得到要删除的话题 id
  // 2. 执行删除操作
  // 3. 发送响应
  const {topicId} = req.params
  Topic.deleteById(topicId, (err, results) => {
    if (err) {
      return next(err)
    }
    res.send({
      code: 200,
      message: '删除成功'
    })
  })
}
```



#### 2.5 编辑话题

##### 2.5.1 渲染编辑页面

##### `router.js`

`controllers/topic.js`

```js
exports.edit = (req, res, next) => {
  const {topicId} = req.params // 要修改的话题 id
  const body = req.body // 要修改的话题内容（title、content）
  Topic.updateById(topicId, body, (err, results) => {
    if (err) {
      return next(err)
    }
    res.send({
      code: 200,
      message: '修改话题成功'
    })
  })
}
```

`views/edit.html`

```html
 <div class="col-md-5">
      {{ if !topic }}
      编辑的话题已不存在！
      {{ else if topic.userId !== sessionUser.id }}
      你没有权限执行该操作！
      {{ else }}
      <form id="form" action="/topic/{{ topic.id }}/edit">
        <!-- 表单隐藏域 -->
        <input type="hidden" id="topic_id" value="{{ topic.id }}">
        <div class="form-group">
          <label for="title">标题</label>
          <input type="text" class="form-control" id="title" name="title" value="{{ topic.title }}">
        </div>
        <div class="form-group">
          <label for="content">内容</label>
          <textarea class="form-control" id="content" name="content" rows="10">{{ topic.content }}</textarea>
        </div>
        <button type="submit" class="btn btn-default">提交</button>
      </form>
      {{ /if }}
    </div>
```

##### 2.5.2 处理表单提交

`views/edit.html`

```js
<script>
    $('#form').on('submit', handleSubmit)

    function handleSubmit(e) {
      e.preventDefault()
      var formData = $(this).serialize()
      var url = $(this).attr('action') // /topic/xx/edit
      $.post(url, formData, function (data) {
        if (data.code === 200) {
          // 修改成功，重新回到话题的详情页
          window.location.href = '/topic/' + $('#topic_id').val()
        }
      })
    }
  </script>
```

##### 2.5.3 处理服务端逻辑

`model/topic.js`

```js
/**
 * 根据话题id更新话题内容
 * @param  {Number|String}   id    话题id
 * @param  {Object}   topic    要更新话题对象
 * @param  {Function} callback 回调函数
 * @return {undefined}            没有返回值
 */
exports.updateById = (id, topic, callback) => {
  const sqlStr = 'UPDATE `topics` SET `title`=?, `content`=? WHERE `id`=?'
  db.query(
    sqlStr,
    [
      topic.title,
      topic.content,
      id
    ],
    (err, results) => {
      if (err) {
        return callback(err)
      }
      callback(null, results)
    }
  )
}
```

#### 2.6 抽取中间件进行校验

`router.js`

```js
// 首页路由
router
  .get('/', index.showIndex)

// 用户路由
router
  .get('/signin', user.showSignin)
  // 当你 POST /signin 的时候，先调用 checkSigninBody 中间件，校验通过才真正的执行 signin 中间件
  .post('/signin', checkSigninBody, user.signin)
  .get('/signup', user.showSignup)
  .post('/signup', user.signup)
  .get('/signout', user.signout)

function checkSigninBody (req, res, next) {
  Joi.validate(req.body, { // 基本数据校验
    email: Joi.string().email(),
    password: Joi.string().regex(/^[a-zA-Z0-9]{3,30}$/)
  }, (err, value) => {
    if (err) { // 判断客户端发送的数据是否有错误
      res.send({
        code: 400,
        message: err.details
      })
    } else {
      next()
    }
  })
}
```

------



## 总结

#### 1 中间件

- 思想:分治的思想,重用和方便维护,非常灵活
- 语法:组成/分类/应用
- 规则
- 场景

#### 2 在 Express 中获取客户端请求参数的三种方式

例如，有一个地址：`/a/b/c?foo=bar&id=123`

##### 2.1 查询字符串参数

获取 `?foo=bar&id=123`

```js
console.log(req.query)
```

结果如下：

```js
{
  foo: 'bar',
  id: '123'
}
```

##### 2.2 请求体参数

`POST` 请求才有请求体，我们需要单独配置 `body-parser` 中间件才可以获取。 只要程序中配置了 `body-parser`中间件，我们就可以通过 `req.body` 来获取表单 `POST` 请求体数据。

```js
req.body
// => 得到一个请求体对象
```

##### 2.3 动态的路径参数

在 Express 中，支持把一个路由设计为动态的。例如：

```js
// /users/:id 要求必须以 /users/ 开头，:id 表示动态的，1、2、3、abc、dnsaj 任意都行
// 注意：:冒号很重要，如果你不加，则就变成了必须 === /users/id
// 为啥叫 id ，因为是动态的路径，服务器需要单独获取它，所以得给它起一个名字
// 那么我们就可以通过 req.params 来获取路径参数
app.get('/users/:id', (req, res, next) => {
  console.log(req.params.id)
})

// /users/*/abc
// req.params.id
app.get('/users/:id/abc', (req, res, next) => {
  console.log(req.params.id)
})

// /users/*/*
// req.params.id
// req.params.abc
app.get('/users/:id/:abc', (req, res, next) => {
  console.log(req.params.id)
})

// /*/*/*
// req.params.users
app.get('/:users/:id/:abc', (req, res, next) => {
  console.log(req.params.id)
})

// /*/id/*
app.get('/:users/id/:abc', (req, res, next) => {
  console.log(req.params.id)
})
```

