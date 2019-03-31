# redux
## 基本概念
函数式编程
* 不能修改参数
* 不能调用系统IO API
* 不能调用Math.random() Date.now()
```javascript
import { createStore } from 'redux'
let { getState, dispatch, subscribe} = createStore(reducer, initialState)
store.subscribe(render)
const Counter = ({val, increment, decrement})=>{
  <div>
    <h1>{val}</h1>
    <button onClick={increment}>+</button>
    <button onClick={decrement}>-</button>
  </div>
}
const render = ()=>{
  ReactDOM.render(
    <Counter value={store.getState()} 
             increment={() => store.dispatch({type: 'increment'})}
             decrement={() => store.dispatch({type: 'decrement'})}
    ></Counter>,
    document.getElementById('root')
  )
}
reducer: (state, action)=>{
    switch(action.type){
      case 'increment': return state + 1;
      case 'decrement': return state - 1;
      default: return state;
    }
}
```
视图与状态一一对应
* dispatch：用户发出action
* reducer：store自动调用reducer, 计算state, 并返回
* subscribe：state改变，store调用监听函数
* getState：listener通过store.getState()获取当前状态，触发重新渲染
## 中间件
* applyMiddlewares
## 异步操作
* dispatch 正在抓取数据
* 异步操作.then(dispatch 操作结束)
## react-redux
* UI组件
    * 只负责UI呈现
    * 没有state, 只有props
    * 纯粹由props决定他的值
* 容器组件
    * 只管理数据和业务逻辑
* 外部是容器组件，内部是UI组件
* 前者负责与外部通信，后者负责渲染
* connect：容器组件由react-redux自动生成
```javascript
import { connect } from 'react-redux'
const = connect(
  mapStateToProps，
  mapDispatchToProps
)(UI组件)
```
