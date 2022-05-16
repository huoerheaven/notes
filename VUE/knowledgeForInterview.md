### 面试题
1. v-show 和 v-if的区别
> v-show是通过css 的 display来控制，v-if是vue本身的机制去控制组件的动态显示和销毁。
2. 为何v-for中要用key
3. 描述vue组件生命周期（有父子组件额情况）、
4. vue组件如何通讯
> prop $emit (父子组件)  
> 兄弟组件或不相关组件，用自定义事件的方式  
> vuex  
5. 描述组件渲染和更新过程
6. 双向数据绑定v-model的实现原理


### vue高级特性
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
