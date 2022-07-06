#### 面试题
##### 1. v-show 和 v-if的区别
- v-show是通过css 的 display来控制
- v-if是vue本身的机制去控制组件的动态显示和销毁
- 频繁切换用 v-show 

##### 2. 为何v-for中要用key
- 必须用key,且不能是index和random
- diff算法中通过tag和key来判断，是否是sameNode 
- 减少渲染次数，提升渲染性能

##### 3. 描述vue组件生命周期（有父子组件额情况）
- 单组件生命周期图
- 父子组件生命周期关系

##### 4. vue组件如何通讯
- 父子组件 props和 this.$emit  
- 兄弟组件或不相关组件，用自定义事件的方式  event.$on  event.$off  event.$emit
- vuex  

##### 5. 描述组件渲染和更新过程
==== 一个组件渲染到页面，修改 data 触发更新（数据驱动视图）
- 初次渲染过程
- - 1. 解析模板为 render 函数（或在开发环境已完成，vue-loader）
- - 2. 触发响应式，监听 data 属性 getter setter (执行render过程中 触发data的getter)
- - 3. 执行 render 函数，生成 vnode,patch(elem,vnode)
- 更新过程
- - 1. 修改 data,触发 setter (此前在getter中已被监听  未在getter中监听的数据修改，不会触发setter)
- - 2. 重新执行 render 函数，生曾 newVnode
- - 3. patch(vnode,newVode)
- 异步渲染
- - 1. $nextTick 
- - - 异步渲染，$nextTick 待 DOM 渲染完再回调
- - - 页面渲染时会将 data 的修改做整合，多次 data 修改只会渲染一次
- - 2. 汇总 data 的修改，一次性更新视图
- - 3. 减少 DOM 操作次数，提高性能

##### 6. 双向数据绑定v-model的实现原理 [查看vue官网](https://cn.vuejs.org/v2/guide/components-custom-events.html#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%BB%84%E4%BB%B6%E7%9A%84-v-model)
- input元素的value=this.name
- 绑定input事件 this.name = $event.target.value
- data更新触发 re-render

##### 7. vue原理的三大模块
- 响应式 监听属性变化
- - Object.defineProperty 中的getter setter方法 + 深度监听
- 模板编译 template->render->vnode
- - question：描述组件渲染和更新过程：（重点！！！！）
- - 模板编译成render函数，执行render函数会触发Data某些属性的getter,收集依赖到watcher（收集那些data中触发getter的属性），当data发生变化时，会触发setter，会通知（notify）watcher看之前是否已经通知过了,如果通知过了就会重新执行render函数。
- vdom  (可参考snabbdom实现)

##### 8. computed有何特点
- 缓存，data不变不会重新计算
- 提高性能

##### 9. 为何组件data必须是一个函数
- vue其实是一个Class类，每个vue组件相当于类的实例。如果data是一个对象的话，每个实例就会共享这个data，一个组件修改，另一个组件也跟着修改。如果data是个函数的话，调用data会产生闭包，每个组件的数据都在各自的闭包里，互不影响。

##### 10. Ajax请求应该放在哪个生命周期
- mounted
- JS是单线程的，Ajax异步获取数据
- 放在mounted之前没有用，只会让逻辑更加混乱
- 一般情况下，都放在mounted中，保证逻辑的统一性。因为生命周期是同步执行的，ajax是异步执行的。
- 服务端渲染不支持mounted方法，所以在服务端渲染的情况下统一放在created中。

##### 11. 如何自己实现v-model
```js
Vue.component('base-input', {
    model: {
        prop: 'text',
        event: 'input'
    },
    props: {
        text: Boolean
    },
    template: `
        <input
          type="text"
          :value="text"
          @input="text = $event.target.value"
        >
    `
})
```

##### 12. 何时需要使用keep-alive
- 缓存组件，不需要重复渲染
- 如多个静态tab的切换
- 优化性能

##### 13. 多个组件有相同的逻辑，如何抽离
- mixin
- 以及mixin的一些缺点

##### 14. 何时需要使用beforeDestroy
- 解绑自定义事件 event.$off
- 清除定时器
- 解绑自定义的DOM事件，如window  scroll 等

##### 15. vuex中action和mutation有何区别
- action中处理异步，mutation不可以
- mutation做原子操作
- action可以整合多个mutation

##### 16. 请用vnode描述一个DOM结构
```js
{
    tag:"div",
    props:{
        className:"contianer",
        id:"id1"
    },
    children:[
        {
            tag:"ul",
            props:{
                className:"ul",
            },
            children:[
                {
                    tag:"li",
                    props:{
                        className:"li",
                    },
                    children:"i am li"
                }
            ]
        }
    ]
}
```

##### 17. vue如何监听数组变化
- Object.defineProperty 不能监听数组变化（解决方案见22题）
- 重新定义原型，重写 push pop 等方法，实现监听(再看看视频)
- Proxy可以原生支持监听数组变化

##### 18. diff算法的时间复杂度
- O(n)
- 在O(n^3)基础上做了一些调整
- 具体调整：
- -  a. 只比较同一层级，type不相同直接销毁重建；
- -  b. 通过type(or sel)和key判断是不是同一个组件，是同一个组件就不再重复对比；

