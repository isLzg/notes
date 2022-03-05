### React 事件系统

[react事件系统原理 ](https://juejin.cn/post/6955636911214067720)

1. `jsx` 中绑定的事件没有注册到真实的`dom`上。是绑定在`document`上统一管理的。
2. 真实的`dom`上的`click`事件被单独处理,已经被`react`底层替换成**空函数**。
3. 我们在`react`绑定的事件,比如`onChange`，在`document`上，可能有多个事件与之对应。
4. `react`并不是一开始，把所有的事件都绑定在`document`上，而是采取了一种**按需绑定**，比如发现了`onClick`事件,再去绑定`document click`事件。

##### React事件合成

在`react`中，我们绑定的事件`onClick`等，并不是原生事件，而是由原生事件合成的`React`事件，比如 `click`事件合成为`onClick`事件。比如`blur` , `change` , `input` , `keydown` , `keyup`等 , **合成为`onChange`。**

1. 将事件绑定在`document`统一管理，防止很多事件直接绑定在原生的`dom`元素上。造成一些不可控的情况。
2. 抹平不同浏览器的差异。

### React class component 生命周期

1. 初始化阶段

   发生在 `constructor` 中的内容，在 `constructor` 中进行 `state` 、`props` 的初始化，在这个阶段修改 `state`，不会执行更新阶段的生命周期，可以直接对 `state` 赋值。

2. 挂载阶段

   1. componentWillMount   

      > 发生在 render 函数之前，还没有挂载 Dom 

   2. **render**

   3. componentDidMount   

      > 发生在 render 函数之后，已经挂载 Dom

3. 更新阶段

   * state 更新
     1. shouldComponentUpdate
     2. render 
     3. componentDidUpdate
   * props 更新
     1. shouldComponentUpdate(nextProps,nextState)
     2. render 
     3. componentDidUpdate(prevProps, prevState)

4. 卸载阶段

   * componentWillUnmount

***

### setState 同步、异步

* 如果 `setState` 在 React 能够控制的范围被调用，它就是**异步**的。比如**合成事件处理函数**，**生命周期函数**， 此时会进行批量更新，也就是将状态合并后再进行 DOM 更新。

* 如果 `setState` 在原生 JavaScript 控制的范围被调用，它就是**同步**的。比如原生事件处理函数，定时器回调函数，Ajax 回调函数中，此时 `setState` 被调用后会立即更新 DOM 。

***

### 函数式编程

* 无状态

  数据的不可变性，这意味着如果你想修改一个对象，那你应该创建一个新的对象用来修改，而不是修改已有的对象。

* 无副作用

  给定相同的输入，给出相同的输出，完全不依赖外部状态的变化。（箭头函数）

**优点**

* 便与测试与优化

  > 纯函数对于相同的输入永远会返回相同的结果，因此我们可以轻松断言函数的执行结果，同时也可以保证函数的优化不会影响其他代码的执行。

* 可缓存性

  > 相同的输入总是可以返回相同的输出，因此，我们可以提前缓存函数的执行结果。

* 更少的 Bug

  > **不存在指向不明的 this，不存在对全局变量的引用，不存在对参数的修改**

***





## React Hooks

### 不能在条件语句中写 Hook

[React Hook 必须按顺序、不能在条件语句中调用](https://juejin.cn/post/6939766434159394830#heading-0)

**不要在循环，条件或嵌套函数中调用 Hook**

hook 在每次渲染时的查找是根据一个“全局”的下标对链表进行查找的，如果放在条件语句中使用，有一定几率会造成拿到的状态出现错乱。hook底层并没有为每一个element增加【key】，是为了更好的进行逻辑复用，并且减少bug。如果任何子函数，任何条件表达式中都可以调用 Hook，代码也会变得更加**难以理解和维护**。



### Hooks 组件更新过程

1. 因为某个事件 `state` 发生变化。
2. React 内部更新 `state` 变量。
3. React 处理更新组件中 return 出来的 DOM 节点（进行一系列 dom diff 、调度等流程）。
4. 将更新过后的 DOM 数据绘制到浏览器中。
5. 用户看到新的页面。



### useState

**每次渲染都是独立的**，**每次渲染具有独立的状态值**。





### useEffect

- 将初次渲染（`componentDidMount`）、重渲染（`componentDidUpdate`）和销毁（`componentDidUnmount`）三个阶段的逻辑用一个统一的 API 去解决
- 把相关的逻辑都放到一个 Effect 里面（例如 `setInterval` 和 `clearInterval`），更突出逻辑的内聚性

**重点：**

1. `deps` 数组在判断元素是否发生改变时同样也使用了 `Object.is` 进行比较。因此一个隐患便是，当 `deps` 中某一元素为非原始类型时（例如函数、对象等），**每次渲染都会发生改变**，从而失去了 `deps` 本身的意义（条件式地触发 Effect）。
2. useEffect不能为async 函数，`useEffect` 约定 Effect 函数要么没有返回值，要么返回一个 Cleanup 函数。而这里 async 函数会隐式地返回一个 Promise，直接违反了这一约定，会造成不可预测的结果。





### useEffect 和 useLayoutEffect 区别

1. 更新时机不同，useEffect执行在第4步后（渲染之后），useLayoutEffect执行在第3步到第4步之间，（渲染之前）
2. useEffect异步执行，不会阻塞浏览器进程；uselayoutEffect同步





#### useEffect 依赖为空数组与 componentDidMount 区别

在 `render` 执行之后，`componentDidMount` 会执行，如果在这个生命周期中再一次 `setState` ，会导致再次 `render` ，返回了新的值，浏览器只会渲染第二次 `render` 返回的值，这样可以避免闪屏。

但是 `useEffect` 是在真实的 DOM 渲染之后才会去执行，这会造成两次 `render` ，有可能会闪屏。

实际上 `useLayoutEffect` 会更接近 `componentDidMount` 的表现，它们都同步执行且会阻碍真实的 DOM 渲染的。





#### React.useCallback() 和 React.useMemo() 的区别

- `useCallback` 可缓存函数，其实就是避免每次重新渲染后都去重新执行一个新的函数。
- `useMemo` 可缓存值。



**重点：**

有很多时候，我们在 `useEffect` 中使用某个定义的外部函数，是要添加到 `deps` 数组中的，如果不用 `useCallback` 缓存，这个函数在每次重新渲染时都是一个完全新的函数，也就是引用地址发生了变化，这就会导致 `useEffect` 总会无意义的执行。

***





### React Vdom diff 算法

**虚拟DOM**简而言之就是，用JS去按照DOM结构来实现的树形结构对象，你也可以叫做**DOM对象**

**createElement()** 方法也是vue和react用来创建虚拟DOM的方法，我们也叫这个名字，方便记忆。接收三个参数，分别是**type**，**props**和**children**

- 参数分析
  - type: 指定元素的标签类型，如'li', 'div', 'a'等
  - props: 表示指定元素身上的属性，如class, style, 自定义属性等
  - children: 表示指定元素是否有子节点，参数以数组的形式传入



##### Diff

* 假设

  1. 两个不同类型的元素会产生出不同的树
  2. 开发者可以通过 `key` prop 来暗示哪些子元素在不同的渲染下能保持稳定

* **策略**

  1. Web UI 中 DOM 节点跨层级的移动操作特别少，可以忽略不计
  2. 拥有相同类型的两个组件将会生成相似的树形结构，拥有不同类型的两个组件将会生成不同树形结构
  3. 对于同一层级的一组子节点，它们可以通过唯一 key 进行区分

* **diff 具体优化**

  - tree diff

    React 只对虚拟 DOM 树进行**分层比较**，不考虑节点的跨层级比较。

  - component diff

    1. 如果是同类型组件，首先使用 `shouldComponentUpdate()`方法判断是否需要进行比较，如果返回`true`，继续按照 React diff 策略比较组件的虚拟 DOM 树，否则不需要比较

    2. 如果是不同类型的组件，则将该组件判断为 dirty component，从而替换整个组件下的所有子节点

  - element diff

    React 首先会对新集合进行遍历，通过唯一 key 来判断老集合中是否存在相同的节点，如果没有则创建，如果有的，则判断是否需要进行**移动**操作。并且 React 对于移动操作也采用了比较高效的算法，使用了一种顺序优化手段，这里不做详细讨论。







### React Fiber





### React 性能优化手段

* 使用 `React.memo` 来缓存组件。
* 使用 `React.useMemo` 缓存大量的计算。
* 避免使用匿名函数。
* 利用 `React.lazy` 和 `React.Suspense` 延迟加载不是立即需要的组件。
* 尽量使用 CSS 而不是强制加载和卸载组件。
* 使用 `React.Fragment` 避免添加额外的 DOM。



