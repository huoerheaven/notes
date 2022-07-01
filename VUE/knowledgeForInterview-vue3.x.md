##### 1. vue3比vue2有什么优势
- 性能更好
- 体积更小
- 更好的ts支持
- 更好的代码组织
- 更好的逻辑抽离
- 更多新功能

##### 2. Options API生命周期
- beforeDestroy改为beforeUnmount
- destroyed改为 unmounted
- 其他沿用vue2的生命周期

##### 3. Composition API对比Options API
- 1. Composition API带来了什么？
- - a. 更好的代码组织
- - b. 更好的逻辑复用
- - c. 更好的类型推导
-2. Composition API和 Options API如何选择
- - a. 不建议共用，会引起混乱
- - b. 小型项目、业务逻辑简单，用Options API
- - c. 中大型项目、逻辑复杂，用Composition API
- 3. 别误解Composition API
- - a. Composition API属于高阶技巧，不是基础必会
- - b. Composition API是为解决复杂业务逻辑而设计
- - c. 就像Hooks在React中的地位

##### 4. toRef
- **一个普通对象要实现响应式用reactive，一个响应式对象的某个属性要单独拿出来使用响应式用toRef**
- 1. 针对一个响应式对象（reactive封装）的prop
- 2. 创建一个ref,具有响应式
- 3. 两者保持引用关系

##### 5. toRefs
- **toRef是针对响应式对象的一个属性，toRefs是针对多个属性**
- 1. 将响应式对象（reactive封装）转换为普通对象
- 2. 对象的每个prop都是对应的ref
- 3. 两者保持引用关系

##### 6. ref toRef toRefs的最佳使用方式
- 1. 用reactive做对象的响应式，用ref做值类型响应式
- 2. setup中返回toRefs(state),或者toRef(state,"xxx")
- 3. ref的变量命名都用xxxRef
- 4. 合成函数返回响应式对象时，使用toRefs(有助于使用方对数据进行解构)

##### 7. 为什么需要ref
- 1. 返回值类型，会丢失响应式
- 2. 如在setup computed 合成函数中，都有可能返回值类型
- 3. Vue如果不定义ref,用户将自造ref,反而混乱

##### 8. 为什么ref需要value属性
- 1. ref是一个对象（不丢失响应式），value存储值
- 2. 通过.value属性的get和set实现响应式
- 3. 用于模板、reactive时，不需要.value，其他情况都需要
```js 
演示：
 * computed返回的是ref类型，实现响应式
*/
//错误
// function computed(getter){
//     let value=0;
//     setTimeout(()=>{
//         value=getter();
//     },1500);
//     return value
// }
//正确
// function computed(getter){
//     let ref={
//         value:0
//     }
//     setTimeout(()=>{
//         ref.value=getter();
//     },1500);
//     return ref
// }
```

##### 8. 为什么需要toRef toRefs
- 1. 初衷：不丢失响应式的情况下，把对象数据分解/扩散（解构赋值）
- 2. 前提：针对的是响应式对象（reactive封装的），非普通对象
- 3. 注意：不创造响应式，而是延续响应式（创造响应式的ref）

##### 9. vue3升级了哪些重要功能
- 1. createApp
- 2. emits属性 
- 3. 生命周期
- 4. 多事件 （@click="aa(),bb()"）
- 5. Fragment (template标签中可以有多个元素)
- 6. 移除.sync 用v-model参数代替 
- 7. 异步组件写法
- 8. 移除filter
- 9. Teleport (可以将组件移到body中)
- 10. Suspense 
- 11. Composition API

##### 10. Composition API实现逻辑复用
- 1. 抽离逻辑代码到一个函数
- 2. 函数命名约定为useXxxx格式（React Hooks也是）
- 3. 在setup中引用useXxxx函数

##### 11. vue3为何比vue2快(编译优化)
- 1. Proxy响应式
- 2. PatchFlag
- -  [PatchFlag](https://vue-next-template-explorer.netlify.app/)
- -  a. 编译模板时，动态节点做标记
- -  b. 标记，分为不同的类型，如TEXT PROPS
- -  c. diff算法时，可以区分静态节点，以及不同类型的动态节点
- 3. hoistStatic
- -  a. 将静态节点的定义，提升到父作用域，缓存起来
- -  b. 多个相邻的静态节点，会被合并起来
- -  c. 典型的拿空间换时间的优化策略
- 4. cacheHandler
- -  a. 缓存事件
- 5. SSR优化
- -  a. 静态节点直接输出，绕过了vdom
- -  b. 动态节点，还是需要动态渲染
- 6. tree-shaking
- -  a. 编译时，根据不同的情况，引入不同的API 

##### 12. Vite为何启动快
- 1. 开发环境使用ES6 Module，无需打包------非常快
- 2. 生产环境使用rollup,并不会快很多
