* constructor
  1. 初始化state: 不要用setState,直接为this.state赋值
  2. 方法绑定：事件处理函数绑定实例
  3. super(props)
  不要把props的值赋给state,没有必要
* render
  * 对DOM进行操作
* componentDidMount:
  1. 组件已挂载（插入DOM树）
  2. 此时调用setState，会触发额外渲染
     但此渲染发生在浏览器更新屏幕之前，用户看不到
     但是也要谨慎使用，会有性能问题
* key：特殊的react属性
  1. key变化，react会创建而不是更新一个既有的组件
  2. 一般用来渲染动态列表，但也是处理重置state最好的方法
  3. 优点：可以省略子组件diff
* componentDidMount: 添加订阅
  componentWillUnmount: 取消订阅
* componentDidUpdate:
  ```javascript
  componentDidUpdate(prevProps) {
    this.props.userID !== prevProps.userID
  }
  ```
   * 可以调用setState，但必须在if中，否则会死循环
* shouldComponentUpdate(nextProps, nextState)
   * 可以用PureComponent代替
* PureComponent
   * 对state和props进行浅层比较
* 可复用性行为
* 代码分割
  * 打包时创建多个包，并在运行时动态加载
  * 可以避免加载用户永远不需要的代码
  * 动态import
    ```javascript
      import('./math').then(math => {
          math.add()..
      })
    ```
 * Hook
    * useEffect Hook
      * 等同于componentDidMount componentDidUpdate componentWillUnount的组合   
      * 对抗class组件中的state类异步
      * 所有变量的值都会保留在该副作用执行的时刻
 * class组件 & 函数组件
    * 函数组件
      * 无状态
      * 没有this（组件实例）
    * Hook在class组件不起作用
 * setState()可能会异步更新，类异步
  ```
    this.setState((state, props) => ({
      counter: state.a + props.b
    }))
  ```
  * 数据向下流动 父传子
 
