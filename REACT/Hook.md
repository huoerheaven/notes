#### 初始化一个项目
- npx create-react-app ts-with-react --template typescript
- npx 避免安装全局模块、调用项目内部安装的模块

#### React Hook
##### 1. 什么是 React Hook
- React 16.8带来的全新特性，即将代替class组件的写法
- React 组件一直是函数，使用Hook完全拥抱函数

##### 2. State Hook
- useState(initialState)返回一个数组，其中第一项是状态值，第二项是一个更新状态的函数。

##### 3. Effect Hook
- useEffect
- useEffect Hook 看做 componentDidMount，componentDidUpdate 和 componentWillUnmount 这三个函数的组合。
- [详见](https://blog.csdn.net/glorydx/article/details/114107703)

##### 4. 自定义Hook
- 自定义Hook以use开头（约定）
- 两个不同的组件中使用相同的hook 不会共享useState,每次使用自定义hook时，useState是完全隔离的
- HOC - (Higher order component)
- 高阶组件 就是一个函数，接受一个组件作为参数，返回一个新的组件
- [random pictures](https://dog.ceo/api/breeds/image/random)

##### 5. useRef
- 组件的每次渲染 props和state都是保持独立的
- [useRef](https://zhuanlan.zhihu.com/p/115230135)

##### 6. useContext
- [useContext使用方法](https://juejin.cn/post/6896353934525497357)

##### 7. hook规则
- 只在最顶层使用Hook
- 只在React函数中调用Hook
- [Hook API 索引 from 官方](https://zh-hans.reactjs.org/docs/hooks-reference.html)
- [usehooks](https://usehooks.com/)
- [react hook api form zhihu](https://zhuanlan.zhihu.com/p/424967794)

##### 8. react v18 新特性
- **新的渲染API:createRoot**
- **Automatic Batching (批处理)**
- - 批处理可以帮忙减少re-render的次数
- **具备并发性**
- - 并发性是指 具备处理多个任务的能力，但不是在同时处理多个，而是有可能交替的进行处理，按照任何的优先级，每次处理一个任务 
- - 比如 我们在打电话的过程中，又进来一个电话，我们可以接听新的电话，让原来的通话继续保持，等新的通话结束后继续原来的通话
- **Transition API** 
- - useTransition 
```js
const [isPending,startPosition] = useTransition();
//isPending 用来判断startPosition中的非紧急任务是否是pending状态 ，是的话可以加loading样式
```
- - startTransition 接收一个回调函数 将非紧急更新任务放入回调中
- - **紧急更新**： 反映在用户交互上，需要实时性，比如点击、输入、拖拽等
- - **非紧急更新**： 也就是transition,是界面从一个状态过渡到另一个状态
- Transition API 应用场景
- - Slow Rendering: 当数据量很大的界面渲染，自然要花费更长的时间
- - Slow Network: 异步请求，由于需要从外界请求数据，自然有一定的延迟
```js  
import React, { useState , useTransition} from 'react';

const App:React.FC=()=>{
    //useTransition
    const [isPending,startTransition] = useTransition();
    const [input,setInput] = useState("");
    const [searchData,setSearchData] = useState<number[]>([]);

    const updateInput = (e:React.ChangeEvent<HTMLInputElement>)=>{
        setInput(e.target.value);
        startTransition(()=>{
        const arr = Array.from({length:10000},(_,i)=>i+new Date().getTime());
        setSearchData(arr);
        })
    }

    return (
        <>
        <input type="text" value={input} name="" id="" onChange={updateInput} />
        {isPending?<p>⏳</p>:""}
        {
          searchData.map(s=>
            <option key={s}>{s}</option>
        )}
        </>
    )
}

```

- **suspense**
- - react v16就已经推出了，动态加载组件
- - [详见](https://segmentfault.com/a/1190000022185283)

- **实践Suspense**
> wrapPromise的工作机制
- - 接受Promise作为参数
- - 当Promise resolved,返回resolved value
- - 当Promise rejected,throw对应的rejected value
- - 当Promise pending,throw对应的Promise对象
- - 暴露一个对应的read方法，来读取Promise的状态
```js
//fetchData.tsx
import axios from "axios";

function wrapPromise(promise:Promise<any>){
    //设定两个变量，一个指当前状态，一个指最后的结果
    let status = "pending";
    let result:any;
    let suspender = promise.then(s=>{
        status = "success";
        result=s;
    }).catch(e=>{
        status = "error";
        result=e;
    });
    return {
        read(){
            if(status==="pending"){
                throw suspender;
            }else if(status==="success"){
                return result;
            }else if(status==="error"){
                throw result;
            }
        }
    }
}

export default function fetchData(url:string){
    const promise = axios.get(url).then(res=>res.data);
    return wrapPromise(promise);
}
```
```js
//DogShow.tsx DogShow组件
import React from "react";
import fetchData from "../request/fetchData";

const data = fetchData("https://dog.ceo/api/breeds/image/random");

const DogShow: React.FC = ()=>{
    const dogData = data.read();
    return (
        <img src={dogData.message} style={{width:"60px",height:"60px"}}/>
    )
}

export default DogShow;
```
```js
//App.tsx
import React, { Suspense} from 'react';
import DogShow from "./components/DogShow";
const App:React.FC=()=>{
    return (
        <>
            <Suspense fallback={<h1>loading dog image...</h1>}>
            <DogShow />
            </Suspense>
        </>
    )
}
```

