#### mixin混入
1. 组件data,methods 优先级高于mixin的data,methods 优先级
2. 生命周期函数，先执行mixin里面的，再执行组件里面的
3. 自定义的属性，组件中的属性优先级高于mixin属性的优先级（this.$options.XX获取组件的自定义属性）
#### 自定义指令
```
//局部自定义指令
const directive={
  focus:{
    mounted(el){
      el.focus();
    }
  }
}
const app = Vue.createApp({
  data(){
    reutrn{
    }
  },
  template:`
    <div>
      <input v-focus />//v-focus是自定义指令
    </div>
  `
  //全局自定义指令
  app.directive("focus",{
    mounted(el){
      el.focus();
    }
  })
})
```
> 自定义指令补充
```
const app = Vue.createApp({
  data(){
    reutrn{
      instance:100
    }
  },
  template:`
    <div v-pos:right="instance">
      <input v-focus />//v-focus是自定义指令
    </div>
  `
});

//全局自定义指令
  app.directive("pos",{
    mounted(el,binding){
      el.style[bindign.arg] = binding.value+"px";//bindign.arg是v-pos:right="instance"的right,bindign.value是v-pos:right="instance"的instance
    },
    updated(el,binding){
      el.style[bindign.arg] = binding.value+"px";//bindign.arg是v-pos:right="instance"的right,bindign.value是v-pos:right="instance"的instance
    }
  })
```
#### teleport 传送门
> 可以将将组件中的dom元素传送到其他位置
```
const app = Vue.createApp({
  data(){
    reutrn{
      msg:"hello world"
    }
  },
  template:`
    <div class="area">
      <button>点击</button>
      <teleport to="body">//将mask传送到body.也可以写成to="#hello"
          <div class="mask">我是蒙层</div>
      </teleport>
    </div>
  `
});
```
#### 更加底层的render函数
1. template->render->h函数->虚拟DOM(JS对象)->真实DOM->展示到页面上
```
const app = Vue.createApp({
    data(){
      return{
        level:2
      }
    },
    render(){
      const {h} = Vue;
      return h("h"+this.leval,{},["hello",h("h4",{},"h4")])
    }
});
```
#### 插件plugin,也是把通用性的功能封装起来
```
const myPlugin={
  install(app,options){
    app.provide("name","Weiwei");
    app.directive("focus",{});
    app.mixin({});
    app.config.globalProperties.$sayHello = "hello world";//定义app的属性
  }
};
const app = Vue.createApp({
    inject:["name"]
    data(){
      return{
      }
    },
    mounted(){
      console.log(this.$sayHello)
    },
    template:`
      <div>hello</div>
    `
});
app.use(myPlugin,{a:"qqq"});//{a:"qqq"}就是options
```
> plugin案例：数据校验插件开发实例
```
const app = Vue.createApp({
    data(){
      return{
          age:22,
          name:"HuoWeiwei"
      }
    },
    rules:{
      age:{
        validate:age=>age>25,
        message:"age is too young"
      },
      name:{
        validate:name=>name.length>5,
        message:"name is too short"
      },
    },
    mounted(){
    },
    template:`
      <div>hello</div>
    `
});

const validatePlugin={
  install(app,options){}
}
//等价于
const validatePlugin=(app,options)=>{}

//vue组件自定义的属性 通过this.$options.xx获取到
//vue组件自带的属性通过this.$XX获取到，比如this.$data,this.$watch
const validatePlugin=(app,options)=>{
    app.mixin({
      create(){
        for(let key in this.$options.rules){
          const item = thid.$options.rules[key];
          this.$watch(key,value=>{
            const result = item.validate(value);
            if(!result) console.log(item.message)
          })
        }
      }
    })
    
}

app.use(validatePlugin);
```
