#### 1. 判断是不是Object
- typeof null == "object"
#### 2. 位移运算符
```js
<<  //左移
>>  //有符号右移
>>> //无符号右移
```
- 左移运算符
- - 按二进制形式把所有的数字向左移动对应的位数，高位移出(舍弃)，低位的空位补零。
```js
// 3<<2 则是将数字3左移2位
// 首先把3转换为二进制数字0000 0000 0000 0000 0000 0000 0000 0011，然后把该数字高位(左侧)的两个零移出，其他的数字都朝左平移2位，最后在低位(右侧)的两个空位补零。则得到的最终结果是0000 0000 0000 0000 0000 0000 0000 1100，则转换为十进制是12。
// 总结：在数字没有溢出的前提下，对于正数和负数，左移一位都相当于乘以2的1次方，左移n位就相当于乘以2的n次方。
```
- 右移运算符
- - 按二进制形式把所有的数字向右移动对应位移位数，低位移出(舍弃)，高位的空位补符号位，即正数补零，负数补1。
```js
// 12 >> 2
0000 0000 0000 0000 0000 0000 0000 1100 -> 0000 0000 0000 0000 0000 0000 0000 0011  -> 3
// 结论：右移一位相当于除2，右移n位相当于除以2的n次方。
```
- 无符号右移运算符
- - 按二进制形式把所有的数字向右移动对应位数，低位移出(舍弃)，高位的空位补零。对于正数来说和带符号右移相同，对于负数来说不同。
#### 3. parseInt()
```js
["1","2","3"].map((val,index)=>parseInt(val,index));
parseInt("1",0); //1
parseInt("2",1); //NaN
parseInt("3",2); ////NaN
```
- 参数一：必需，要被解析的字符串；
- 参数二：可选，表示要解析的数字的基数。该值介于 2 ~ 36 之间。（几进制）

#### 4. 数据类型8种判断方式
###### 1. typeof
- 主要用途：操作数的类型，只能识别基础数据类型和引用类型
- 特别注意：null,NaN,document.all
```js
typeof null === "object"
typeof NaN === "number"
typeof document.all === "undefined"
```
- 注意事项：已经不是绝对安全（暂时性死区）

###### 2. constructor
- 原理：constructor指向创建实例对象的构造函数
- 注意事项1：null和undefined没有构造函数
- 注意事项2：constructor可以被改写

###### 3. instanceof
- 原理：就是在原型链上查找，查到即是其实例
- 注意事项1：右操作数必须是函数或者class
- 注意事项2：多全局对象，多window之间会出现此Array非彼Array的情况，比如iframe

###### 4. isPrototypeOf()
- 原理：是否出现在实例对象的原型链上
- 注意事项：**能正常返回值**的情况，基本等同于instanceof

###### 5. Object.prototype.toString
```js
Object.prototype.toString(); //'[object Object]'
Object.prototype.toString.call(Boolean.prototype); //'[object Boolean]'
```

###### 6.鸭子类型检测
- 原理：检查自身属性的类型或者执行结果的类型
- 例子：kindOf与p-is-promise
- 总结：候选方案

###### 7. Symbol.toStringTag
- 原理：Object.prototype.toString会读取该值
- 适用场景：需自定义类型
- 注意事项：兼容性
```js
class MyArray{
 get [Symbol.toStringTag](){
  return "MyArray"
 }
}
let a = new MyArray();
console.log(Object.prototype.toString.call(a));//[object MyArray]
```

###### 8.等比较
- 原理：与某个固定值进行比较
- 使用场景：undefined,window,document,null
```js
const isUndefined = function(obj){
 return obj === void 0;//void 0 === undefined
}
```

###### 9.总结

|方法                |基础数据类型 |引用类型 |注意事项 |
|:-:                |:-:          |:-:     |:-:|
|typeof             |√            |×       |NaN,null,document.all |
|constructor        |√ 部分       |√        |可以被改写 |
|instanceof         |×            |√       |多窗口，右边构造函数或者class |
|isPrototypeOf      |×            |√       |null和undefined无此方法 |
|toString           |√            |√       |小心内置原型 |
|鸭子类型            |---          |√       |不得已或者兼容 |
|Symbol.toStringTag |×            |√       |识别自定义对象 |
|等比较              |√            |√       |特殊对象 |


#### 5. NaN和Number.NaN
###### 特点
- 特点1：typeof NaN === "number"  typeof Number.NaN === "number"
- 特点2：NaN != NaN  Number.NaN != Number.NaN
- 特点3：不能被删除
###### isNaN
- 当我们向isNaN传递一个参数，它的本意是通过Number()方法尝试转换参数的类型为Number，如果转换成功返回false，否则转返回true，它只是判断这个参数能否转成数字而已，并不是判断是否严格等于NaN
###### Number.isNaN
- 判断一个值是否是数字，并且值等于NaN(是否严格等于NaN)
###### Object.is()
- 判断两个值是否为同一个值
```js
NaN == NaN  //false
Object.is(NaN,NaN)  //true

+0 == -0  //true
Object.is(+0,-0)  //false
```
###### other
```js
let arr=[NaN];
arr.indexOf(NaN);//-1
arr.includes(Nan);//true
```
