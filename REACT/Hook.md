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
- [react hook api form zhihu](https://zhuanlan.zhihu.com/p/424967794)



