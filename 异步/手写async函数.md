```js
//async函数的实现原理，就是将Generator函数和自动执行器，包装在一个函数里。


        // async function fn(args){
        //     ...
        // }
        //等同于
        // function fn(args){
        //     return spawn(function* (){});
        // }
        //所有的async函数都可以写成上面的第二种形式，其中的spawn函数就是自动执行器。
        function* genF(){
            yield 1;
            yield 2;
            return 3;
        }
        function spawn(genF){
            return new Promise((resolve,reject)=>{
                const gen = genF();
                function stepF(nextF){
                    let next;
                    try{
                        next = nextF();
                    }catch(e){
                        return reject(e)
                    }
                    if(next.done) return resolve(next.value);
                    Promise.resolve(next.value).then(v=>{
                        stepF(function(){return gen.next(v)});
                    }).catch(e=>{
                        stepF(function(){return gen.throw(e)});
                    });
                }
                stepF(function(){return gen.next(undefined)});
            })
        }

        spawn(genF).then(res=>{console.log(res)}).catch(e=>{
            throw new Error(e)
        });
```
