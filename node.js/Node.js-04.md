

#### 1 回调函数

##### 1.0 回调函数使用场景

​	记住： 当需要得到一个函数中的异步操作结果的时候，我们就必须使用回调函数了

- 定时器
- ajax
- readFile、writeFile

##### 1.1 回调函数的基本使用

- 想要获取一个函数中异步操作的结果：

  ```js
  function fn () {
    setTimeout(function () {
      // 我想调用 fn 得到这里的 num
      const num = 123
    }, 1000)
  }
  ```

- 想法

  ```js
  function fn () {
    console.log(1)
    setTimeout(function () {
      console.log(2)
      // 我想调用 fn 得到这里的 num
      const num = 123
      // 从返回值角度来讲，这里的 return 也只是返回给了当前的函数，而非外部的 fn 函数
      return num
    }, 1000)
    // 到这里 fn 函数就执行结束了，所以不可能得到里面 return 的结果
    console.log(3)
  }
  ```


- 正确的方式（通过函数来接收异步操作结果，这就是回调函数，因为不是立即调用，而是回头再调用）：

  ```js
  // 2. 在 fn 函数中通过形参 callback 接收了 handler 函数
  function fn (callback) {
    // var callback = handler
    // callback 就是我们的 handler
    console.log(1, '函数开始执行')

    // 定时器是异步的，所以遇到定时器不会等待，函数会继续往后执行
    setTimeout(function () {
      console.log(2)

      // 我想调用 fn 得到这里的 num
      const num = 123

      // 定时器操作结束，我们就可以在这里调用 callback（也就是我们的 handler）函数，把结果 num 传递给了该函数
      // 我们这里调用 callback 也就是在调用 handler
      callback(num)
    }, 1000)

    // 到这里 fn 函数就执行结束了（定时器还没有被调用），所以你拿到的 num 就是 undefined
    console.log(3)
  }

  function handler = function (data) {
    console.log('handler 函数被调用了：', data)
  }

  // 1. 这里把 handler 传递到了 fn 函数中
  fn(handler)
  ```

- 上面的方式比较繁琐，我们没必要单独定义一个全局函数，我们可以可以在调用的时候直接传递一个匿名函数即可：

  ```js
  function fn (callback) {
    setTimeout(function () {
      const num = 123
      callback(num)
    })
  }

  fn(function (data) {
    console.log('回调函数被执行了：', data)
  })
  ```

##### 1.2 封装ajax操作

#### 2 回掉函数应用

##### 2.1 封装ajax

```js
// $.get
    // $.post
    // $.ajax
    get('package.json', function (data) {
      console.log('get 方法请求成功了')
      console.log(data)
    })

    // 我需要得到 get 方法内部的 ajax 请求响应结果
    function get(url, callback) {
      
      var oReq = new XMLHttpRequest();

      // 设置请求方法及请求路径
      oReq.open("get", url, true);

      // 设置响应成功的回调函数
      oReq.onreadystatechange = function () {
       	  if (oReq.readyState === 4 && oReq.status === 200) {
              callback(oReq.responseText)
          }
      };
      // 发起请求
      oReq.send();
    }
```

##### 2.2 封装读取文件

```js
// 封装一个方法，调用该方法得到 package.json 文件结果对象

const fs = require('fs')

// 将二进制转为字符串有两种方式：
//  1. 二进制数据.toString()
//  2. 给 readFile 提供第二个可选参数编码 utf8


// 调用 getPackage() 得到 data.json 文件中 todos 数组
getTodos(function (err, data) {
  // 如果 getTodos 失败了，我就要换另一种解决方案了
  if (err) {
    console.log('获取 todos 失败了')
  } else {
    // 没有错误，可以放心大胆的使用 todos 了
    console.log('获取 todos 成功，')
    console.log(data)
  }
})
// 另一个问题:
// 如果封装的异步函数中遇到错误怎么办？
// 1. 不要在自己封装的异步函数中自行处理错误
//    因为调用者不知道
//    我们需要让调用者知道错误的发生，由调用者来解决如何处理
// 2. 如何让调用者知道错误的发生
//    还是通过回调函数，通知调用者
//    把错误信息传递给回调函数
//    在 Node 中通常采用错误优先的方式来向外抛出错误
//    Error First 把错误始终作为第一个参数
function getTodos(callback) {
  // var callback = function (data) {
  //   console.log(data)
  // }
  fs.readFile('./data.json', 'utf8', (err, data) => {
    if (err) {
      // throw err
      // 失败了，也要调用 callback ，通知调用者，任务失败了
      // callback(err)
      callback(err, null)
    } else {
      const todos = JSON.parse(data).todos
      // callback(todos)
      callback(null, todos)
    }
  })
}
```

##### 2.3 封装写入文件

```js
// 封装一个方法，该方法最少接收一个参数：todo 对象
// 将 todo 对象持久化存储到 data.json 文件中的 todos 数组中
const fs = require('fs')
saveTodo({
  title: '测试',
  done: true
}, function (err) {
  if (err) {
    console.log('保存 todo 失败了')
  } else {
    console.log('保存 todo 成功了')
  } 
})

function saveTodo(todo, callback) {
  fs.readFile('./data.json', 'utf8', (err, data) => {
    // if (err) {
    //   callback(err)
    // } else {
    // }
    if (err) {
      // callback(err)
      // // 这里的 return 是用来阻止代码继续执行的
      // return

      return callback(err)
    }

    data = JSON.parse(data)
    data.todos.push(todo)

    data = JSON.stringify(data)

    fs.writeFile('./data.json', data, err => {
      if (err) {
        return callback(err)
      }
      // 代码执行到这里，意味着数据存储成功了
      // 然后通知调用者，操作成功了
      callback()
    })
  })
}
```

##### 2.4 封装拷贝文件

```js
// 请封装一个方法，实现文件拷贝

const fs = require('fs')

copy('./data.json', './abc.json', err => {
  if (err) {
    return console.log('复制文件操作失败了')
  }
  console.log('复制文件操作成功了')
})

// data.json abc.json
// 读出来，写进去
function copy(src, dest, callback) {
  fs.readFile(src, (err, data) => {
    if (err) {
      return callback(err)
    }
    fs.writeFile(dest, data, err => {
      if (err) {
        return callback(err)
      }
      callback()
    })
  })
}
```

### 总结

#### 1  回调函数

- 封装自定义异步函数
  - 回调函数
- 如何得到一个函数中异步操作的结果？
  - 回调函数
- 异步操作中的错误如何处理？
  - 不要自行处理
  - 也是通过回调函数来传递错误
  - 错误优先：Error First







