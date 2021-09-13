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
    <div>{{age}}</div>
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
    return {age}
  }
})
```
> setup函数的其中一个参数 context   
```
const app=Vue.createApp({
  template:`
    <child @change="handleChange" app="app"/>
  `,
  methods(){
    handleChange(){...}
  }
  
});
app.component("child",{
  template:`
    <div>child</div>
  `,
  setup(props,context){
    const {h} = Vue;
    const {attrs,slots,emit}=context;
    //attrs
    //console.log(attrs.app);//attrs包含的是None-props属性 （父组件传递的属性，子组件没有用props接收的属性就是None-props属性）
    //slots
    //return ()=>h("div",{},slots.default());
    //emit
    function handleClick (){ emit("change")};
    return {handleClick}
  }
})
```
#### watch 和 watchEffect 的使用和差距
> watch 
```
const app=Vue.createApp({
  setup(){
    const {reactive}  = Vue;
    const nameObj = reactive({name:"WEIWEI",engName:"HUOWEIWEI"});
    //具备一定的惰性lazy
    //参数可以拿到原始值和当前值
    //参数可以是数组
    watch([()=>nameObj.name,()=>nameObj.engName],([curName,curEng],[prevName,preEng])=>{})
  },
  template:`
    <div>
      Name:<input v-model = "name">
      <span>name is {{name}}</span>
    </div>
    <div>
      engName:<input v-model = "engName">
      <span>engName is {{engName}}</span>
    </div>
  `,
  
});
```
> watchEffect 侦听器 偏向于 effect
```
const app=Vue.createApp({
  setup(){
    const {reactive}  = Vue;
    const nameObj = reactive({name:"WEIWEI",engName:"HUOWEIWEI"});
    //具备一定的惰性lazy
    //参数可以拿到原始值和当前值
    //参数可以是数组
    watch([()=>nameObj.name,()=>nameObj.engName],([curName,curEng],[prevName,preEng])=>{})
    
    //立即执行 没有惰性 immediate
    //不需要传递你要侦听的内容，自动会感知代码依赖，不需要传递很多参数，只要传递一个回调函数
    //不能获取之前数据的值
    watchEffect(()=>{
      console.log("abc");//只有第一次加载的时候被执行 因为代码中没有对外部的依赖
      console.log(nameObj.name);//第一次加载的时候和 每当改变nameObj.name的时候 都会执行watchEffect，因为代码中有对外部的依赖nameObj.name
    });
    //取消侦听
    const stop = watchEffect(()=>{
      setTimeout(()=>{
        stop();
      },3000)
    });
    
    
  },
  template:`
    <div>
      Name:<input v-model = "name">
      <span>name is {{name}}</span>
    </div>
    <div>
      engName:<input v-model = "engName">
      <span>engName is {{engName}}</span>
    </div>
  `,
  
});
```
> watch 非惰性侦听
```
const app=Vue.createApp({
  setup(){
    const {reactive}  = Vue;
    const nameObj = reactive({name:"WEIWEI",engName:"HUOWEIWEI"});
    //具备一定的惰性lazy
    //参数可以拿到原始值和当前值
    //参数可以是数组
    watch(
      [()=>nameObj.name,()=>nameObj.engName],
      ([curName,curEng],[prevName,preEng])=>{},
      {immediate:true}//添加此代码即可实现非惰性
    )
  },
  template:`
    <div>
      Name:<input v-model = "name">
      <span>name is {{name}}</span>
    </div>
    <div>
      engName:<input v-model = "engName">
      <span>engName is {{engName}}</span>
    </div>
  `,
  
});
```
#### 生命周期函数新写法
```
const app=Vue.createApp({
  setup(){
    //beforeMount->onBeforeMount
    //mounted->onMounted
    //beforeUpdate->onBeforeUpdate
    //updated->onUpdated
    //beforeUnmount->onBeforeUnmount
    //unmouted->onUnmounted
    const {onBeforeMount,onMounted,onRenderTracked,onRenderTriggered}=Vue;
    onBeforeMount(()=>{});
    onMounted(()=>{})
    //onRenderTracked 每次页面渲染都会触发此钩子函数
    //onRenderTriggered 页面渲染过一次，再次重新渲染会触发此钩子函数
  },
  template:`
    <div>
      Name:<input v-model = "name">
      <span>name is {{name}}</span>
    </div>
    <div>
      engName:<input v-model = "engName">
      <span>engName is {{engName}}</span>
    </div>
  `,
  
});
```
#### provide,inject,模板Ref的用法
```
const app=Vue.createApp({
  setup(){
  const {provide} = Vue;
  const name = ref("huoweiwei");
  provide("name",readonly(name));
  provide("fnChangeName",()=>{
    name.value="mmm";//这里可以修改
  })
  },
  template:`
    <div></div>
  `,
  
});
app.component("child",{
  setup(){
    const {inject} = Vue;
    const name  = inject("name","hello");//第二个参数hello为默认值
    const fnChangeName = inject("fnChangeName");
    
    const handleClick=()=>{
      //修改父组件数据
      fnChangeName(name);//可以
      //name.value = "bbb";//不能修改，因为此name值用了readonly,这种方法就防止在子组件改父组件的数据,约束了单向数据流的概念
    }
    return {name,handleClick}
  },
  template:`
    <div @click="handleClick">{{name}}</div>
  `
})
```
