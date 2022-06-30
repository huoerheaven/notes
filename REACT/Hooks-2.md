##### 1. 函数组件的特点
- 没有组件实例
- 没有生命周期
- 没有state和setState,只能接收props

##### 2. class 组件的问题
- 大型组件很难拆分和重构，很难测试（即class不易拆分）
- 相同业务逻辑，分散到各个方法中，逻辑混乱
- 复用逻辑变得复杂，如Mixins、HOC、Render Prop

##### 3. React 组件更易用函数表达
- React 提倡函数式编程，view=fn(props)
- 函数更灵活，更易拆分，更易测试
- 但函数组件太简单，需要增强能力------Hooks

##### 4. Hooks命名规范
- 规定所有的Hooks都使用 use 开头，如 useState
- 自定义 Hook 也要用 use 开头
- 非 Hooks 的地方，尽量不要使用 useXxx 写法

##### 5. 用 useState 实现 state 和 setState 功能
- 默认函数组件，没有state
- 函数组件是一个纯函数，执行完即销毁，无法存储 state
- 需要State Hook,即把state功能“钩”到纯函数中

##### 6. useState 使用总结
- useState(0) 传入初始值，返回数组[state,setState]
- 通过 state 获取值
- 通过 setState(1) 修改值

##### 7. 用 useEffect 模拟组件生命周期
- 默认函数组件没有生命周期
- 函数组件是一个纯函数，执行完即销毁，自己无法实现生命周期
- 使用 Effect Hook 把生命周期“钩”到纯函数中
```js
const App=()=>{
    // 不传第二个参数，模拟class组件的componentDidMount和componentDidUpdate
    useEffect(()=>{
        console.log("模拟class组件的componentDidMount和componentDidUpdate");
    });

    // 第二个参数是 []（不依赖任何 state ）
    useEffect(()=>{
        console.log("模拟class组件的componentDidMount");
    },[]);

    // 第二个参数依赖 state ）
    useEffect(()=>{
        console.log("模拟class组件的componentDidUpdate");
    },[count,name]);

    //返回一个函数  模拟class组件的componentWillUnMount
    return ()=>{
        //...
    }
}
```

##### 8. useEffect 让纯函数有了副作用
- 默认情况下，执行纯函数，出入参数，返回结果，无副作用
- 所谓副作用，就是对函数之外造成影响，如 设置全局定时任务
- 而组件需要副作用，所以需要 useEffect “钩”入纯函数中

##### 9. useEffect 中返回函数fn
- useEffect 依赖[],组件销毁执行fn,等同于class组件的componentWillUnMount；
- useEffect 无依赖或依赖[a,b],组件更新时执行fn, 不同于class组件的componentWillUnMount；
- 即，下一次执行useEffect 之前，就会执行fn,无论更新或卸载。

```js
//！！！注意！！！
//当useEffect的第二个参数什么也不传时，相当于模拟class组件的componentDidMount和componentDidUpdate
useEffect(()=>{
    console.log(`开始监听变化`);
    //[特别注意]
    //此时并不完全等同于class组件的componentWillUnMount
    //props 发生变化，即更新，也会执行结束监听
    //准确的说：返回的函数，会在下一次 effect 执行之前，被执行 
    return ()=>{
        console.log(`结束监听变化`);
    }
})
```

##### 10. useReducer
```js
import React,{ useReducer } from "react";

const initialState = {count:0};
const reducer=(state,action)=>{
    switch(action.type){
        case "increment":
            return {count:state.count+1};
        case "decrement":
            return {count:state.count-1};
        default:
            return state;
    }
}

const UseReducer = ()=>{
    const [state,dispatch] = useReducer(reducer,initialState);

    return (
        <>
            <p>{state.count}</p>
            <button onClick={()=>{dispatch({type:"increment"})}}>increment</button>
            <button onClick={()=>{dispatch({type:"decrement"})}}>decrement</button>
        </>
        
    )
}

export default UseReducer;
```

##### 11. useReducer 和 redux的区别
- useReducer 是 useState 的代替方案，用于 state 复杂变化
- useReducer 是单个组件状态管理，组件通讯还需要props
- redux 是全局的状态管理，多组件共享数据

##### 12. useMemo 使用总结
- React 父组件变化之后，默认会更新其所有子组件
- class 组件使用SCU 和 PureComponent 做优化
- Hooks 中使用useMemo ,但优化的原理是相同的

```js
import React, { useState, memo, useMemo } from "react";


//通过memo对函数组件进行封装
//类似 class PureComponent,对 props 进行浅层比较
const Child = memo(({userInfo})=>{
    console.log("Child render...");
    return (
        <>
        <p>This is Child {userInfo.name} {userInfo.age} </p>
        </>
    )
})

const UseMemo = ()=>{
    console.log("parent render...");

    const [count,setCount] = useState(0);
    const [name,setName] = useState("huoweiwei");

    //用useMemo 缓存数据，有依赖
    //第二个参数（类似于useEffect） 依赖的数据发生变化，缓存就会失效
    const userInfo  = useMemo(()=>{
        return {name,age:18}
    },[name]);

    return(
        <>
            count is {count}
            <button onClick={()=>{setCount(count+1)}}>click Count </button>
            <button onClick={()=>{setName(name+"1")}}>click Name </button>
            <Child userInfo={userInfo}></Child>
        </>
    )


}

export default UseMemo;
```

