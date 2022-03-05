###  React 事件绑定原理

React并不是将click事件绑在该div的真实DOM上，而是`在document处监听所有支持的事件`，当事件发生并冒泡至document处时，React将事件内容封装并交由真正的处理函数运行。这样的方式不仅减少了内存消耗，还能在组件挂载销毁时统一订阅和移除事件。
 另外冒泡到 document 上的事件也不是原生浏览器事件，而是 React 自己实现的合成事件（SyntheticEvent）。因此我们如果不想要事件冒泡的话，调用 event.stopPropagation 是无效的，而应该调用 `event.preventDefault`。

![react事件绑定原理](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2089718f74b342869de15f01588f033f~tplv-k3u1fbpfcp-watermark.awebp)







### 数据类型

#### 基本数据类型（值类型）

* Number
* String
* Boolean
* Symbol
* BigInt
* null
* Undefined

#### 引用数据类型（复杂数据类型）

* Object
* Function
* Array
* Date
* RegExp

##### 数据类型检测方法

1. `typeof()`  
   * 快速区分基本数据类型
   * 不能区分Object、Array、Null，都返回Object
   * 
2. `instanceof()`
   * 区分Array、Object和Function，适合用于判断自定义的类实例对象
   * 无法判断基本数据类型
3. `Object.prototype.toString.call()`
   * 精准判断数据类型
   * 可以进行封装使用

***

### 垃圾回收机制

[JS 垃圾回收机制 ](https://segmentfault.com/a/1190000018605776)

##### 标记清除

当变量进入环境时，就将这个变量标记为“进入环境”。从逻辑上讲，永远不能释放进入环境的变量所占用的内存，因为只要执行流进入相应的环境，就可能会用到它们。而当变量离开环境时，则将其标记为“离开环境”。垃圾收集器在运行的时候会给存储在内存中的所有变量都加上标记（当然，可以使用任何标记方式）。然后，它会去掉环境中的变量以及被环境中的变量引用的变量的标记。而在此之后再被加上标记的变量将被视为准备删除的变量，原因是环境中的变量已经无法访问到这些变量了。

##### 引用计数

引用计数的含义是跟踪记录每个值被引用的次数。当声明了一个变量并将一个引用类型值赋给该变量时，则这个值的引用次数就是1.如果同一个值又被赋给另一个变量，则该值的引用次数加1。，相反，如果包含对这个引用的变量取得了另外一个值，则这个值的引用次数减1.当这个值的引用次数变成0时，则说明没有办法再访问这个值了，因而就可以将其占用的内存空间收回来。

##### 查找引用

浏览器不定时去查找当前内存的引用，如果没有被占用了，浏览器会回收它；如果被占用，就不能回收。

***

### 作用域与作用域链

作用域就是变量与函数的可访问范围，由当前环境与上层环境的一系列变量对象组成。

1. 全局作用域：代码在程序的任何地方都能被访问，window 对象的内置属性都拥有全局作用域。
2. 函数作用域：在固定的代码片段才能被访问。

**作用**：作用域最大的用处就是`隔离变量`，不同作用域下同名变量不会有冲突。



**作用域链：** 一般情况下，变量到 创建该变量 的函数的作用域中取值。但是如果在当前作用域中没有查到，就会向上级作用域去查，直到查到全局作用域，这么一个查找过程形成的链条就叫做作用域链。

### 原型与原型链

### Event Loop

#### Node环境下的事件循环

`Node`是基于V8引擎的运行在服务端的`JavaScript`运行环境，在处理高并发、I/O密集(文件操作、网络操作、数据库操作等)场景有明显的优势。虽然用到也是V8引擎，但由于服务目的和环境不同，导致了它的API与原生JS有些区别，其Event Loop还要处理一些I/O，比如新的网络连接等，所以Node的Event Loop(事件环机制)与浏览器的是不太一样。

执行顺序如下：

- `timers`: 计时器，执行setTimeout和setInterval的回调
- `pending callbacks`: 执行延迟到下一个循环迭代的 I/O 回调
- `idle, prepare`: 队列的移动，仅系统内部使用
- `poll轮询`: 检索新的 I/O 事件;执行与 I/O 相关的回调。事实上除了其他几个阶段处理的事情，其他几乎所有的异步都在这个阶段处理。
- `check`: 执行`setImmediate`回调，setImmediate在这里执行
- `close callbacks`: 执行`close`事件的`callback`，一些关闭的回调函数，如：socket.on('close', ...)



### New运算符

1. 首先创建了一个新的`空对象`
2. `设置原型`，将对象的原型设置为函数的`prototype`对象。
3. 让函数的`this`指向这个对象，执行构造函数的代码（为这个新对象添加属性）
4. 判断函数的返回值类型，如果是值类型，返回创建的对象。如果是引用类型，就返回这个引用类型的对象。





### 防抖节流

#### 函数防抖（debounce）

- **定义**
  函数防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间。

  > 简单的说，当一个动作连续触发，则只执行最后一次。

- **应用场景**
  1、搜索框搜索输入。只需用户最后一次输入完，再发送请求
  2、手机号、邮箱验证输入检测
  3、窗口大小Resize。只需窗口调整完成后，计算窗口大小。防止重复渲染。

```javascript
/*  实现函数防抖的函数*/
const debounce =function(fn, wait){
  let timer;
  return function(){
    timer && clearTimeout(timer);
    let context = this
    let args = arguments;
    timer = setTimeout(function(){
        fn.apply(context,args);
    }, wait || 1000);
  };
};
```

#### 函数节流（throttle）

- **定义**
  限制一个函数在一定时间内只能执行一次。
- **应用场景**
  1、加载更多、滚到底部监听
  2、高频点击提交，表单重复提交

```javascript
// 实现函数节流的函数
const throttle =function(fn,wait) {
  let canrun = true;
  return function( ) {
    if(!canrun) return false;
    canrun = false;
    let context = this, 
    let args = arguments;
    setTimeout(function(){
         fn.apply(context,args);
         canrun = true;
    }, wait || 1000);
  };
};
```



### 快速排序





### 数组扁平化

```javascript
const flat = (arr, depth = 1) => {
    if(depth > 0) {
        return arr.reduce((pre, cur) => {
            return per.concat(Array.isArray(cul) ? flat(cur, depth - 1) : cur)
        }, [])
    }
    return arr.slice()
}
```



### 手写reduce

```javascript

```

