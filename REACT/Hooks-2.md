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

