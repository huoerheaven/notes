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

#### 三、Effect 
1. 实现原理
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
            track(target,key);
            const result = Reflect.get(target,key,receiver);
            return convert(result);
        },
        set(target,key,value,receiver){
            const oldVal = Reflect.get(target,key,receiver);
            let result = true;
            if(oldVal!==value){
                result = Reflect.set(target,key,value,receiver);
                //触发更新
                trigger(target,key);
            }
            return result;
        },
        deleteProperty(target,key){
            const hasKey = hasOwn(target,key);
            const result = Reflect.deleteProperty(target,key);
            if(hasKey&&result){
                //触发更新
                trigger(target,key);
            }
            return result;
        }
    }
    
    return new Proxy(target,handler);
}

let activeEffect = null;//当前活动的effect函数
export const effect=function(callback){
    activeEffect = callback;
    callback();
    //依赖收集完之后 将activeEffect设置为null
    activeEffect = null;
}

let targetMap = new WeakMap();
//收集依赖
export const track=function(target,key){
    if(!activeEffect) return;
    let depsMap = targetMap.get(target);
    if(!depsMap){
        targetMap.set(target,(depsMap=new Map()));
    }
    let dep=depsMap.get(key);
    if(!dep){
        depsMap.set(key,(dep=new Set()));
    }
    dep.add(activeEffect);
}

//触发更新
export const trigger=function(target,key){
    const depsMap = targetMap.get(target);
    if(!depsMap) return;
    const dep = depsMap.get(key);
    if(dep){
        dep.forEach(effect=>{
            effect();
        })
    }
}
```
2. 使用
```js
    import {reactive,effect} from "./reactive/index.js";

    let total;
    const product = reactive({
        price:200,
        count:3
    });

    effect(()=>{
        total = product.price*product.count;
    });
    console.log(total);//600
    product.price=400;
    console.log(total);//1200
    product.count=5;
    console.log(total);//2000
```

#### 四、 ref
1. 实现原理
```js
//ref
export const ref=function(raw){
    //判断 raw 是否是ref创建的对象，如果是的话直接返回
    if(isObject(raw)&&raw.__v_isRef) return raw;
    //raw不是ref创建的对象，则转换成reactive
    let value  = convert(raw);
    let r={
        __v_isRef:true,
        get value(){
            //收集依赖
            track(r,"value"); 
            return value;
        },
        set value(newVal){
            if(value===newVal) return ;
            value=convert(newVal);//重新赋值成对象也是响应式的
            //触发更新
            trigger(r,"value");
        }
    }
    return r;
}
```
2. 使用
```js
<script type="module">
    import {reactive,effect,ref} from "./reactive/index.js";

    let total;
    let price=ref(200);
    let count=ref(3);

    effect(()=>{
        total = price.value*count.value;
    });
    console.log(total);//600
    price.value=400;
    console.log(total);//1200
    count.value =5;
    console.log(total);//2000
</script>
```
3. reactive vs ref  
- ref 可以把基本数据类型数据，转成响应式对象
- ref 返回的对象，重新赋值成对象也是响应式的
- reactive 返回的对象，重新赋值丢失响应式
- reactive 返回的对象不可以解构

#### 五、toRefs
1. 实现原理
```js
//toRefs
//接收一个响应式对象，并且对响应式对象的属性赋值成ref对象
export const toRefs=function(proxy){
    let ret = proxy instanceof Array ? new Array(proxy.length):{};
    for(const key in proxy){
        ret[key] = toProxyRef(proxy,key);
    }
    return ret;
}
function toProxyRef(proxy,key){
    const r={
        __v_isRef:true,
        get value(){
            return proxy[key];
        },
        set value(newVal){
            proxy[key]=newVal;
        }
    };
    return r;
}
```
2. 使用
```js
<script type="module">
    import {reactive,effect,toRefs} from "./reactive/index.js";

    let total;
    function useProduct(){
        const product=reactive({
            name:"iphone",
            price:2000,
            count:3
        })
        return toRefs(product)
    }

    let {price,count} = useProduct();
    console.log(price,count)


    effect(()=>{
        total = price.value*count.value;
    });
    console.log(total);
    price.value=400;
    console.log(total);
    count.value=5;
    console.log(total);
</script>
```

#### 六、computed
1. 实现原理
```js
//computed
//computed接收一个有返回值的函数作为参数，函数的返回值就是计算属性的值，并且要监听函数内部使用的响应式数据的变化
export const computed=function(getter){
    let result = ref();
    effect(()=>result.value = getter());
    return result;
}
```
2. 使用
```js
<script type="module">
    import {reactive,effect,computed} from "./reactive/index.js";


    const product = reactive({
        price:200,
        count:3
    });

    let total=computed(()=>{
        return product.price * product.count;
    })
    console.log(total.value);
    product.price=400;
    console.log(total.value);
    product.count=5;
    console.log(total.value);
</script>
```
