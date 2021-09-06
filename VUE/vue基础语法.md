## vue基础语法
#### 一、vue生命周期
1. beforeCreate 在vue实例生成之前会自动执行的函数  
2. created 在vue实例生成之后会自动执行的函数  
3. beforeMount 在组件内容被渲染到页面之前自动执行的函数  
4. mounted 在组件内容被渲染到页面之后被自动执行的函数  
5. beforeUpdate 当数据发生变化时会立即执行的函数  
6. updated 当数据发生变化 页面重新渲染后 会自动执行的函数  
7. beforeUnmount 当vue应用失效时，自动执行的函数  
8. unmounted 当vue应用失效时，且dom完全销毁之后，自动执行的函数  

#### 二、常用模板
1. 插值表达式
```
 template:`<div >{{message}}</div>`  //{{message}}
```
2. vue指令  
> 1. v-html  
> 2. v-bind  （简写为：）
> 3. v-once 只渲染一次 降低无用渲染，提高性能
```
 const app = Vue.createApp({
     data(){
         return {
             message:"hello word"
         }
     },
     template:`<div v-once>{{message}}</div>` //首次渲染时 显示hello word,当改变message的内容时，由于v-once指令，该dom元素还是展示的hello world.
 })
```
> 4. v-show
> 5. v-on 事件绑定 （简写为@）
3.动态属性
```
const app = Vue.createApp({
     data(){
         return {
             message:"hello word",
             name:"title",
             event:"click"
             
         }
     },
     template:`<div :[name]="message" @[event]=handleClick>{{message}}</div>`  //:[name]/@[event]的形式
```
4. 修饰符
> 1. 组织默认行为
```
    template:`<div :[name]="message" @[event].prevent=handleClick>{{message}}</div>`  //修饰符.prevent
```
#### 三、数据、方法、计算属性和侦听器（data & methods & computed & watcher）
1. 计算属性computed和方法methods的区别
> computed:当计算属性依赖的内容发生变更时，才会重新执行计算  (内部带有缓存的机制，页面渲染更高效)  
> methods:只要页面重新渲染，就会重新计算（不高效）
```
const app = Vue.createApp({
     data(){
         return {
             message:"hello word",
             price:2,
             count:2
         }
     },
     computed:{
         total(){
             return new Date()+this.count //如果this.count不发生变化 就不会重新计算total
         }
     },
     methods:{
         getTotal(){
            return new Date()+this.count //改变message的值后，页面会重新渲染，就会触发getTotal方法
         }
     },
     template:`<div >{{message}}--{{total}}/{{getTotal()}}</div>`
})
```
2. 侦察器watcher和计算属性computed的区别
> watcher用来处理异步操作。如果没有异步操作，优先使用computed，比较方便；如果有异步操作，则使用watcher.
#### 四、事件绑定
1. 事件修饰符：stop,prevent,capture,self,once,passive
2. 按键修饰符：enter,tab,delete,esc,up,down,left,right
3. 鼠标修饰符：left,right,middle
4. 精确修饰符：exact
#### 五、表单中双向绑定指令的使用
1. input,textarea,checkbox,radio,select
2. 修饰符：lazy,number,trim

