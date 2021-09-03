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
>> 1. v-html  
>> 2. v-bind  （简写为：）
>> 3. v-once 只渲染一次 降低无用渲染，提高性能
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
>> 4. v-show
>> 5. v-on 事件绑定 （简写为@）
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
>> 1. 组织默认行为
```
    template:`<div :[name]="message" @[event].prevent=handleClick>{{message}}</div>`  //修饰符.prevent
```

 