##### 19. 简述diff算法过程
- patch(elem,vnode)和patch(vnode,vnode)
- patchVnode 和 addVnodes 和 removeVnodes
- updateChildren (key的重要性)

##### 20. vue为何是异步渲染，$nextTick何用
-异步渲染（以及合并data修改），以提高渲染性能
- $nextTick在DOM更新完之后，触发回调

##### 21. vue常见性能优化方式
- 1. 合理使用v-show和v-if
- 2. 合理使用computed
- 3. v-for时加key,以及避免和v-if同时使用（v-for的优先级高）
- 4. 自定义事件、DOM事件及时销毁
- 5. 合理使用异步组件
- 6. 合理使用keep-alive
- 7. data层级不要太深（对data做响应式监听时，深度监听是一次性遍历完，这就导致计算深度比较多，导致页面卡顿）
- 8. 使用vue-loader在开发环境做模板编译（预编译）
-9. webpack层面的优化
- 10. 前端通用的性能优化，如图片懒加载等
- 11. 使用SSR

##### 22. Object.defineProperty缺点
- 1. 深度监听，需要递归到底，一次性计算量大
- 2. 无法监听新增属性/删除属性（Vue.set Vue.delete）
- 3. 无法原生监听数组，需要特殊处理
```js
 //特殊处理：使之能监听原生数组
  const oldArrayProto = Array.prototype;
  //Object.create() 创建新对象，原型指向oldArrayProto，再扩展新的方法不会影响原型
  const arrPrototype = Object.create(oldArrayProto);
  ["push","pop"].forEach(name=>{
      //arrPrototype的__proto__指向oldArrayProto，这种处理会使arrPrototype某些方法（push or pop or ...）除了继承原生array的方法外，又添加了单独的逻辑
      arrPrototype[name]=function(){
          //触发视图更新----这就相当于单独的逻辑
          console.log(this,"this");
          oldArrayProto[name].call(this,...arguments)
      }
  });
   let arr=[1,2,3];
   arr.__proto__ = arrPrototype;
  //  arr.push(6)

  //  console.log(arr)
```

#### 如何理解MVVM
- 数据驱动视图
- - 传统组件，只是静态渲染，更新还要依赖于操作DOM
- - 数据驱动试图-vue MVVM
- - 数据驱动视图 - React setState

#### vue高级特性
1. 自定义v-model
2. $nextTick
3. slot 
4. 动态、异步组件
5. keep-alive
6. mixin

#### 自定义v-model
> parent component
```
<template>
<div class="parent">
  <p>我是父亲, 对儿子说： {{sthGiveChild}}</p>
  <Child v-model="sthGiveChild"></Child>
</div>
</template>
<script>
import Child from './Child.vue';
export default {
  data() {
    return {
      sthGiveChild: '给你100块'
    };
  },
  components: {
    Child
  }
}
</script>
```
> child component
```
<template>
<div class="child">
  <p>我是儿子，父亲对我说： {{give}}</p>
  <a href="javascript:;"rel="external nofollow" @click="returnBackFn">回应</a>
</div>
</template>
<script>
export default {
  props: {
    give: String
  },
  model: {
    prop: 'give',
    event: 'returnBack'
  },
  methods: {
    returnBackFn() {
      this.$emit('returnBack', '还你200块');
    }
  }
}
</script>
```
> 总结：子组件的model属性prop默认是value,event默认是input。

#### slot
1. 基本使用
2. 作用域插槽
3. 具名插槽
> 单个插槽
```
parent component
<Child url="/profile">
  Your Profile
</Child>
child component
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
```
> 作用域插槽(父组件能拿到子组件的数据)  
> 有时让插槽内容能够访问子组件中才有的数据是很有用的  
> [vue中slot详解](https://cn.vuejs.org/v2/guide/components-slots.html)  
> 为了让 user 在父级的插槽内容中可用，我们可以将 user 作为 <slot> 元素的一个 attribute 绑定上去：
```
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```
> 绑定在 <slot> 元素上的 attribute 被称为插槽 prop。现在在父级作用域中，我们可以使用带值的 v-slot 来定义我们提供的插槽 prop 的名字：
```
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```
> 在这个例子中，我们选择将包含所有插槽 prop 的对象命名为 slotProps，但你也可以使用任意你喜欢的名字。
  
> 具名插槽
```
parent component
<Child>
  <template v-slot:header>
    <h1>Here might be a page title</h1>
  </template>

  <template v-slot:default or 什么也不写>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template v-slot:footer>
    <p>Here's some contact info</p>
  </template>
</Child>
child component
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
```

#### vue如何异步加载组件
1. import()函数
2. 按需加载，异步加载组件(什么时候用什么时候加载)
3. vue常见性能优化
> 如果组件很大 可用此方式，很常用
```
export default{
  components:{
    A:()=>import(../a.vue)
  }
}
```
#### vue如何缓存组件
##### keep-alive
1. 缓存组件
2. 频繁切换，不需要重复渲染
3. vue常见性能优化
> 比如tab切换

#### vue组件如何抽离公共逻辑
##### mixin
1. 多个组件有相同的逻辑，抽离出来
2. mixin并不是完美的解决方案，会有一些问题
3. Vue3提出的Composition API旨在解决这些问题
##### mixin的问题
1. 变量来源不明确，不利于阅读
2. 多mixin可能造成命名冲突
3. mixin和组件可能出现多对多的关系，复杂度高
