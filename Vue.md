# Vue

### Vue 3 新特性

1. 重写双向绑定

   * Vue 2基于Object.definProperty() 实现
   * Vue 3基于Proxy

2. 优化Vdom

   Vue 2 中每次更新diff，都是全量对比，Vue 3 则只对比带有标记的，大大减少了非动态内容的对比消耗。patch flag优化静态树。

3. Fragment

4. Tree shaking

5. Composition API



### 模板语法

* v-text 用来显示文本
* v-html 用来展示富文本
* v-if 用来控制元素的显示隐藏（切换真假DOM）
* v-else-if 表示 v-if 的“else if 块”。可以链式调用
* v-else v-if  条件收尾语句
* v-show 用来控制元素的显示隐藏（display none block Css切换）
* **v-on  简写 @ 用来给元素添加事件**
* **v-bind 简写 :  用来绑定元素的属性Attr**
* v-model 双向绑定
* v-for 用来遍历元素



### 虚拟DOM

虚拟DOM就是通过JS来生成一个AST节点树。一个DOM上的属性非常的多，直接操作DOM非常损耗性能，我们可以用JS的计算性能来换取操作DOM所消耗的性能。尽可能少的操作DOM。

##### Diff算法

1. 前序对比算法
2. 尾序对比算法

尽可能的复用节点。



### Vue3生命周期

* `beforeCreate` `crated`  的Hook为 `setup()`
* `beforeMount` 的 Hook为 `onBeforeMount()`
* `mounted` 的Hook为 `onMounted()`
* `beforeUpdate` 的Hook为 `onBeforeUpdate()`
* `updated` 的Hook为 `onUpdated()`
* `beforeUnmount` 的Hook为 `onBeforeUnmount()`
* `unmounted` 的Hook为 `onUnmounted()`
  * `errorCaptured` 的Hook为 `onErrorCaptured()`
  * `renderTracked` 的Hook为 `onRenderTracked()`
  * `renderTriggered` 的Hook为 `onRenderTriggered()`
  * `activated` 的Hook为 `onActivated()`
  * `deactivated` 的Hook为 `onDeactivated()`

 

### Ref

接受一个内部值并返回一个响应式且可变的 ref 对象。ref 对象仅有一个 `.value` property，指向该内部值。

##### isRef

##### shallowRef

##### triggerRef 

强制更新页面DOM

##### customRef

customRef 是个工厂函数要求我们返回一个对象 并且实现 get 和 set



***

### Reactive

用来绑定复杂的数据类型。例如：对象、数组。

##### readonly

拷贝一份proxy对象将其设置为只读

##### shallowReactive 

只能对浅层的数据 如果是深层的数据只会改变值 不会改变视图



### to系列全家桶

* toRef

* toRefs

  可以帮我们批量创建ref对象主要是方便我们解构使用

* toRaw

  将响应式对象转化为普通对象

***

### computed

计算属性就是当依赖的属性的值发生变化的时候，才会触发他的更改，如果依赖的值，不发生变化的时候，使用的是缓存中的属性值。



### 全局组件

例如组件使用频率非常高（table，Input，button，等）这些组件 几乎每个页面都在使用便可以封装成全局组件。



### 动态组件

让多个组件使用同一个挂载点，并动态切换。

**注意事项** 

1. 在Vue2 的时候is 是通过组件名称切换的 在Vue3 setup 是通过组件实例切换的。
2. 如果你把组件实例放到Reactive Vue会给你一个警告，这是因为reactive 会进行proxy 代理 而我们组件代理之后毫无用处 节省性能开销 推荐我们使用shallowRef 或者 markRaw 跳过proxy 代理。



### 插槽 slot

##### 匿名插槽

##### 具名插槽

具名插槽其实就是给插槽取个名字。一个子组件可以放多个插槽，而且可以放在不同的地方，而父组件填充内容时，可以根据这个名字把内容填充到对应插槽中。

##### 作用域插槽

##### 动态插槽





### 异步组件

1. 顶层 await
2. defineAsyncComponent



##### suspence

`<suspense>` 组件有两个插槽。它们都只接收一个直接子节点。`default` 插槽里的节点会尽可能展示出来。如果不能，则展示 `fallback` 插槽里的节点。 

     <Suspense>
            <template #default>
                <Dialog>
                    <template #default>
                        <div>我在哪儿</div>
                    </template>
                </Dialog>
            </template>
     
            <template #fallback>
                <div>loading...</div>
            </template>
        </Suspense>



### Teleport

`Teleport` 是一种能够将我们的模板渲染至指定`DOM`节点，不受父级`style`、`v-show`等属性影响，但`data`、`prop`数据依旧能够共用的技术；类似于 `React` 的 `Portal`。

* 制作模态组件



### 内置组件

##### keep-alive缓存组件

