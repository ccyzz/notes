

# JavaScript 高级

### 介绍 JavaScript

#### JavaScript 是什么

- 解析执行：轻量级解释型的 

  - 解释执行语言： JavaScript、PHP    
    - 每一行代码都要先解释再执行。特点： 慢，灵活

  ```js
  var a = 10;
  var b = 20;
  console.log(a, b);
  ```

  - 编译执行语言：  C#、 Java         
    - 一次性编译，然后再一行一行执行。特点：快

- 语言特点：动态，头等函数 (First-class Function)
  + 又称函数是 JavaScript 中的一等公民

- 执行环境：在宿主环境（host environment）下运行，浏览器是最常见的 JavaScript 宿主环境
  + 但是在很多非浏览器环境中也使用 JavaScript ，例如 node.js

  [MDN-JavaScript](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript)

#### JavaScript 的组成

- ECMAScript  - 语法规范
  - 变量、数据类型、类型转换、操作符
  - 流程控制语句：判断、循环语句
  - 数组、函数、作用域、预解析
  - 对象、属性、方法、简单类型和复杂类型的区别
  - 内置对象：Math、Date、Array，基本包装类型String、Number、Boolean
- Web APIs
  - BOM
    - onload页面加载事件，window顶级对象
    - 定时器
    - location、history
  - DOM
    - 获取页面元素，注册事件
    - 属性操作，样式操作
    - 节点属性，节点层级
    - 动态创建元素
    - 事件：注册事件的方式、事件的三个阶段、事件对象 

#### JavaScript 可以做什么

> 阿特伍德定律：r
>
> Any application that can be written in JavaScript, will eventually be written in JavaScript.  
>
> 任何可以用*JavaScript*来写的应用，最终都将用*JavaScript*来写。阿特伍德 stackoverflow的创始人之一
>

