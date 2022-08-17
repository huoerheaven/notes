#### Promise 相关知识点
- [Promise](https://es6.ruanyifeng.com/#docs/promise);
#### Promise 源码实现
```js
const PENDING = "pending";
const FULFILLED = "fulfilled";
const REJECTED = "rejected";
class MyPromise{
    constructor(executor){
        //捕获执行器内的错误
        try{
            executor(this.resolve,this.reject);
        }catch(e){
            this.reject(e);
        }
    }
    status=PENDING;
    value=undefined;
    reason=undefined;
    //成功回调
    successCallback=[];
    //失败回调
    failCallback=[];
    //使用箭头函数的目的就是 函数中的this指向promise实例，不然会指向window
    resolve=value=>{
        // 如果状态不是等待 阻止程序向下执行
        if(this.status!==PENDING) return;
        // 将状态更改为成功
        this.status = FULFILLED;
        // 保存成功之后的值
        this.value=value;
        // 判断成功回调是否存在 如果存在 调用
        while(this.successCallback.length) this.successCallback.shift()();
        
    }
    reject=reason=>{
        //如果状态不是等待 阻止程序向下执行
        if(this.status!==PENDING) return;
        // 将状态更改为失败
        this.status = REJECTED;
        // 保存失败之后的原因
        this.reason=reason;
        // 判断失败回调是否存在 如果存在 调用
        while(this.failCallback.length) this.failCallback.shift()();
    }
    then(successCallback,failCallback){
        //处理then方法不传值的情况
        successCallback = successCallback ?successCallback:value=>value;
        failCallback=failCallback?failCallback:reason=>{throw reason};
        const promise2 = new MyPromise((resolve,reject)=>{
            // 判断状态
            if(this.status===FULFILLED){
                setTimeout(()=>{
                    //捕获then 方法中的错误
                    try{
                        let val = successCallback(this.value);
                        // 判断val的值是普通值还是promise对象
                        // 如果是普通值 直接调用resolve
                        // 如果是promise对象 查看promise对象返回的结果
                        // 再根据promise返回的结果 决定调用resolve还是reject
                        resolvePromise(promise2,val,resolve,reject);
                    }catch(e){
                        reject(e);  
                    }
                },0)
            }else if(this.status===REJECTED){
                setTimeout(()=>{
                    //捕获then 方法中的错误
                    try{
                        let val = failCallback(this.reason);
                        // 判断val的值是普通值还是promise对象
                        // 如果是普通值 直接调用resolve
                        // 如果是promise对象 查看promise对象返回的结果
                        // 再根据promise返回的结果 决定调用resolve还是reject
                        resolvePromise(promise2,val,resolve,reject);
                    }catch(e){
                        reject(e);  
                    }
                },0)
            }else{
                //pending 处理异步情况 将成功回调和失败回调存储起来
                this.successCallback.push(()=>{
                    setTimeout(()=>{
                        //捕获then 方法中的错误
                        try{
                            let val = successCallback(this.value);
                            // 判断val的值是普通值还是promise对象
                            // 如果是普通值 直接调用resolve
                            // 如果是promise对象 查看promise对象返回的结果
                            // 再根据promise返回的结果 决定调用resolve还是reject
                            resolvePromise(promise2,val,resolve,reject);
                        }catch(e){
                            reject(e);  
                        }
                    },0)
                });
                this.failCallback.push(()=>{
                    setTimeout(()=>{
                        //捕获then 方法中的错误
                        try{
                            let val = failCallback(this.reason);
                            // 判断val的值是普通值还是promise对象
                            // 如果是普通值 直接调用resolve
                            // 如果是promise对象 查看promise对象返回的结果
                            // 再根据promise返回的结果 决定调用resolve还是reject
                            resolvePromise(promise2,val,resolve,reject);
                        }catch(e){
                            reject(e);  
                        }
                    },0)
                });
            };
        });
        return promise2;
    }
    catch(failCallback){
        return this.then(undefined,failCallback);
    }
    finally(callback){
        let p = this.constructor;
        return this.then(value=>{
            return p.resolve(callback()).then(()=> value);
        },reason=>{
            return p.resolve(callback()).then(()=>{throw reason});
        })
    }
    static all(array){
        return new MyPromise((resolve,reject)=>{
            const result=[];
            let count=0;
            function addData(key,value){
                result[key]=value;
                count++;
                if(count===array.length){
                    resolve(result);
                }
            }
            for(let i=0;i<array.length;i++){
                let currV = array[i];
                if(currV instanceof MyPromise){
                    //promise
                    currV.then(value=>addData(i,value),reason=>reject(reason))
                }else{
                    //普通值
                    addData(i,currV);
                }
            }
        });
    }
    static resolve(value){
        if(value instanceof MyPromise) return value;
        return new MyPromise(resolve=>resolve(value));
    }
}

function resolvePromise(promise2,val,resolve,reject){
    if(promise2===val){
        return reject(new TypeError("Chaining cycle detected for promise #<Promise>"))
    }
    if(val instanceof MyPromise){
        //promise
        // val.then(value=>resolve(value),reason=>reject(reason));
        val.then(resolve,reject)
    }else{
        //普通值
        resolve(val);
    }
}   
```
- 测试代码
```js
/*
            1. Promise 就是一个类 在执行这个类的时候 需要传递一个执行器进去 执行器会立即执行
            2. Promise 中有三种状态 分别为成功：fulfilled 失败：rejected 等待：pending
               pending->fulfilled
               pending->rejected
               一旦状态确定就不可更改
            3. resolve 和 rejected 函数是用来更改状态的
               resolve:fulfilled
               reject:rejected
            4. then 方法内部做的事情就是判断状态 如果状态是成功，调用成功的回调函数，如果是失败，调用失败的回调函数 then方法是被定义在原型对象上的
            5. then成功回调有一个参数，表示成功之后的值 then失败回调有一个参数，表示失败后的原因
            7. then 方法是可以被链式调用的，后面then方法的回调函数拿到的值是上一个then方法的回调函数的返回值
        */

        const promise = new MyPromise((resolve,reject)=>{
            // throw new Error("executor error")
            // setTimeout(()=>{
            //     resolve("成功");
            // },3000)
            reject("失败");

           
        });

        promise.then().then().then(value=>console.log(value),reason=>console.log(reason))
        // function other(){
        //     return new MyPromise((resolve,reject)=>{
        //         resolve("other");
        //     })
        // }

        // promise.then(value=>{
        //     console.log(value);
        //     throw new Error("then error")
        // },reason=>{
        //     console.log(reason);//executor error 
        //     return 100;
        // }).then(value=>{
        //     console.log(value)
        // },reason=>{
        //     console.log(reason)
        // })

        // let p1=promise.then(value=>{
        //     console.log(value);
        //     return p1;
        // },reason=>{
        //     console.log(reason)
        // });
        // p1.then((value)=>{
        //     console.log(value)
        // },reason=>{
        //     console.log(reason)
        // })

        // 测试 Promise.all()
        function p1(){
            return new MyPromise((resolve,reject)=>{
                setTimeout(()=>{
                    // resolve("p1")
                    reject("p1 reject")
                },2000)
            })
        }
        function p2(){
            return new MyPromise((resolve,reject)=>{
                resolve("p2 test")
            });
        }
        
        //按顺序返回结果
        MyPromise.all(["a","b",p1(),p2(),"c"]).then(res=>{
            console.log(res)
        });//["a","b","p1","p2","c"]

        //resolve
        MyPromise.resolve(100).then(value=>console.log(value));
        MyPromise.resolve(p1()).then(value=>console.log(value));

        //catch
        promise.catch(reason=>console.log(reason,11));

        //finally
        p2().finally(()=>{
            console.log("finally");
            return p1();
        }).then(value=>{
            console.log(value)
        },reason=>{
            console.log(reason);
        })
```
