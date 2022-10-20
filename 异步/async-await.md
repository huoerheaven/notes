```js
// await相当于Promise里的then
!(async function(){
    const p1 = Promise.resolve(100);
    const data1 = await p1;
    console.log(data1,"data1")
})();

!(async function(){
    const data2 = await 200;//相当于 await Promise.resolve(200)
    console.log(data2,"data2")
})();

// try...catch可捕获异常，代替了Promise的catch
!(async function(){
    const p3 = Promise.reject("err");
    try{
        const err = await p3;
        console.log(err,'err1')
    }catch(ex){
        console.log(ex,'err2-catch')
    }
})()

!(async function(){
    const p4 = Promise.reject("err");
    const err2 = await p4;//await相当于then 而p4是rejected状态 所以不会执行await 会报错
    console.log(err2,'err2')
})()



//async-await是语法糖，异步的本质还是回调函数（重要！！！）
async function async1(){
    console.log("async1 start");//第二个被打印出来 2
    await async2();//先执行async2函数，再执行await操作
    //await的后面，都可以看作是callback里的内容，即异步
    //event loop
    //下面三行都是异步回调的内容
    console.log("async1 end");// 5

    
    await async3();
    //下面一行是异步回调的内容
    console.log("async1 end 2");// 7
} 

async function async2(){
    console.log("async2");//第三个被打印出来 3
}

async function async3(){
    console.log("async3");//6
}

console.log("script start");//第一个被打印出来 1
async1();
console.log("script end");//第四个被打印出来 4

```
