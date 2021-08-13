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
