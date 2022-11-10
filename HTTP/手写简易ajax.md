```js
// function Ajax(url,successFn){
//     const xhr = new XMLHttpRequest();
//     xhr.open("GET",url,true);//第三个参数，true代表异步 false代表同步
//     xhr.onreadystatechange=function(){
//         if(xhr.readyState===4){
//             if(xhr.status===200){
//                 successFn(xhr.responseText);
//             }
//         }
//     }
//     xhr.send(null);
// }

// Ajax("data/data.json",data=>{
//     console.log(data,"data")
// })

function Ajax(url){
    return new Promise((resolve,reject)=>{
        const xhr = new XMLHttpRequest();
        xhr.open("GET",url,true);//第三个参数，true代表异步 false代表同步
        xhr.onreadystatechange=function(){
            if(xhr.readyState===4){
                if(xhr.status===200){
                    resolve(JSON.parse(xhr.responseText));
                }else if(xhr.status===404){
                    reject(new Error("404 not found"))
                }
            }
        }
        xhr.send(null);
    })
};
Ajax("/data/data.json").then().catch();

```
