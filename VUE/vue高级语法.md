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
