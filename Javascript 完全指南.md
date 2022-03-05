# Javascript 完全指南

### this函数指向问题：

function函数由函数调用者决定，层层向上，最高为windows（运动的变化）

箭头函数的this指向该箭头函数定义时所处的对象，而不是函数使用时所在的对象，默认使用父类的this。不能用作构造函数。（集合，映射）定义域，值域，法则。

在ES6中，会默认采用严格模式，因此this也不会自动指向window对象了，而箭头函数本身并没有this，因此this就只能是undefined.

***

### 闭包

闭包由两部分组成，执行上下文A，以及在该执行上下文中创建的函数B，当B执行时，并且访问了执行上下文A中变量对象的值，那么闭包就产生了。

* 模块化
* 柯里化

* 实现setTimeout函数的传参效果
* 封装私有变量，避免污染变量
* 函数防抖
* 定义回调行为

[(142条消息) 面试官：谈谈对JS闭包的理解及常见应用场景(闭包的作用)_ LYFlied的博客-CSDN博客_前端面试谈谈对闭包理解](https://blog.csdn.net/qq_39903567/article/details/115010640)

#### 执行上下文（栈结构）

* 全局环境：JavaScript代码运行起来会首先进入该环境
* 函数环境：当函数被调用执行时，会进入当前函数中执行代码

##### 执行上下文的生命周期

* **创建阶段**

在这个阶段中，执行上下文会分别创建变量对象，建立作用域链，以及确定this指向。

* **代码执行阶段**

创建完成之后，就会开始执行代码，会完成变量赋值，函数引用，以及执行其他代码。



##### 变量提升

对于 var 关键字而言，即使未对变量进行赋值，在声明时期被收集到变量对象中并赋值undefined

而 let/const 关键字声明的变量，即使仍然会被收集到变量对象中（仍然会存在变量提升），但不会对其赋值undefined，这个现象被称为**暂时性死区** 抛出 `ReferenceError`

***

### 高度塌陷和BFC

高度塌陷的原因：

在文档流中，父元素的高度默认是被子元素撑开的，也就是子元素多高，父元素就多高。但是当为子元素设置浮动以后，子元素会完全脱离文档流，此时将会导致子元素无法撑起父元素的高度，导致父元素的高度塌陷。

```
overflow: hidden
```

[CSS高度塌陷问题解决方案 - 我所向往的美好 - 博客园 (cnblogs.com)](https://www.cnblogs.com/fuck1/p/7441327.html)

***

### 跨域问题

* **使用ajax的jsonp**
* **使用cors**：服务器端设置，前端不需要改变。

***

### 回文字符串

```javascript
var s = '上海自来水来自海上';
var len = s.length;
var mid = len % 2 ? len / 2 - 1 : len / 2; // 字符串长度为奇、偶数时的比较次数 
var flag = true;
 
for (let i = 0; i < mid; i++) {
  let head = s[i];
  let tail = s[len - i - 1];
	
  if (head !== tail) {
    flag = false;
    break;
  }
}
 
console.log(flag);
```

***

### 数据类型详解

JavaScript中分为基本数据类型和引用数据类型。

* 基本数据类型：Number、String 、Boolean、Null和Undefined。按值访问。储存在栈。
* 引用数据类型：**Object 、Array 、Function 、Data。**按引用访问。储存在堆。

两种不同数据类型的根本区别在于声明变量时不同的内存分配，不同的内存分配机制也带来了不同的访问机制。进而对变量的复制以及传递参数的行为也不同。

##### 如何判断数据类型：

* typeof()、typeof(null) = object、 typeof(function) = function

  * ```
    typeof 1` 返回 `"number"`. `typeof "number"` 返回 `"string"
    ```

* instanceof()  通过原型链判断 array.____proto____ === Array.prototype   

* constructor 无法判断 null 和 undefined 

* toString()   返回当前对象的[[Class]]，一个内部属性，其格式为 [object 类型]



#### PS

* [...args] 返回一个带参数的数组

* 所有对象键（不包括`Symbols`）都会被存储为字符串，即使你没有给定字符串类型的键。 这就是为什么`obj.hasOwnProperty（'1'）`也返回`true`。
* 只有7种内置类型：`null`，`undefined`，`boolean`，`number`，`string`，`object`和`symbol`。 `function`不是一个类型，因为函数是对象，它的类型是`object`。
* `JavaScript`中只有6个假值：
  - `undefined`
  - `null`
  - `NaN`
  - `0`
  - `''` (empty string)
  - `false`

***

### 为什么要有构造函数

类是一种抽象结构，而不是一种实体，并不占用存储空间。而类的对象不同，需要容纳数据，占用空间。因为对于不可见的成员，不知道哪些需要进行初始化，而哪些又是不用的。所以通过引入构造函数，来解决类的对象的初始化问题。

***

### 快速排序

[JavaScript 快速排序算法 - 新羽 - 博客园 (cnblogs.com)](https://www.cnblogs.com/hjx-blog/articles/9183453.html)

***

### Event Loop

任务队列又分为macro-task（宏任务）与micro-task（微任务），在最新标准中，它们被分别称为task与jobs。

* macro-task大概包括：script(整体代码), **setTimeout, setInterval, setImmediate**, I/O, UI rendering。
* micro-task大概包括: **process.nextTick**, **Promise**, Object.observe(已废弃), MutationObserver(html5新特性)

它从script(整体代码)开始第一次循环。之后全局上下文进入函数调用栈。直到调用栈清空(只剩全局)，然后执行所有的micro-task。当所有可执行的micro-task执行完毕之后。循环再次从macro-task开始，找到其中一个任务队列执行完毕，然后再执行所有的micro-task，这样一直循环下去。

***

### 垃圾回收

[(142条消息) js垃圾清除的那些事儿_南徒切切的博客-CSDN博客](https://blog.csdn.net/weixin_43678786/article/details/84882984)

[JavaScript垃圾收集——标记清除和引用计数 - 邹天得 - 博客园 (cnblogs.com)](https://www.cnblogs.com/videring/articles/7191749.html)

[js的新生代垃圾回收 - 张啊咩 - 博客园 (cnblogs.com)](https://www.cnblogs.com/amiezhang/p/11386644.html)



***

### sessionStorage 和 localStorage

关闭选项卡后，将删除存储在`sessionStorage`中的数据。

如果使用`localStorage`，数据将永远存在，除非例如调用`localStorage.clear()`。

***

### call() 和 bind() 方法

使用两者，我们可以传递我们想要`this`关键字引用的对象。 但是，`.call`方法会立即执行！

`.bind`方法会返回函数的拷贝值，但带有绑定的上下文！ 它不会立即执行。

//TODO





***

### Array

当你为数组中的元素设置一个超过数组长度的值时，`JavaScript`会创建一个名为“空插槽”的东西。 这些位置的值实际上是`undefined`