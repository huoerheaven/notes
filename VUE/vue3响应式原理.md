- [参考](https://www.bbsmax.com/A/xl56YvomJr/)
#### 一、 Proxy
##### 1. Proxy实现响应式优缺点
- 优点（和Object.defineProperty()对比）
- 1. 深度监听，性能更好
- 2. 可监听 新增/删除 属性
- 3. 可监听数组变化
- 缺点：
- 1. Proxy无法兼容所有浏览器，无法polyfill

##### 2. 创建响应式
```js
function reactive(target={}){
    if(typeof target !=="object"||target==null){
        //不是对象或数组，则返回
        return target;
    }
    //代理配置
    const proxyConf = {
        get(target,key,receiver){
            //只处理本身（非原型）属性
            const ownKeys=Reflect.ownKeys(target);
            if(ownKeys.includes(key)){
                console.log("get",key);//处理监听
            }
            const result = Reflect.get(target,key,receiver);

            //深度监听
            //性能如何提升？
            //proxy是什么时候get什么时候递归，不触发get就不会递归  
            //Object.defineProperty()是一开始就一次性递归完成
            return reactive(result);
        },
        set(target,key,value,receiver){
            //重复的数据，不处理(比如说调用数组的push方法时，length就是重复赋值)
            if(value===target[key]){
                return true;
            }

            const ownKeys=Reflect.ownKeys(target);
            if(ownKeys.includes(key)){
                console.log("已有的 key",key)
            }else{
                console.log("新增的 key",key)
            }
            console.log("set",key,value)
            const result = Reflect.set(target,key,value,receiver);
            // console.log(result,'result')
            return result;//是否设置成功
        },
        deleteProperty(target,key){
            console.log("delete property",key)
            const result = Reflect.deleteProperty(target,key);
            return result;//是否设置成功
        }

    }

    //生成代理对象
    const observerd= new Proxy(target,proxyConf);
    return observerd;
}
//测试数据
const data={
    name:"hww",
    age:18,
    info:{
        city:"bj"
    }

}
// const data=[1,2,3]
const proxyData = reactive(data);
```
##### 3. 知识点
1. set 和 deleteProperty 中需要返回布尔类型的值
- 在严格模式下，如果返回 false 的话会出现 Type Error 的异常
2. Proxy 和 Reflect 中使用的receiver
- Proxy 中 receiver：Proxy 或继承Proxy的对象
- Reflect 中 receiver：如果target 对象中设置了getter,getter中的this指向receiver
```js
const obj={
    get foo(){
        return this.bar;
    }
}
const proxy=new Proxy(obj,{
    get(target,key,receiver){
        if(key==="bar"){
            return "value-bar"
        }
        return Reflect.get(target,key,receiver);//不加receiver参数：this指向obj;加receiver，this指向receiver
    }
})
console.log(proxy.bar)
```
#### 二、reactive
1. reactive
- 接收一个参数，判断这个参数是否是对象
- 创建拦截器对象 handler,设置 get/set/deleteProperty
- 返回 Proxy 对象  

2.实现原理
```js
const isObject = target => target!==null&&typeof target==="object";
const convert = target => isObject(target)?reactive(target):target;
const hasOwnProperty = Array.prototype.hasOwnProperty;
const hasOwn = (target,key)=> hasOwnProperty.call(target,key);

export const reactive=function(target){
    if(!isObject(target)) return target;
    const handler={
        get(target,key,receiver){
            //收集依赖
            console.log("get",key);
            const result = Reflect.get(target,key,receiver);
            return convert(result);
        },
        set(target,key,value,receiver){
            const oldVal = Reflect.get(target,key,receiver);
            let result = true;
            if(oldVal!==value){
                result = Reflect.set(target,key,value,receiver);
                //触发更新
                console.log("set",key,value)
            }
            return result;
        },
        deleteProperty(target,key){
            const hasKey = hasOwn(target,key);
            const result = Reflect.deleteProperty(target,key);
            if(hasKey&&result){
                //触发更新
                console.log("delete",key)
            }
            return result;
        }
    }
    
    return new Proxy(target,handler);
}
```
3.使用
```js
<script type="module">
    import {reactive} from "./reactive/index.js";
    const obj=reactive({
        name:"hww",
        age:18
    });
    console.log(obj.name);
    obj.name="yjy";
    delete obj.age;
    console.log(obj);// Proxy{name:"yjy"}
</script>
```