- [知乎 - JavaScript 能做什么，该做什么？](https://www.zhihu.com/question/20796866)
- [最流行的编程语言 JavaScript 能做什么？](https://github.com/phodal/articles/issues/1)

### 浏览器是如何工作的

![img](media/jszxgc.png)

[参考链接](http://www.2cto.com/kf/201202/118111.html)

```
User Interface  用户界面，我们所看到的浏览器
Browser engine  浏览器引擎，用来查询和操作渲染引擎
*Rendering engine 用来显示请求的内容，负责解析HTML、CSS，并把解析的内容显示出来。
				负责把HTML文档解释成DOM树
Networking   网络，负责发送和接收网络请求
*JavaScript Interpreter(解析者)   JavaScript解析器，负责执行JavaScript的代码
UI Backend   UI后端，用来绘制类似组合框和弹出窗口
Data Persistence(持久化)  数据持久化，数据存储  cookie、HTML5中的sessionStorage
```

---

## JavaScript 面向对象编程

<img src="./media/mxdxkf.png" width="400" alt="">

### 面向对象介绍

#### 什么是对象

> Everything is object （万物皆对象）

<img src="./media/20160823024542444.jpg" alt="">

对象到底是什么，我们可以从两次层次来理解。

- **现实世界中的对象**

  对象是**单个**事物的抽象。

  - **具体的物**：一本书、一辆汽车、一个人都是对象。
  - **抽象的事**：一笔交易、一个与远程服务器的连接也可以是对象。

  当事物被抽象成对象，事物之间的关系就变成了对象之间的关系，从而就可以模拟现实情况，**针对对象进行编程**。

- **程序世界中的对象**

  对象是一个容器，封装了属性（property）和方法（method）。

  - 属性是对象的状态，方法是对象的行为（完成某种任务）。
    - 比如：我们可以把动物抽象为animal对象，使用“属性”记录具体是那一种动物，使用“方法”表示动物的某种行为（奔跑、捕猎、休息等等）。
  - 对象是一个抽象的概念，可以将其简单理解为：**数据集或功能集**。
  - ECMAScript-262 把对象定义为：**无序属性的集合，其属性可以包含基本值、对象或者函数**。

  > 提示：每个对象都是基于一个引用类型创建的，这些类型可以是系统内置的原生类型，也可以是开发人员自定义的类型。


#### 什么是面向对象

> 面向对象不是新的东西，它只是过程式代码的一种高度封装，目的在于**提高**代码的**开发效率**和**可维护性**。

- 面向对象编程

   Object Oriented Programming，简称 OOP ，是一种编程开发思想。它将真实世界各种复杂的关系，抽象为一个个对象，然后由对象之间的分工与合作，完成对真实世界的模拟。

- 在面向对象程序开发思想中，每一个对象都是功能中心，具有**明确分工**，可以完成接受信息、处理数据、发出信息等任务。**对象要职责分明**

>  因此，面向对象编程具有灵活、代码可复用、高度模块化等特点，容易维护和开发，比起由一系列函数或指令组成的传统的过程式编程（procedural programming），更**适合多人合作的大型软件项目**。

面向对象与面向过程： 

- 面向过程就是亲力亲为，事无巨细，面面俱到，步步紧跟，有条不紊
- 面向对象就是找一个对象，指挥得结果
- 面向对象将执行者转变成指挥者
- **面向对象不是面向过程的替代，而是面向过程的封装**

**面向对象的特性**：

- 封装性 
- 继承性
- [多态性]抽象

扩展阅读：

- [维基百科 - 面向对象程序设计](https://zh.wikipedia.org/wiki/%E9%9D%A2%E5%90%91%E5%AF%B9%E8%B1%A1%E7%A8%8B%E5%BA%8F%E8%AE%BE%E8%AE%A1)
- [知乎：如何用一句话说明什么是面向对象思想？](https://www.zhihu.com/question/19854505)
- [知乎：什么是面向对象编程思想？](https://www.zhihu.com/question/31021366)

**小结：**

1. 对象是无序属性的集合
2. 面向对象是面向过程的封装，适合多人合作的大型项目(页面特效一般不需要)。面向对象是学习编程的一个必经阶段。

#### 通过案例演示面向过程和面向对象的区别

在 JavaScript 中，所有数据类型都可以视为对象，当然也可以自定义对象。
自定义的对象数据类型就是面向对象中的类（ Class ）的概念。

**需求： **假设我们要打印学生的成绩表。

##### 面向过程的方式

1. 为了记录一个学生的成绩，面向过程的程序可以用一个对象表示(此处的对象仅仅是用来存储数据)
2. 打印学生成绩通过函数实现(封装一段代码重复使用)
3. 调用函数实现功能

```javascript
// 1. 记录学生成绩
var std1 = { name: 'Michael', score: 98 };
var std2 = { name: 'Bob', score: 81 };
// 2. 封装函数打印成绩
function printScore (student) {
  console.log('姓名：' + student.name + '  ' + '成绩：' + student.score);
}
// 3. 调用函数
printScore(std1);
printScore(std2);
```

**小结：**整个思路是按照执行的流程(步骤)来实现

##### 面向对象的方式

1. 面向对象的设计思路，根据需求分析对象。学生对象 记录自己的姓名和成绩
2. 分析对象具有的属性和方法. 属性：name和score，方法：打印成绩printScore
3. 创建对象，调用方法

抽象数据行为模板（Class）：

```javascript
// 自定义的数据类型
function Student(name, score) {
  this.name = name;
  this.score = score;
  this.printScore = function() {
    console.log('姓名：' + this.name + '  ' + '成绩：' + this.score);
  }
}
```

根据模板创建具体实例对象（Instance）：

```javascript
var std1 = new Student('Michael', 98)
var std2 = new Student('Bob', 81)
```

实例对象具有自己的具体行为（给对象发消息）：

```javascript
std1.printScore() // => 姓名：Michael  成绩：98
std2.printScore() // => 姓名：Bob  成绩 81
```

**小结：**面向对象的设计思想

- 抽象出 Class(构造函数)
- 根据 Class(构造函数) 创建 Instance
- 指挥 Instance 得结果

面向对象的抽象程度又比函数要高，因为一个 Class 既包含数据，又包含操作数据的方法。

### 创建对象

#### 简单方式

-  `new Object()` 创建

```javascript
var person = new Object()
person.name = 'Jack'
person.age = 18

person.sayName = function () {
  console.log(this.name)
}
```

- 对象字面量创建

```javascript
var person = {
  name: 'Jack',
  age: 18,
  sayName: function () {
    console.log(this.name)
  }
}
```

对于上面的写法固然没有问题，但是假如我们要生成两个 `person` 实例对象呢？

```javascript
var person1 = {
  name: 'Jack',
  age: 18,
  sayName: function () {
    console.log(this.name)
  }
}

var person2 = {
  name: 'Mike',
  age: 16,
  sayName: function () {
    console.log(this.name)
  }
}
```

通过上面的代码我们不难看出，这样写的代码太过冗余，重复性太高。

### 构造函数

内容引导：

- 构造函数语法
- new的执行过程
- 构造函数和实例对象的关系
  + 实例的 constructor 属性
  + instanceof 操作符
- 普通函数调用和构造函数调用的区别
- 构造函数的问题

#### 构造函数

- 构造函数是创建对象的**模板。**

- 构造函数通过**new关键**调用。

  我们可以认为构造函数是一种自定义的类型，通过构造函数可以创建自定义的对象。

```javascript
function Person (name, age) {
  this.name = name;
  this.age = age;
  this.sayName = function () {
    console.log(this.name);
  };
}
var p1 = new Person('Jack', 18);
p1.sayName(); // => Jack
var p2 = new Person('Mike', 23)
p2.sayName(); // => Mike
```

#### new操作符的执行过程

- new操作符执行的4个步骤：
  1. 在内存中创建一个空对象
  2. 将构造函数内部的this执行新创建的对象
  3. 执行构造函数中的代码
  4. 返回新对象

下面是具体的伪代码：

```javascript
function Person (name, age) {
  // 当使用 new 操作符调用 Person() 的时候，实际上这里会先创建一个对象
  // var instance = {}
  // 然后让内部的 this 指向 instance 对象
  // this = instance
  // 接下来所有针对 this 的操作实际上操作的就是 instance
  this.name = name;
  this.age = age;
  this.sayName = function () {
    console.log(this.name);
  };
  // 在函数的结尾处会将 this 返回，也就是 instance
  // return this
}
```

#### 构造函数和对象(实例)的关系

- 在每一个实例对象中都有一个 `constructor` 属性，该属性指向创建该实例的构造函数：
- 对象的 `constructor` 属性最初是用来标识对象类型的

```javascript
console.log(p1.constructor === Person); // => true
console.log(p2.constructor === Person); // => true
console.log(p1.constructor === p2.constructor); // => true
```

- 如果要检测对象的类型，使用 `instanceof` 操作符更可靠一些：

```javascript
console.log(p1 instanceof Person); // => true
console.log(p2 instanceof Person); // => true
```

**小结：**

- 构造函数是根据具体的事物抽象出来的抽象**模板**
- 实例对象是根据抽象的构造函数模板得到的具体实例对象
- 每一个实例对象都具有一个 `constructor` 属性，指向创建该实例的构造函数
  + 注意： `constructor` 是实例的属性的说法不严谨，具体后面的原型会讲到
- 可以通过实例的 `constructor` 属性判断实例和构造函数之间的关系
  + 注意：这种方式不严谨，推荐使用 `instanceof` 操作符，后面学原型会解释为什么

#### 实例成员和静态成员

- 实例成员

  属于对象的成员，每一个对象都具有自己的成员，但是每个对象具有的这个成员的值是不同的。

  例如：p.name。通过Person构造函数创建的对象的name属性的值是不同的

- 静态成员

  属于构造函数的成员

  Person.verson = '1.0';  

#### 普通函数和构造函数调用的区别

- 普通函数直接调用，构造函数需要通过new调用

- 普通函数需要返回值的时候return，构造函数不需要自己写return

  构造函数调用的时候return会被忽略

#### 构造函数的问题

使用构造函数带来的最大的好处就是创建对象更方便了，避免代码的重复。

但是其本身也存在一个浪费内存的问题

1. 使用构造函数创建多个对象，每一个对象都会在内存中存储一个方法，浪费内存
2. 多个对象使用相同的属性也会浪费内存

```javascript
function Person (name, age) {
  this.name = name;
  this.age = age;
  this.type = 'human';
  this.sayHello = function () {
    console.log('hello ' + this.name);
  }
}

var p1 = new Person('Tom', 18);
var p2 = new Person('Jack', 16);
```

那就是对于每一个实例对象，`type` 和 `sayHello` 都是一模一样的内容，
每一次生成一个实例，都必须为重复的内容，多占用一些内存，如果实例对象很多，会造成极大的内存浪费。

```javascript
console.log(p1.sayHello === p2.sayHello); // => false
```

对于这种问题我们可以把需要共享的函数定义到构造函数外部：

```javascript
function sayHello() {
  console.log('hello ' + this.name);
}

function Person (name, age) {
  this.name = name;
  this.age = age;
  this.type = 'human';
  this.sayHello = sayHello;
}

var p1 = new Person('Top', 18)
var p2 = new Person('Jack', 16)

console.log(p1.sayHello === p2.sayHello) // => true
```

这样确实可以了，但是如果有多个需要共享的函数的话就会造成全局命名空间冲突的问题。

#### 小结

- 构造函数语法
- new执行的四个步骤
- 构造函数和实例对象的关系
  + 实例的 constructor 属性
  + instanceof 操作符
- 构造函数的问题

### 原型

JavaScript 规定，每一个**(构造)函数**都有一个 **prototype** 属性，指向另一个对象。**这个对象的所有属性和方法，都会被构造函数的对象所访问**。

- **构造函数的原型**

  我们可以把所有对象实例需要共享的属性和方法直接定义在 `prototype` 对象上。所有通过构造函数创建的对象共享原型对象上的属性和方法。

```javascript
function Person (name, age) { 
}

console.log(Person.prototype)

Person.prototype.type = 'human'

Person.prototype.sayName = function () {
  console.log(this.name)
}

var p1 = new Person(...)
var p2 = new Person(...)

console.log(p1.sayName === p2.sayName) // => true
```

这时所有实例的 `type` 属性和 `sayName()` 方法，
其实都是同一个内存地址，指向 **prototype** 对象，因此就提高了运行效率。

- **对象的原型**

  每一个对象都有一个**\_\_proto\_\_**的属性，此属性指向构造函数的**prototype**

  > **注意**：`__proto__` 是非标准属性。

#### 构造函数、实例、原型三者之间的关系

<img src="./media/构造函数-实例-原型之间的关系.png" alt="">

任何函数都具有一个 **prototype** 属性，该属性是一个对象。

```javascript
function F () {}
console.log(F.prototype) // => object

F.prototype.sayHi = function () {
  console.log('hi!')
}
```

构造函数的 `prototype` 对象默认都有一个 **constructor** 属性，指向 **prototype** 对象所在函数。

```javascript
console.log(F.prototype.constructor === F) // => true
```

通过构造函数得到的实例对象内部会包含一个指向构造函数的 `prototype` 对象的指针 `__proto__`。

```javascript
var instance = new F()
console.log(instance.__proto__ === F.prototype) // => true
```

实例对象可以直接访问原型对象成员。

```javascript
instance.sayHi() // => hi!
```

总结：

- **任何函数**都具有一个 `prototype` 属性，该属性是一个对象
- `prototype` 对象默认都有一个 `constructor` 属性，指向 `prototype` 对象所在函数
- 通过构造函数得到的实例对象内部会包含一个指向构造函数的 `prototype` 对象的指针 `__proto__`
- 所有实例都直接或间接继承了原型对象的成员

#### 属性成员的搜索原则：原型链

对象是如何访问构造函数的原型对象上的成员的？

- 访问对象的某个成员的时候，首先搜索对象实例本身的成员
- 如果在实例中找到了具有给定名字的成员，则返回(调用)该成员
- 如果没有找到，则继续搜索指针指向的原型对象，在原型对象中查找具有给定名字的成员
- 如果在原型对象中找到了这个成员，则返回(调用)该成员

也就是说，在我们调用 `person1.sayName()` 的时候，会先后执行两次搜索：

- 首先，解析器会问：“实例 person1 有 sayName 属性吗？”答：“没有。
- ”然后，它继续搜索，再问：“ person1 的原型有 sayName 属性吗？”答：“有。
- ”于是，它就读取那个保存在原型对象中的函数。
- 当我们调用 person2.sayName() 时，将会重现相同的搜索过程，得到相同的结果。

而这正是多个对象实例共享原型所保存的属性和方法的基本原理。

总结：

- 先在自己身上找，找到即返回
- 自己身上找不到，则沿着原型链向上查找，找到即返回
- 如果一直到原型链的末端还没有找到
  - 如果是属性则返回 `undefined`
  - 如果是方法会报错

#### 实例对象读写原型对象成员

读取：

- 先在自己身上找，找到即返回
- 自己身上找不到，则沿着原型链向上查找，找到即返回
- 如果一直到原型链的末端还没有找到
  - 如果是属性则返回 `undefined`
  - 如果是方法会报错

写入：

- 当实例期望重写原型对象中的某个普通数据成员时实际上会把该成员添加到自己身上
- 也就是说该行为实际上会屏蔽掉对原型对象成员的访问

#### 更简单的原型语法

我们注意到，前面例子中每添加一个属性和方法就要敲一遍 `Person.prototype` 。

简化操作给原型对象设置一个对象字面量来增加成员。

```javascript
function Person (name, age) {
  this.name = name
  this.age = age
}
Person.prototype = {
  type: 'human',
  sayHello: function () {
    console.log('我叫' + this.name + '，我今年' + this.age + '岁了')
  }
}
```

我们将 `Person.prototype` 重置到了一个新的对象。
这样做的好处就是为 `Person.prototype` 添加成员简单了，但是也会带来一个问题，那就是原型对象丢失了 `constructor` 成员。

所以，我们为了保持 `constructor` 的指向正确，建议的写法是：

```javascript
function Person (name, age) {
  this.name = name
  this.age = age
}

Person.prototype = {
  constructor: Person, // => 手动将 constructor 指向正确的构造函数
  type: 'human',
  sayHello: function () {
    console.log('我叫' + this.name + '，我今年' + this.age + '岁了')
  }
}
```

#### 原生对象的原型

  所有函数都有 prototype 属性对象。

- Object.prototype
- Function.prototype
- Array.prototype
- String.prototype
- Number.prototype
- Date.prototype
- ...

练习：为数组对象和字符串对象扩展原型方法。

> **注意**：内置对象的**prototype**属性(只读属性)不能够直接复制，否则内置的成员丢失

#### 原型对象使用建议

- 属性（一般就是非函数成员）放到构造函数中
- 方法（一般就是函数）放到原型对象中
- 如果重置了 `prototype` 记得修正 `constructor` 的指向

### 案例：随机方块游戏

- 分析对象：方块对象 Box

- 分析属性和方法

  - 属性 
    - left、top  方块的坐标
    - size         方块的大小（方块是正方形的）
    - color        方块的颜色
    - _div           方块对应的div
  - 方法
    - random    随机生成位置

- 构造函数

  构造函数中设置方块的属性，创建div

  ```js
  function Box(map) {
    this.left = 0;
    this.top = 0;
    this.size = 20;
    this.color = 'red';

    this.div = document.createElement('div');
    map.appendChild(this.div);
    this.div.  = 'box';
  }
  ```

- 随机方块的位置

  1. 随机生成方块的坐标，范围在map中
  2. 随机生成方块的颜色
  3. 设置div的样式属性
  4. 构造函数中调用random
  5. 新建app.js创建方块进行测试

  ```js
  Box.prototype.random = function () {
    // 1. 随机生成坐标
    var maxX = 800 / this.size - 1;
    var maxY = 600 / this.size - 1;
    this.left = getRandom(0, maxX) * 20;
    this.top = getRandom(0, maxY) * 20;
    // 2. 随机生成颜色
    var r = getRandom(0, 255);
    var g = getRandom(0, 255);
    var b = getRandom(0, 255);
    this.color = 'rgb('+ r +', '+ g +', '+ b +')';
    // 3. 设置div的样式属性
    this.div.style.left = this.left + 'px';
    this.div.style.top = this.top + 'px';
    this.div.style.width = this.size + 'px';
    this.div.style.height = this.size + 'px';  
    this.div.style.backgroundColor = this.color;
  }
  ```

- 随机生成[min, max]之间的整数

  ```js
  function getRandom(min, max) {
    return parseInt(Math.random() * (max - min + 1)) + min;
  }
  ```

- 游戏主逻辑

  1. 创建方块对象
  2. 开启定时器，定时随机方块的位置
  3. 给map注册点击事件，如果点击到box会冒泡到map
     1. 判断触发事件的对象e.target的类样式是否是box

---

## 继承(知道)

- 后续react课程使用到了继承
- 方便以后跟其他程序员沟通

### 什么是继承

- 现实生活中的继承

- 对象的"继承"

  对象的“继承”本质是对象的拷贝，把一个对象的所有成员拷贝给另一个对象，通过遍历把一个对象的所有成员拷贝给另一个对象

```js
// 对象的拷贝
// 复制对象的成员给另一个对象
function extend(parent, child) {
  for (var key in parent) {
    // 不复制同名的属性
    if (child[key]) {
      continue;
    }
    child[key] = parent[key];
  }
}
```

### 程序中的继承

- 程序中的继承
  - 继承是类型和类型之间的关系
  - 子类型继承父类型的成员(把子类型中共同的成员提取到父类型中)
    - Student -> name, age , sex , birthday, score成绩
    - Teacher -> name, age, sex, birthday, salary工资
    - Person
  - 目的：**代码重用**
- JavaScript中的继承
  - JavaScript本身不像其它语言(Java、C#)具备继承的语法
  - JavaScript实现继承是通过其特殊的语法来实现的，其它程序员在学习JavaScript继承的时候都会有不适应的情况

### 原型继承 

- 继承：类型和类型之间的关系
- 案例：学生类型、老师类型  继承 Person类型

```js
// 父类型
function Person() {
  this.name = 'zs';
  this.age = 18;
  this.sex = '男';
}
Person.prototype.sayHi = function() {
  console.log('大家好，我是' + this.name);
}
// 子类型
function Student() {
  this.score = 100;
}
Student.prototype = new Person();
Student.prototype.constructor = Student;

var s1 = new Student();
console.log(s1.constructor);
```

> 注意：当设置了构造函数的prototype之后，别忘记设置constructor
>
> 问题：原型继承，无法设置构造函数的参数**Student.prototype = new Person();**只执行一次，无法给属性传值，可以方便的继承父类型的原型中的方法，但是属性的继承无意义

### 借用构造函数

- 前置知识 call，call的作用：

  1. 函数具有call方法
  2. 调用函数
  3. 改变函数中的this


```js
  function fn(x, y) {
    console.log(this);
    console.log(x + y);
  }
  var o = {
    name: 'zs'
  };
  // call()  改变函数中的this，直接调用函数
  fn.call(o, 2, 3);
```

- 借用构造函数

  借用构造函数：让子类型借用父类型的构造函数。

  可以方便的继承父类型的属性，但是无法继承原型中的方法

  ```javascript
  // 父类型
  function Person(name, age, sex) {
    this.name = name;
    this.age = age;
    this.sex = sex;
  }
  Person.prototype.sayHi = function () {
    console.log(this.name);
  }
  // 子类型
  function Student(name, age, sex, score) {
    Person.call(this, name, age, sex);
    this.score = score;
  }
  var s1 = new Student('zs', 18, '男', 100);
  console.dir(s1);
  ```

  > 借用构造函数继承的问题：无法继承方法

### 组合继承：原型继承+借用构造函数继承

```javascript
// 父类型
function Person(name, age, sex) {
  this.name = name;
  this.age = age;
  this.sex = sex;
}

Person.prototype.sayHi = function () {
  console.log('大家好，我是' + this.name);
}
// 子类型
function Student(name, age, sex, score) {
  // 借用构造函数
  Person.call(this, name, age, sex);
  this.score = score;
}
Student.prototype = new Person();
Student.prototype.constructor = Student;
// 学生特有的方法
Student.prototype.exam = function () {
  console.log('考试');
}
```

> **小结**：
>
> 继承：让子类型的所有对象具有父类型的成员
>
> 关于继承要记住下面的代码模式

```js
function Student(name, age) {
    // 继承属性
    Person.call(this, name, age);
    ....
}
// 继承方法
Student.prototype = new Person();
Student.prototype.constructor = Student;
```

Object是所有自定义类型的祖先

重新解释instanceof   

​	instanceof 判断的是Object.prototype是否出现在obj的原型链上

```js
obj instanceof Object
```



---

## 函数进阶

### 函数的定义方式

- 函数声明
- 函数表达式
- `new Function`

#### 函数声明

```javascript
function foo () {
}
```

#### 函数表达式

```javascript
var foo = function () {
};
```

#### **new Function**

```js
var fn = new Function('a', 'b', 'return a + b;');
var sum = fn(5, 6);
console.log(sum);
```

#### 函数声明与函数表达式的区别

- 函数声明必须有名字
- **函数声明会函数提升**，在预解析阶段就已创建，声明前后都可以调用
- 函数表达式类似于变量赋值，提升的仅仅是变量声明
- 函数表达式可以没有名字，例如匿名函数
- 函数表达式没有变量提升，在执行阶段创建，必须在表达式执行之后才可以调用

下面是一个根据条件定义函数的例子(了解)：

```javascript
if (true) {
  function f () {
    console.log(1);
  }
} else {
  function f () {
    console.log(2);
  }
}
```

以上代码执行结果在不同浏览器中结果不一致。有些浏览器里不会进行函数提升(Chrome)，有些浏览器中会函数提升。

不过我们可以使用函数表达式解决上面的问题：

```javascript
var f;

if (true) {
  f = function () {
    console.log(1)
  };
} else {
  f = function () {
    console.log(2)
  };
}
```

### 函数的调用方式

- 普通函数
- 构造函数
- 方法调用

### 函数内 `this` 指向的不同场景

**函数的调用方式决定**了 `this` 指向的不同：

| 调用方式   | 非严格模式   | 备注'use strict'    |
| ------ | ------- | ----------------- |
| 普通函数调用 | window  | 严格模式下是 undefined  |
| 构造函数调用 | 实例对象    | 原型方法中 this 也是实例对象 |
| 方法调用   | 该方法所属对象 | 紧挨着的对象            |
| 事件绑定方法 | 绑定事件对象  |                   |
| 定时器函数  | window  |                   |

这就是对函数内部 this 指向的基本整理，写代码写多了自然而然就熟悉了。

> **注意**：**函数内部的this，是由函数的调用方式决定的。**

### 函数也是对象

- 所有函数都是 `Function` 的实例

### 改变函数中的this

- 为什么要改变函数内部的this

  例如：我们经常在定时器外部备份 this 引用，然后在定时器函数内部使用外部 this 的引用。

- 函数有三个方法可以改变内部的this：call、apply、bind。

#### call

`call()` 方法**调用一个函数**, 其具有一个指定的 `this` 值和分别地提供的参数(参数的列表)。

语法：

```javascript
fun.call(thisArg[, arg1[, arg2[, ...]]])
```

参数：

- `thisArg`
  + 在 fun 函数运行时指定的 this 值
  + 如果指定了 null 或者 undefined 则内部 this 指向 window

- `arg1, arg2, ...`
  + 指定的参数列表

应用：

1. 借用构造函数 Person.call(this, ....)

2. 借用其他对象的方法

   ```js
   // 伪数组
   var obj = {
     0: 100,
     1: 10,
     2: 11,
     3: 20,
     length: 4
   };
   Array.prototype.push.call(obj, 30);
   Array.prototype.splice.call(obj, 0, 3);

   // 借用Object的toString()
   var arr = [5, 9];
   console.log(arr.toString());
   console.log(Object.prototype.toString.call(arr));
   ```

#### apply

`apply()` 方法**调用一个函数**, 其具有一个指定的 `this` 值，以及作为一个数组（或类似数组的对象）提供的参数。

语法：

```javascript
fun.apply(thisArg, [argsArray])
```

参数：

- `thisArg`
- `argsArray`

`apply()` 与 `call()` 非常相似，不同之处在于提供参数的方式。
`apply()` 使用参数数组而不是一组参数列表。例如：

```javascript
fun.apply(this, ['eat', 'bananas']);
```

应用：把数组展开

```js
// Math.max(3, 5, 6);
var arr = [5, 10, 1, 3, 6];
// Math.max不能求数组中的最大值
console.log(Math.max.apply(null, arr));
console.log(Math.max.apply(Math, arr));
// console.log(1, 2, 3);
// console.log(arr);
console.log.apply(null, arr);
console.log.apply(console, arr);
```

#### bind

- bind不会调用函数，会返回一个新的函数
- 新函数内部的this是bind的第一个参数
- 原函数中的参数，通过第二个参数传递

语法：

```javascript
fun.bind(thisArg[, arg1[, arg2[, ...]]])
```

参数：

- thisArg
  + 当绑定函数被调用时，该参数会作为原函数运行时的 this 指向。当使用new 操作符调用绑定函数时，该参数无效。

- arg1, arg2, ...
  + 当绑定函数被调用时，这些参数将置于实参之前传递给被绑定的方法。

返回值：

返回由指定的this值和初始化参数改造的原函数拷贝。

示例1：

```javascript
var obj = {
  name: 'zs',
  fun: function() {
    setInterval(function() {
      console.log(this.name);
    }.bind(this), 1000);
  }
}
obj.fun();
```

示例2：

```javascript
btn.onclick = function () {
  // 事件处理函数中的this  是触发该事件的对象
  // 通过bind 改变事件处理函数中this的指向
}.bind(obj);
```

#### 小结

- call 和 apply 特性一样
  + 都是用来调用函数，而且是**立即调用**
  + 但是可以在调用函数的同时，通过第一个参数**改变函数内部 `this` 的指向**
  + call 调用的时候，参数必须以**参数列表**的形式进行传递，也就是以逗号分隔的方式依次传递即可
  + apply 调用的时候，参数必须是一个**数组**，然后在执行的时候，会将数组内部的元素一个一个拿出来，与形参一一对应进行传递
  + 如果第一个参数指定了 **`null` 或者 `undefined`** 则内部 **this 指向 window**

- bind
  + 可以用来指定内部 this 的指向，然后生成一个**改变 this 指向**的**新的函数**
  + 它和 call、apply 最大的区别是：**bind 不会调用函数**
  + bind 支持传递参数，它的传参方式比较特殊，一共有两个位置可以传递
    * 1. 在 bind 的同时，以参数列表的形式进行传递
    * 2. 在调用的时候，以参数列表的形式进行传递
    * 那到底以谁 bind 的时候传递的参数为准呢还是以调用的时候传递的参数为准
    * 两者合并：bind 的时候传递的参数和调用的时候传递的参数会合并到一起，传递到函数内部

### 函数的其它成员

- arguments
  + 实参集合
- caller
  + 函数的调用者
- length
  + 形参的个数
- name
  + 函数的名称

```javascript
function fn(x, y, z) {
  console.log(fn.length) // => 形参的个数
  console.log(arguments) // 伪数组实参参数集合
  console.log(arguments.callee === fn) // 函数本身
  console.log(fn.caller) // 函数的调用者
  console.log(fn.name) // => 函数的名字
}
```

### 高阶函数

- 函数可以作为参数
- 函数可以作为返回值

#### 作为参数

函数作为参数，可以让函数更灵活，外部调用该函数的时候可以传入一个逻辑功能。

```javascript
function eat(callback) {
  console.log('吃完了')
  callback();
}

eat(function () {
  console.log('去唱歌');
});
```

数组的sort方法：

- 默认的sort方法，无参数
  - 排序的时候，是按照数字，大写字母，小写字母的顺序排序
  - 本质是按照ASCII码排序，[ASCII码参考链接](https://baike.baidu.com/item/ASCII/309296?fr=aladdin)
- sort方法可以传递一个函数
  - 该函数有两个参数，是数组的两个元素
  - 返回，两个元素比较的结果

#### 作为返回值

函数作为返回值，相当于创造一个函数，创造可以执行多个类似功能的函数。

案例：写一个函数返回一个函数，返回的函数可以实现 1+m,  100+m, 1000+m

```javascript
function getFun(n) {
  return function (m) {
    return n + m;
  }
}
// 求 100 + m
var fn100 = getFun(100);
// 求 1000 + m
var fn1000 = getFun(1000);
console.log(fn100(1));
console.log(fn1000(1));
```

### 闭包

#### 作用域、作用域链、预解析

- 全局作用域
- 函数作用域
- **没有块级作用域**(ES6中有块级作用域)
- **词法作用域：**由你在写代码时将变量和块作用域写在哪里来决定的

```javascript
{
  var foo = 'bar';
}

console.log(foo);

if (true) {
  var a = 123;
}
console.log(a);
```

作用域链示例代码：

```javascript
var a = 10;

function fn() {
  var b = 20;

  function fn1() {
    var c = 30;
    console.log(a + b + c);
  }

  function fn2() {
    var d = 40;
    console.log(c + d);
  }

  fn1();
  fn2();
}
fn();
```

- **内层作用域可以访问外层作用域**，反之不行

#### 什么是闭包

**闭包(Closure)**：闭包是函数和声明该函数的词法环境的组合。当函数可以记住并访问所在的词法作用域时，就产生了闭包，即使函数是在当前词法作用域之外执行 。

- 闭包就是能够读取/设置其他函数内部变量的函数，
- 闭包就是将函数内部和函数外部连接起来的一座桥梁。

闭包的用途：

- 可以在函数外部读取函数内部成员
- 让函数内成员始终存活在内存中(延展变量的使用范围)

#### 一些关于闭包的例子

闭包演示：

```javascript
function fn() {
  var n = 10;
  return function () {
    return n;
  }
}
var f = fn();
console.log(f());
```

案例1：点击按钮设置文档字体大小

```js
// 创建一个函数，设置body的字体大小
function makeFun(size) {
  return function () {
    document.body.style.fontSize = size + 'px';
  }
}

btn1.onclick = makeFun(12);
btn2.onclick = makeFun(14);
btn3.onclick = makeFun(16);
```

案例2：闭包的经典案例

```js
var heroes = document.getElementById('heroes');
var list = heroes.children;
for (var i = 0; i < list.length; i++) {
  var li = list[i];
 
  (function (i) {
    li.onclick = function () {
      // 点击li的时候输出当前li对应的索引
      console.log(i);
    }
  })(i);
}
```

案例3：定时器输出循环变量i

```js
console.log('start');
for (var i = 0; i < 3; i++) {
  (function (i) {
    setTimeout(function () {
      console.log(i);
    }, 0);
  })(i);
}
console.log('end');
```

- setTimeout的执行原理

  所有的js代码都在执行栈上执行，定时器和注册事件的函数会放到一个任务队列上。

  当执行栈上的代码执行完毕后，才会执行任务队列上的任务。

#### 闭包的思考题

思考题 1：

```javascript
var name = "The Window";
var object = {
  name: "My Object",
  getNameFunc: function () {
    return function () {
      return this.name;
    };
  }
};

console.log(object.getNameFunc()());
```

思考题 2：

```javascript
var name = "The Window";　　
var object = {　　　　
  name: "My Object",
  getNameFunc: function () {
    var that = this;
    return function () {
      return that.name;
    };
  }
};
console.log(object.getNameFunc()())
```

#### 小结

闭包现象：在函数的外部可以操作函数内部的变量(延展了变量的作用范围)

### 递归

- 什么是递归
  - 函数自己调用自己
  - 一般递归函数都需要参数
  - **递归需要有结束的条件**，递归函数的参数一般跟结束条件相关

#### 递归执行模型

```javascript
function fn1 () {
  console.log(111)
  fn2()
  console.log('fn1')
}

function fn2 () {
  console.log(222)
  fn3()
  console.log('fn2')
}

function fn3 () {
  console.log(333)
  fn4()
  console.log('fn3')
}

function fn4 () {
  console.log(444)
  console.log('fn4')
}

fn1()
```

#### 举个栗子

- 计算累加的递归函数

![](media/累加递归.png)

递归一般归纳：f(n) = f(n - 1)  + 业务逻辑处理

把大范围的处理，逐级递减

​	1. 业务的逻辑是固定的

​	2. 业务的范围逐渐缩小，直至结束条件

- 计算阶乘的递归函数

```javascript
function factorial (num) {
  if (num <=  1) {
    return 1
  } else {
    return num * factorial(num - 1)
  }
}
```

#### 递归应用场景

- 深拷贝

  ```js
  // 深拷贝
  function deepCopy(o1, o2) {
    for (var key in o1) {
      if (o2[key]) {
        continue;
      }
      if (o1[key] instanceof Function) {
  	  o2[key] = o1[key];
      } else if (o1[key] instanceof Array) {
        // 如果key是数组类型 Array？   []
        o2[key] = [];
        deepCopy(o1[key], o2[key]);
      } else if (o1[key] instanceof Object) {
        // 如果key是复杂类型 Object？  {}
        o2[key] = {};
        deepCopy(o1[key], o2[key]);
      } else {
        // 如果key这个属性 是基本类型
        o2[key] = o1[key];
      }
    }
  }
  ```

- 遍历 DOM 树

---

## 面向对象游戏案例：贪吃蛇

### 案例介绍

#### 游戏演示

演示：[贪吃蛇](snake/index.html)

#### 案例目标

游戏的目的是用来体会js高级语法的使用 不需要具备抽象对象的能力，使用面向对象的方式分析问题，需要一个漫长的过程。

#### 前置知识点

- 自调用函数

  - 解决变量命名冲突的问题

  - 问题：演示不加分号的问题

  - 语法

    ```js
    (function() {
      	var msg = '你猜谁能访问我';
        console.log(msg);
    })();
    console.log(msg);
    ```

    > 注意：
    >
    > - 函数要使用()包含起来，把函数变成表达式
    > - 后面的小括号是函数调用

  - 问题

    - 问题1：如果存在多个自调用函数要用分号分割，否则语法错误

      ```js
      // 错误
      (function () {
      })()

      (function () {
      })()

      // 正确
      ;(function () {
      })()

      ;(function () {
      })()
      ```

    - 问题2：当自调用函数 前面有函数表达式时，会出错。

      ```js
      // 所以建议自调用函数前，加上;
      var a = function () {
        alert('11');
      }
      (function () {
        alert('22');
      })();
      ```


### 功能实现

#### 搭建页面

放一个容器盛放游戏场景 div#map，设置样式

```css
#map {
  position: relative;
  margin: 0 auto;
  width: 800px;
  height: 600px;
  border: 1px solid #eee;
  background-color: lightgray;
}
/* 蛇和食物的样式 */
.snake-head, .snake-body, .food {
  position: absolute;
  width: 17px;
  height: 17px;
  border: dotted 1px black;
  background-color: red;
}
.snake-body {
  background-color: blue;
}
.food {
  background-color: green;
}
```

#### 分析对象

- 游戏对象
- 蛇对象
- 食物对象

#### 创建食物对象

- Food
  - 属性
    - left   横坐标
    - top   纵坐标
    - _div   食物对应的div
  - 方法
    - random()  随机设置食物的位置 
- 创建Food的构造函数
  - 设置属性
  - 创建食物对应的div

```js
function Food(map) {
  this.left = 0;
  this.top = 0;
  this.div = document.createElement('div');
  this.div.className = 'food';
  map.appendChild(this.div);
}
```

- 通过原型设置random方法，实现随机设置食物的位置
  - 随机生成食物的位置
  - 设置食物对象对应div的位置

```js
Food.prototype.random = function () {
  var maxY = 800 / 20 - 1;
  var maxX = 600 / 20 - 1;
  this.left = getRandom(0, maxX) * 20;
  this.top = getRandom(0, maxY) * 20;
  this.div.style.left = this.left + 'px';
  this.div.style.top = this.top + 'px';
}
/**
 * 生成min到max之间的随机整数，范围[min, max]
 * @param {Number} min 
 * @param {Number} max 
 */
function getRandom(min, max) { 
  return parseInt(Math.random() * (max - min + 1)  + min);
}
```

- 通过自调用函数，进行封装，通过window暴露Food对象

```js
window.Food = Food;
```

#### 创建蛇对象

- Snake
- 属性
  - body     数组，蛇的头部和身体，第一个位置是蛇头
  - direction    蛇运动的方向  默认right  可以是 left  top bottom
  - _map    地图对象
- 方法
  - move                蛇移动
- Snake构造函数

```js
function Snake(map) {
  // 蛇运动的方向
  this.direction = 'right';
  // 存储蛇的头和身体，第一个元素是蛇头 (存储的是蛇对应的div元素)
  this.body = [];
  this._map = map;
    
  // 初始化，创建蛇
}
```

- insertHead函数插入蛇头，传入蛇头的新的位置
  - 作用：1. 初始化的时候创建蛇。2. 蛇吃到食物之后新加一节
  - 步骤：
    - 获取当前的蛇头，修改类样式标记为蛇身体。初始化的时候没有蛇头设置为空对象。
    - 创建新的蛇头-div，设置类样式和位置并追加到map中
    - 把新的蛇头通过数组的unshift方法，放到body数组中的第一个元素

```js
function insertHead(snake) {
  var location = getHeadLocation(snake);
  // 1. 获取当前的蛇头，标记为身体
  var head = snake.body[0] || {};
  // 因为要插入一个蛇头，所以这节该为body
  head.className = 'snake-body';
  // 2. 创建新的蛇头
  var newHead = document.createElement('div');
  newHead.className = 'snake-head';
  newHead.style.top = location.top + 'px';
  newHead.style.left = location.left + 'px';
  snake._map.appendChild(newHead);

  // 把元素插入到数组的第一个位置
  snake.body.unshift(newHead);
}
```

- getHeadLocation函数
  - 作用：插入蛇头之前计算蛇头的位置，蛇在移动过程中计算蛇头的新位置
  - 步骤：
    - 如果body数组的长度为0，返回0坐标
    - 如果body数组不为空，取出第一个元素(蛇头)，获取蛇头当前的坐标
    - 根据蛇运动的方向，设置新坐标

```js
function getHeadLocation(snake) {
  // 1. 如果蛇的body属性长度为0，第一个放进入的认为是蛇头
  if (snake.body.length === 0) {
    return {left: 0, top: 0};
  }
  // 2. 如果蛇不为空，取出蛇头的位置
  var head = snake.body[0];
  var left = head.offsetLeft;
  var top = head.offsetTop;
  var step = 20;
  // 3. 判断蛇运动的方向
  switch (snake.direction) {
    case 'right': 
      left += step;
      break;
    case 'left':
      left -= step;
      break;
    case 'top': 
      top -= step;
      break;
    case 'bottom':
      top += step;
      break;
  }
  return {left: left, top: top};
}
```

- move方法
  - 作用：蛇移动的方法

  - 步骤：
    - 获取新蛇头移动之后的位置
    - 获取当前蛇头，通过类样式标记为蛇身体
    - 从body中弹出蛇的最后一个身体作为新的蛇头，通过类样式标记为蛇头，设置新位置
    - 把新的蛇头，放到body数组的第一个元素之前

```js
Snake.prototype.move = function () {
  // 获取蛇头的新位置
  var location = getHeadLocation(this);

  // 蛇移动
  var currentHead = this.body[0];
  currentHead.className = 'snake-body';
  var currentTail = this.body.pop();
  currentTail.className = 'snake-head';
  currentTail.style.left = location.left + 'px';
  currentTail.style.top = location.top + 'px';
  this.body.unshift(currentTail);
}
```

- 在自调用函数中暴露Snake对象

```js
window.Snake = Snake;
```

#### 创建游戏对象

游戏对象，用来管理游戏中的所有对象和开始游戏

- Game
  - 属性
    - food
    - snake
  - 方法
    - start            开始游戏（绘制所有游戏对象）


- 构造函数
  - 创建食物属性，随机食物位置
  - 创建蛇属性

```js
function Game(map) {
  this.food = new Food(map);
  this.snake = new Snake(map);
}
```

- 开始游戏，渲染食物对象和蛇对象
  - 开启定时器，调用蛇移动的方法
  - 判断游戏是否结束，如果结束，清除定时器
  - 给文档注册键盘监听事件，监听用户按下的上下左右，改变蛇移动的方向

```js
Game.prototype.start = function () {
  // 开启定时器，移动蛇
  var timerId = setInterval(function () {
    this.snake.start();
  }.bind(this), 100);

  document.onkeydown = function (e) {
    // 37 左 38 上 39 右 40 下
    console.log(e.keyCode);
    switch (e.keyCode) {
      case 37:
        if (this.snake.direction === 'right') {
          return;
        }
        this.snake.direction = 'left';
        break;
      case 38:
        if (this.snake.direction === 'bottom') {
          return;
        }
        this.snake.direction = 'top';
        break;
      case 39:
        if (this.snake.direction === 'left') {
          return;
        }
        this.snake.direction = 'right';
        break;
      case 40:
        if (this.snake.direction === 'top') {
          return;
        }
        this.snake.direction = 'bottom';
        break;
    }
  }.bind(this);
}
```

#### 游戏的逻辑

- 在移动的过程中判断蛇是否死亡
  - 在snake.js中，新增isDead函数
  - 传递蛇头的最新位置，判断蛇是否越界

```js
// 判断蛇是否死亡
function isDead(location) {
  return (location.left < 0 || 
          location.top < 0 || 
          location.left >= 800 || 
          location.top >= 600
         );
}
```

- 蛇移动的过程中判断是否死亡
  - 在snake.js的方法中
    - 判断蛇是否死亡，如果蛇撞墙，返回true，游戏结束。
    - 否则返回false，继续游戏
  - 在game.js的start方法中
    -  根据move方法的返回值，判断游戏是否结束

```js
// 1. 在snake.js的方法中
// 判断蛇是否死亡
if (this.isDead(location)) {
    return true;
}
// move方法的最后
return false;
 
// 2. 在game.js的start方法中
var gameover = this.snake.move();
if (gameover) {
    clearInterval(timerId);
    alert('游戏结束');
}
```

- 蛇移动过程中判断蛇是否吃到食物
  - snake.js的move方法中
    - move方法传入食物对象
    - 判断蛇头位置和食物位置是否相等
      - 如果相等插入一个蛇头
      - 随机食物位置
  - game.js的start方法中
    - 调用move方法的时候传入food对象(**容易忘记**)

```js
// 1. snake.js的move方法中
//  蛇移动的过程中判断是否吃到食物
if (location.left === food.left && 
    location.top === food.top) {
	// 吃到食物，加蛇肉，重新生成食物的位置
    insertHead(this);
    food.random();
}

// 2. game.js的start方法中
// 给move方法传入 食物对象
var gameover = this.snake.move(this.food);
```



#### 整理代码

现在的代码结构清晰，谁出问题就找到对应的js文件即可。
通过自调用函数，已经防止了变量命名污染的问题

但是，由于js文件数较多，需要在页面上引用，会产生文件依赖的问题(先引入那个js，再引入哪个js)
将来通过工具把js文件合并并压缩。现在手工合并js文件演示

## 正则表达式

- 了解正则表达式基本语法
- 能够使用JavaScript的正则对象

### 正则表达式简介

#### 什么是正则表达式

正则表达式：正则表达式是对**字符串操作**的一种逻辑公式，就是用事先定义好的一些特定字符、及这些特定字符的组合，组成一个“**规则字符串**”，这个“**规则字符串**”用来表达对字符串的一种过滤逻辑。

正则表达式在其他语言中也广泛应用。

#### 正则表达式的作用

1. 给定的字符串是否符合正则表达式的过滤逻辑(**匹配**)
2. 可以通过正则表达式，从字符串中获取我们想要的特定部分(**提取**)
3. 强大的字符串替换能力(**替换**)

#### 正则表达式的特点

1. 灵活性、逻辑性和功能性非常的强
2. 可以迅速地用**极简单的方式**达到字符串的复杂控制
3. 对于刚接触的人来说，比较晦涩难懂

### 正则表达式的测试

- [在线测试正则](https://c.runoob.com/front-end/854)
- 工具中使用正则表达式
  + sublime/vscode/word
  + 演示替换所有的数字

### 正则表达式的组成

- 普通字符abc  123
- 特殊字符(元字符)：正则表达式中有特殊意义的字符\d  \w

示例演示：

- `\d` 匹配数字
- `ab\d` 匹配 ab1、ab2

### 元字符

通过测试工具演示下面元字符的使用

#### 常用元字符串

| 元字符  | 说明              |
| ---- | --------------- |
| \d   | 匹配数字            |
| \D   | 匹配任意非数字的字符      |
| \w   | 匹配字母或数字或下划线     |
| \W   | 匹配任意不是字母，数字，下划线 |
| \s   | 匹配任意的空白符        |
| \S   | 匹配任意不是空白符的字符    |
| .    | 匹配除换行符以外的任意单个字符 |
| ^    | 表示匹配行首的文本(以谁开始) |
| $    | 表示匹配行尾的文本(以谁结束) |

#### 限定符

| 限定符   | 说明       |
| ----- | -------- |
| *     | 重复零次或更多次 |
| +     | 重复一次或更多次 |
| ?     | 重复零次或一次  |
| {n}   | 重复n次     |
| {n,}  | 重复n次或更多次 |
| {n,m} | 重复n到m次   |

#### 其它

```
[] 字符串用中括号括起来，表示匹配其中的任一字符，相当于或的意思
[^]  匹配除中括号以内的内容
\ 转义符
| 或者，选择两者中的一个。注意|将左右两边分为两部分，而不管左右两边有多长多乱
() 从两个直接量中选择一个，分组
   eg：gr(a|e)y匹配gray和grey
[\u4e00-\u9fa5]  匹配汉字
```

[正则表达式参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)

### 案例

```js
1. 验证邮编		 100010
2. 验证手机号	18910000000
3. 验证日期		 2018-3-31
4. 验证邮箱		 xxx@itcast.cn
5. 验证IP地址	 192.168.1.10
```

验证手机号：

```javascript
^\d{11}$
```

验证邮编：

```javascript
^\d{6}$
```

验证日期 2012-5-01 

```javascript
^\d{4}-\d{1,2}-\d{1,2}$
```

验证邮箱 xxx@itcast.cn：

```javascript
^\w+@\w+(\.\w+){1,2}$
```

验证IP地址 192.168.1.10

```javascript
^\d{1,3}(\.\d{1,3}){3}$
```

## JavaScript 中使用正则表达式

### 创建正则对象

方式1：

```javascript
var reg = new RegExp('\\d', 'i');
var reg = new RegExp('\\d', 'gi');
```

方式2：

```javascript
var reg = /\d/i;
var reg = /\d/gi;
```

#### 参数

| 标志   | 说明         |
| ---- | ---------- |
| i    | 忽略大小写      |
| g    | 全局匹配       |
| gi   | 全局匹配+忽略大小写 |

### 正则匹配

```javascript
// 匹配日期
var dateStr = '2015-10-10';
var reg = /^\d{4}-\d{1,2}-\d{1,2}$/
console.log(reg.test(dateStr));
```

#### 案例：表单验证

```html
QQ号：<input type="text" id="txtQQ"><span></span><br>
邮箱：<input type="text" id="txtEMail"><span></span><br>
手机：<input type="text" id="txtPhone"><span></span><br>
生日：<input type="text" id="txtBirthday"><span></span><br>
姓名：<input type="text" id="txtName"><span></span><br>
```

```javascript
//获取文本框
var txtQQ = document.getElementById("txtQQ");
var txtEMail = document.getElementById("txtEMail");
var txtPhone = document.getElementById("txtPhone");
var txtBirthday = document.getElementById("txtBirthday");
var txtName = document.getElementById("txtName");

//
txtQQ.onblur = function () {
  //获取当前文本框对应的span
  var span = this.nextElementSibling;
  var reg = /^\d{5,12}$/;
  //判断验证是否成功
  if(!reg.test(this.value) ){
    //验证不成功
    span.innerText = "请输入正确的QQ号";
    span.style.color = "red";
  }else{
    //验证成功
    span.innerText = "";
    span.style.color = "";
  }
};

//txtEMail
txtEMail.onblur = function () {
  //获取当前文本框对应的span
  var span = this.nextElementSibling;
  var reg = /^\w+@\w+\.\w+(\.\w+)?$/;
  //判断验证是否成功
  if(!reg.test(this.value) ){
    //验证不成功
    span.innerText = "请输入正确的EMail地址";
    span.style.color = "red";
  }else{
    //验证成功
    span.innerText = "";
    span.style.color = "";
  }
};
```

表单验证部分，封装成函数：

```javascript
var regBirthday = /^\d{4}-\d{1,2}-\d{1,2}$/;
addCheck(txtBirthday, regBirthday, "请输入正确的出生日期");
//给文本框添加验证
function addCheck(element, reg, tip) {
  element.onblur = function () {
    //获取当前文本框对应的span
    var span = this.nextElementSibling;
    //判断验证是否成功
    if(!reg.test(this.value) ){
      //验证不成功
      span.innerText = tip;
      span.style.color = "red";
    }else{
      //验证成功
      span.innerText = "";
      span.style.color = "";
    }
  };
}
```

通过给元素增加自定义验证属性对表单进行验证：

```html
<form id="frm">
  QQ号：<input type="text" name="txtQQ" data-rule="qq"><span></span><br>
  邮箱：<input type="text" name="txtEMail" data-rule="email"><span></span><br>
  手机：<input type="text" name="txtPhone" data-rule="phone"><span></span><br>
  生日：<input type="text" name="txtBirthday" data-rule="date"><span></span><br>
  姓名：<input type="text" name="txtName" data-rule="cn"><span></span><br>
</form>
```

```javascript
// 所有的验证规则
var rules = [
  {
    name: 'qq',
    reg: /^\d{5,12}$/,
    tip: "请输入正确的QQ"
  },
  {
    name: 'email',
    reg: /^\w+@\w+\.\w+(\.\w+)?$/,
    tip: "请输入正确的邮箱地址"
  },
  {
    name: 'phone',
    reg: /^\d{11}$/,
    tip: "请输入正确的手机号码"
  },
  {
    name: 'date',
    reg: /^\d{4}-\d{1,2}-\d{1,2}$/,
    tip: "请输入正确的出生日期"
  },
  {
    name: 'cn',
    reg: /^[\u4e00-\u9fa5]{2,4}$/,
    tip: "请输入正确的姓名"
  }];

addCheck('frm');


//给文本框添加验证
function addCheck(formId) {
  var i = 0,
      len = 0,
      frm =document.getElementById(formId);
  len = frm.children.length;
  for (; i < len; i++) {
    var element = frm.children[i];
    // 表单元素中有name属性的元素添加验证
    if (element.name) {
      element.onblur = function () {
        // 使用dataset获取data-自定义属性的值
        var ruleName = this.dataset.rule;
        var rule =getRuleByRuleName(rules, ruleName);

        var span = this.nextElementSibling;
        //判断验证是否成功
        if(!rule.reg.test(this.value) ){
          //验证不成功
          span.innerText = rule.tip;
          span.style.color = "red";
        }else{
          //验证成功
          span.innerText = "";
          span.style.color = "";
        }
      }
    }
  }
}

// 根据规则的名称获取规则对象
function getRuleByRuleName(rules, ruleName) {
  var i = 0,
      len = rules.length;
  var rule = null;
  for (; i < len; i++) {
    if (rules[i].name == ruleName) {
      rule = rules[i];
      break;
    }
  }
  return rule;
}
```

### 正则提取

```javascript
// 1. 提取工资
var str = "张三：1000，李四：5000，王五：8000。";
var array = str.match(/\d+/g);
console.log(array);

// 2. 提取email地址
var str = "123123@xx.com,fangfang@valuedopinions.cn 286669312@qq.com 2、emailenglish@emailenglish.englishtown.com 286669312@qq.com...";
var array = str.match(/\w+@\w+\.\w+(\.\w+)?/g);
console.log(array);

// 3. 分组提取  
// 3. 提取日期中的年部分  2015-5-10
var dateStr = '2016-1-5';
// 正则表达式中的()作为分组来使用，获取分组匹配到的结果用Regex.$1 $2 $3....来获取
var reg = /(\d{4})-\d{1,2}-\d{1,2}/;
if (reg.test(dateStr)) {
  console.log(RegExp.$1);
}

// 4. 提取邮件中的每一部分
var reg = /(\w+)@(\w+)\.(\w+)(\.\w+)?/;
var str = "123123@xx.com";
if (reg.test(str)) {
  console.log(RegExp.$1);
  console.log(RegExp.$2);
  console.log(RegExp.$3);
}
```

### 正则替换

```javascript
// 1. 替换所有空白
var str = "   123AD  asadf   asadfasf  adf ";
str = str.replace(/\s/g,"xx");
console.log(str);

// 2. 替换所有,|，
var str = "abc,efg,123，abc,123，a";
str = str.replace(/,|，/g, ".");
console.log(str);
```



---

## 附录

### A 代码规范

#### 代码风格

- [JavaScript Standard Style ](https://github.com/feross/standard)
- [Airbnb JavaScript Style Guide() {](https://github.com/airbnb/javascript)

#### 校验工具

- [JSLint](https://github.com/douglascrockford/JSLint)
- [JSHint](https://github.com/jshint/jshint)
- [ESLint](https://github.com/eslint/eslint)

### B Chrome 开发者工具

### C 文档相关工具

- 电子文档制作工具: [docute](https://github.com/egoist/docute)
- 流程图工具：[DiagramDesigner](http://logicnet.dk/DiagramDesigner/)
