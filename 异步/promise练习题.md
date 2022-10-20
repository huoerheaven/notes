```js
//第一题
// Promise.resolve().then(()=>{
//     console.log(1);//1
// }).catch(()=>{
//     console.log(2);//不会执行，因为上一个then返回的是resolved状态的promise，所以不会执行catch
// }).then(()=>{
//     console.log(3);//3
// });//resolved


//第二题
// Promise.resolve().then(()=>{//rejected
//     console.log(1);//1
//     throw new Error("error1")
// }).catch(()=>{//resolved
//     console.log(2);//2
// }).then(()=>{//resolved
//     console.log(3);//3
// });


//第三题
Promise.resolve().then(()=>{//rejected
    console.log(1);//1
    throw new Error("error1")
}).catch(()=>{//resolved
    console.log(2);//2
}).catch(()=>{
    console.log(3);//不会执行 因为上一个catch返回的是resolved状态的promise
});

//rejected状态会触发catch回调
//resolved状态会触发then回调

[！！！注意！！！]
then 和 catch 改变状态
1. then 正常返回resolved状态的promise，then里面有报错则返回rejected状态的promise
2. catch 正常返回resolved状态的promise,catch里面有报错则返回rejected状态的promise(重要，因为比较绕)
```
