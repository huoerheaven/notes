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