有时候我们不希望组件被重新渲染影响使用体验；或者处于性能考虑，避免多次重复渲染降低性能。而是希望组件可以缓存下来,维持当前的状态。这时候就需要用到`keep-alive`组件。

##### transition动画组件

* **过渡的类名**

  * v-enter-from：定义进入过渡的开始状态。在元素被插入之前生效，在元素被插入之后的下一帧移除。
  * v-enter-active：定义进入过渡生效时的状态。在整个进入过渡的阶段中应用，在元素被插入之前生效，在过渡/动画完成之后移除。这个类可以被用来定义进入过渡的过程时间，延迟和曲线函数。
  * v-enter-to：定义进入过渡的结束状态。在元素被插入之后下一帧生效 (与此同时 v-enter-from 被移除)，在过渡/动画完成之后移除。

  

  * v-leave-from：定义离开过渡的开始状态。在离开过渡被触发时立刻生效，下一帧被移除。
  * v-leave-active：定义离开过渡生效时的状态。在整个离开过渡的阶段中应用，在离开过渡被触发时立刻生效，在过渡/动画完成之后移除。这个类可以被用来定义离开过渡的过程时间，延迟和曲线函数。
  * v-leave-to：离开过渡的结束状态。在离开过渡被触发之后下一帧生效 (与此同时 v-leave-from 被移除)，在过渡/动画完成之后移除。

* **transition 生命周期8个**

  ```css
    @before-enter="beforeEnter" //对应enter-from
    @enter="enter"//对应enter-active
    @after-enter="afterEnter"//对应enter-to
    @enter-cancelled="enterCancelled"//显示过度打断
    @before-leave="beforeLeave"//对应leave-from
    @leave="leave"//对应enter-active
    @after-leave="afterLeave"//对应leave-to
    @leave-cancelled="leaveCancelled"//离开过度打断
  ```



##### transition-group过度列表









### 依赖注入Provide / Inject

provide 可以在祖先组件中指定我们想要**提供**给后代组件的数据或方法，而在任何后代组件中，我们都可以使用 inject 来接收 provide **提供**的数据或方法。 





### vue组件的通信方式

- `props`/`$emit` 父子组件通信

  父->子`props`，子->父 `$on、$emit` 获取父子组件实例 `parent、children` `Ref `获取实例的方式调用组件的属性或者方法 父->子孙 `Provide、inject` 官方不推荐使用，但是写组件库时很常用

- `$emit`/`$on` 自定义事件 兄弟组件通信

  `Event Bus` 实现跨组件通信 `Vue.prototype.$bus = new Vue()` 自定义事件

- vuex 跨级组件通信

  Vuex、`$attrs、$listeners` `Provide、inject`







### Watch

`watch` 需要侦听特定的数据源，并在单独的[回调函数](https://so.csdn.net/so/search?q=回调函数&spm=1001.2101.3001.7020)中执行副作用。

watch第一个参数监听源

watch第二个参数回调函数cb（newVal,oldVal）

watch第三个参数一个options配置项是一个对象

```javascript
{

immediate:true //是否立即调用一次

deep:true //是否开启深度监听

}
```

* 监听多个ref，第一个参数为一个数组。
* 使用reactive监听深层对象开启和不开启deep 效果一样
* 监听reactive 单一值时，第一个参数为一个箭头函数





### watchEffect

立即执行传入的一个函数，同时响应式追踪其依赖，并在其依赖变更时重新运行该函数。

如果用到message 就只会监听message 就是用到几个监听几个 而且是非惰性 会默认调用一次。

**作用：**

* 清除副作用







### Vite 对比 Webpack

#### Vite

基于浏览器原生ES模块的开发服务器。利用浏览器去解析模块，在服务器端按需编译返回，完全跳过了打包这个概念，服务器随起随用。

**特点：**

- 闪电般的冷启动速度
- 即时热模块更换（热更新）
- 真正的按需编译

在 webpack-dev-server 领域上，Vite就是银弹。当我们使用webpack时，我们需要将所有的代码打包成一个或多个bundle.js，然后在交给浏览器。当我们修改了其中一个文件的代码时，整个bundle都需要重新打包。随着项目规模的扩大，重新打包（热更新）的时间越来越长。

**实现原理：**





**热更新（Hot Module Reload）原理**

Vite的热加载原理，实际上就是在客户端与服务端建立了一个websocket链接，当代码被修改时，服务端发送消息通知客户端去请求修改模块的代码，完成热更新。



Webpack原理

优缺点对比









当数据更新时，Vue执行异步的DOM渲染，会先缓存数据在异步的事件循环中，防止不必要的渲染**（批量渲染）**。如果想在数据变化之后，立刻获取到更新后的DOM节点。优先Promise，其次setTimeout()。因为Promise的构造函数本身是同步的。 