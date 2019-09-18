* constructor
  1. 初始化state: 不要用setState,直接为this.state赋值
  2. 方法绑定：事件处理函数绑定实例
  3. super(props)
  不要把props的值赋给state,没有必要
* render
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
 
