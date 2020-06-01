#### Object.defineProperty()
> 第一个参数代表要设置的对象，第二个参数代表要设置的对象的键值，第三个参数是一个配置对象。
配置对象可以设置参数如下：
```javascript
value: 对应key的值
configurable：是否可以删除该key或者重新配置该key
enumerable：是否可以遍历该key
writable：是否可以修改该key
get: 获取该key值时调用的函数
set: 设置该key值时调用的函数
```
1. `value`
```javascript
 let x = {}
 x['name'] = 'vue'
 console.log(Object.getOwnPropertyDescriptor(x,'name'))
```
Object.getOwnPropertyDescriptor可以获取对象某个key的描述对象，打印结果如下：
```javascript
 {
    value: "vue",
    writable: true, 
    enumerable: true, 
    configurable: true
}
```
2. `configurable`
```javascript
Object.defineProperty(x, 'name', {
      configurable: false
})
```
执行成功之后，如果你再想删除该属性，比如delete x['name']，你会发现返回为false，即无法删除了。     

3. `enumerable`
```javascript
let x = {}
x[1] = 2
x[2] = 4
Object.defineProperty(x, 2, {
     enumerable: false
})
for(let key in x){
    console.log("key:" + key + "|value:" +  x[key])
}
```
结果如下：`key:1 | value:2`
为什么呢？ 因为我们把2设置为不可遍历了，那么我们的for循环就取不到了，当然我们还是可以用x[2]去取到2对应的值得，只是for循环中取不到而已。
这个有什么用呢？Vue源码中Observer类中有下面一行代码：
```javascript
def(value, '__ob__', this);
```
这里def是个工具函数，目的是想给value添加一个key为__ob__，值为this，但是为什么不直接 value.__ob__ = this 反而要大费周章呢？
因为程序下面要遍历value对其子内容进行递归设置，如果直接用value.__ob__这种方式，在遍历时又会取到，这显然不是本意，
所以def函数是利用Object.defineProperty给value添加的属性，同时enumerable设置为false。     

4. `get and set`更加强大
```javascript
let x = {}
Object.defineProperty(x, 1, {
      get: function(){
           console.log("getter called!")
      },
      set: function(newVal){
            console.log("setter called! newVal is:" + newVal)
      }
})
```
Vue源码正是通过这种方式实现了访问属性时收集依赖，设置属性时源码有一句dep.notify，里面便是通知视图更新的相关操作。


