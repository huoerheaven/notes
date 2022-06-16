#### Redux 设计和使用的三项原则
- store是唯一的
- 只有store能够改变自己的内容
- - reducer可以接受state 但是绝不能修改state
- reducer必须是纯函数
- - 纯函数指的是，给定固定的输入，就一定会有固定的输出，而且不会有任何副作用
- - 一旦函数里有setTimeOut,ajax请求，new Date()等时，它就不再是一个纯函数，所以reducer中不能有异步操作和有和时间相关的操作

#### Redux核心API
- createStore
- store.dispatch
- store.getState
- store.subscribe (订阅store的改变)

#### 无状态组件
- 当一个组件只有render函数时。我们可以用无状态组件来替换这个组件；
- 性能高
- 就是一个函数组件

#### redux-thunk redux的中间件
- 对store的dispatch方法做一个升级
- 该中间件可以将一个函数传递给dispatch 并且执行dispatch时会自动调用传入的函数 store.dispatch(action) action就是一个函数
- 可以借助redux-thunk可以将异步请求放到actionCreators中去管理
- 有利于自动化测试
- 中间件是指action和store的中间 对dispatch方法的升级

#### redux-saga
- generator函数

#### react-redux的使用
- Provider组件 连接store provider组件内部的组件就都有能力获取到store里的内容了
```js
//index.js
 const App=(
     <Provider store={store}>
        <TodoList />
     </Provider>
 )
```
- connect 让组件和store做连接
```js
import { connect } from "react-redux";

class TodoList extends Component{
    render(){
        return (
            <input 
                value={this.props.inputValue}
                onChange={this.props.changeInputValue}
            />
        )
    }
}

//mapStateToProps 为TodoList和store的映射关系 
//store中的state映射到组件的props
const mapStateToProps = (state)=>{
    return {
        inputValue:state.inputValue
    }
}

//将store.dispatch映射到props
const mapDispatchToProps = (dispatch)=>{
    return {
        changeInputValue(e){
            const action={}
            dispatch(action);
        }
    }
}

export default connect(mapStateToProps,mapDispatchToProps)(TodoList)

```
- 此时的TodoList是UI组件（也是无状态组件），connect方法将UI组件和逻辑业务（数据方法）关联起来，就形成了容器组件，export default导出的就是容器组件。
