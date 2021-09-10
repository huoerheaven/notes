#### Setup函数的使用（Composition api最核心的东西）
> 实例被完全初始化之前会自动调用setup函数
#### ref,reactive响应式引用的用法和原理
> 原理，通过proxy对数据进行封装，当数据变化时，触发模板等内容的更新
> ref 处理基础类型的数据  
```
const app=Vue.createApp({
  template:`
    <div>{{name}}</div>
  `,
  setup(props,context){
    const {ref} = Vue;
    //ref就相当于将"huoweiwei"变成proxy({value:"huoweiwei"})这样的一个响应式引用
    let name = ref("huoweiwei");
    setTimeout(()=>{
      name = "yangingyuan";
    },2000);
    return {name}
  }
})
```
> reactive处理非基础类型的数据  
```
const app=Vue.createApp({
  template:`
    <div>{{obj.name}}</div>
  `,
  setup(props,context){
    const {reactive} = Vue;
    //reactive就相当于将{name:"huoweiwei"}变成proxy({name:"huoweiwei"})这样的一个响应式引用
    let obj = reactive({name:"huoweiwei"});
    setTimeout(()=>{
      obj.name = "yangjingyuan";
    },2000);
    return {obj}
  }
})
```
> readonly 只读不能修改
> toRefs 将reactive返回的数据转换成ref的形式  
```
const app=Vue.createApp({
  template:`
    <div>{{name}}</div>
  `,
  setup(props,context){
    const {reactive，toRefs} = Vue;
    //reactive就相当于将{name:"huoweiwei"}变成proxy({name:"huoweiwei"})这样的一个响应式引用
    let obj = reactive({name:"huoweiwei"});
    setTimeout(()=>{
      obj.name = "yangjingyuan";
    },2000);
    const { name } = obj;//因为name属性的值不是响应式的，所以此时页面不会更新变化
    //这么写就可以
    //原理： toRefs将 proxy({name:"huoweiwei"}) 变成 {name:proxy({value:"huoweiwei"})}
    const { name } = toRefs(obj);
    return {name}
  }
})
```
> toRef 
```
const app=Vue.createApp({
  template:`
    <div>{{obj.name}}</div>
  `,
  setup(props,context){
    const {reactive,toRef} = Vue;
    const obj = reactive({name:"huoweiwei"});
    //toRefs
    const age = toRefs(obj);//给age.value赋值时会报错，因为toRefs给age赋值成undefined
    //toRef
    const age = toRef(obj,"age");//给age.value赋值时不会报错，因为toRef会给age复制成null
    setTimeout(()=>{
      age.value = 18;
    },2000);
    return {obj}
  }
})
```