##### 13. 使用 useCallback 做性能优化
```js
import React, { useState, memo, useMemo, useCallback } from "react";


//通过memo对函数组件进行封装
//类似 class PureComponent,对 props 进行浅层比较
const Child = memo(({userInfo,onChange})=>{
    console.log("Child render...");
    return (
        <>
        <p>This is Child {userInfo.name} {userInfo.age} </p>
        <input onChange={onChange}/>
        </>
    )
})

const UseMemo = ()=>{
    console.log("parent render...");

    const [count,setCount] = useState(0);
    const [name,setName] = useState("huoweiwei");

    //用useMemo 缓存数据，有依赖
    //第二个参数（类似于useEffect） 依赖的数据发生变化，缓存就会失效
    const userInfo  = useMemo(()=>{
        return {name,age:18}
    },[name]);

    //用 useCallback 缓存函数
    const onChange = useCallback((e)=>{
        console.log(e.target.value);
    },[])

    return(
        <>
            count is {count}
            <button onClick={()=>{setCount(count+1)}}>click Count </button>
            <button onClick={()=>{setName(name+"1")}}>click Name </button>
            <Child userInfo={userInfo} onChange={onChange}></Child>
        </>
    )
}

export default UseMemo;
```

##### 14. useMemo useCallback 使用总结
- useMemo 缓存数据
- useCallback 缓存函数
- 两者都是 React Hooks 的常见优化策略

##### 15. 自定义Hook
- 封装通用的功能
- 开发和使用第三方 Hooks
- 自定义 Hook 带来了无限的扩展性，解耦代码
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

##### 16. Hooks 使用规范
- 命名规范 useXxx
- Hooks 使用规范，重要！！！
- 关于 Hooks 的调用顺序
```js
import React, { useState } from 'react';
const Teach=()=>{
    // 函数组件，纯函数，执行完即销毁
    // 所以，无论组件初始化（render）还是组件更新（re-render）都会重新执行一次这个函数，获取最新的组件
    // 这一点和 class 组件不一样 

    //render:初始化 state 的值 
    //re-render: 读取 state 的值
    const [studentName, setStudentName ] = useState("huoweiwei");

    // render: 添加 effect 函数
    // re-render: 替换 effect 函数
    useEffect(()=>{

    })
}
export default Teach;
```
- 只能用于 React 函数组件和自定义 Hook 中,其他地方不可以
- 只能用于顶层代码，不能在循环、判断中使用 Hooks
- eslint 插件 eslint-plugin-react-hooks 可以帮到你

##### 17. Hooks 做组件逻辑复用的好处
- 完全符合 Hooks 原有规则，没有其他要求，易理解记忆
- 变量作用域明确
- 不会产生组件嵌套

##### 18. Hooks 使用中的几个注意事项！！！
- useState trap
```js 
import React,{ useState } from "react";

const Child = ({userInfo})=>{
    //Trap！！！
    //render: 初始化 state
    //re-render: 只恢复初始化的 state 值，不会再重新设置新的值
    //re-render: 只能用 setName 修改
    const [name,setName] = useState(userInfo.name);

    return (
        <>
            // props传过来的name值会改变
            <p>Child, props name: {userInfo.name}</p>
            //state中的name值不会改变，name值只能通过setName改变
            <p>Child, state name: {name}</p>
        </>
    )
}

const UseStateTrap = ()=>{
    const [name,setName] = useState("weiwei");
    const userInfo = {name};

    return (
        <>
        
            <button onClick={()=>{setName("huohuo")}}>setName</button>
            <Child userInfo={userInfo}></Child>
        </>
    )
}

export default UseStateTrap;

```
- useEffect 内部不能修改state
```js
function UseEffectChangeState(){
    const [count,setCount] = useState(0);

    //模拟DidMount 
    useEffect(()=>{
        console.log(count);//0

        //定时任务
        const timer = setInterval(()=>{
            console.log(count);//一直是0 是因为依赖为[],re-render 不会重新执行 effect 函数
            setCount(count+1);
        })

        //清除定时任务
        return ()=>clearTimeout(timer);
    },[]);

    //依赖为 [] 时，re-render 不会重新执行 effect 函数
    //没有依赖时，re-render 会重新执行 effect 函数
}
```
- - 解决方案：可以定义一个变量myCount---不推荐
- - 推荐！！！ 用useRef. const countRef = useRef(0);使用countRef.current的值
```js
function UseEffectChangeState(){
    // const [count,setCount] = useState(0);
    const countRef = useRef(0);

    //模拟DidMount 
    useEffect(()=>{

        //定时任务
        const timer = setInterval(()=>{
            console.log(countRef.current);//会+1
            setCount(countRef.current+1);
        })

        //清除定时任务
        return ()=>clearTimeout(timer);
    },[]);

    //依赖为 [] 时，re-render 不会重新执行 effect 函数
    //没有依赖时，re-render 会重新执行 effect 函数
}
```

- useEffect 可能出现死循环
- - useEffect 第二个参数里如果有{} [] 等引用类型，就会出现死循环 因为Object.is({},{})=false
- - 解决方案：将引用类型打散 如解构赋值 const {a,b} = config;然后将a b 放入useEffect 第二个参数
