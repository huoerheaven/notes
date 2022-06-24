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
