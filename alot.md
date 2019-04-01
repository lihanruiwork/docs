  腾讯 https://juejin.im/post/5c19c1b6e51d451d1e06c163
  2018大厂高级前端面试题汇总 https://juejin.im/post/5bc92e9ce51d450e8e777136
  1、window的onload事件和domcontentloaded谁先谁后
  https://juejin.im/post/5b2a508ae51d4558de5bd5d1
  DOMContentLoaded ：文档解析完毕，页面重新渲染。当页面引用的所有 js 同步代码执行完毕
  load：html 文档中的图片资源，js 代码中有异步加载的 css、js 都加载完毕
  页面中引用的js 代码如果有异步加载的 js、css、图片，是会影响 load 事件触发的。
  video、audio、flash 不会影响 load 事件触发。
  2、Generator  async  promise
  https://juejin.im/post/5ab51336f265da239d493ff4
  function关键字与函数名之间有个*
  内部使用yield，定义不同的内部状态
  Hw.next() next方法返回一个对象{value, done}
  惰性求值
  执行generator函数返回一个遍历器对象
  通过next方法的参数，可以在generator函数开始运行之后，继续向函数体内部注入值，从而调整函数行为
  next方法的参数表示上一个yield的返回值
  第一个next方法用来启动遍历器
  async是generator的语法糖
  async替换*
  await替换yield
  async函数的返回值是Promise
  所以可以用then方法添加回调
  将现有对象转为Promise对象，Promise.resolve()
  Promise对象三种状态：pending、fulfilled、reject
  new Promise(function(resolve,reject){
  if(success){
    resolve()
  }else{
    reject()
  }
  })
  Promise内部的错误不会影响到外部的代码，Promise会吃掉错误
  但是node有一个unhandledRejection事件，专门监听未捕获的reject错误
  process.on(“unhandledRejection”, function (err,p){
  })
  .catch后面返回的还是promise对象，还可以接着用then
  finally 执行完then或catch后都会执行
  Promise.all([p1,p2,p3])   都变成fulfilled，才变成fullfilled
  Promise.race([p1,p2,p3]) 有一个率先改变状态，就变
  Promise.race([
  P1,
  new Promise(function(resolve,reject){
    setTimeout(function(){reject(new Error(‘request timeout’))},40000)
  })	
  ])
  3、map set
  Set 类似数组，值唯一
  5和“5”是不同的值 内部判断用===
  add  delete has clear size
  Array.from(new Set(array))  转数组
  Array.from(new Set(array)) 去除数组重复成员
  let unique = [...new Set(arr)];
  Keys values entries forEach
  并集 new Set([…a, …b])
  交集 new Set([…a].filter(x => b.has(x)))
  差集 new Set([…a].filter(x => !b.has(x)))
  Map

   3、this
  https://juejin.im/post/5c049e6de51d45471745eb98
  4、for in && for of
  https://www.jianshu.com/p/c43f418d6bf0
  5、属性类型：
  Object.defineProperty(person, “name”, {
    configurable:false,
    value:deqw
  });
  数据属性
  Configurable：configurable一旦设为false，调用defineProperty只能修改writable
  Enumerable：能否for in 循环返回属性
  Writable
  Value
  访问器属性
  Configurable Enumerable get set
  设置一个属性的值会导致其他属性发生变化
  6、前端性能优化
  https://csspod.com/frontend-performance-best-practices/
  重要指标：页面加载时间===》用户体验&搜索引擎排名
  减少http请求数 图片&样式&脚本
  合并js、css文件
  Css sprite 合并背景图片 backgroud-position
  Svg sprite
  并行下载，将请求划分到不同的域名
  DNS缓存
  避免重定向 301 302   根据响应头中的location再次发送请求
  缓存响应结果
  延迟加载
  非首屏使用的数据样式脚本图片
  延迟渲染：将首屏以外的html放在不渲染的元素(隐藏)中，减少初始渲染的dom元素数量
  7、http缓存
  https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching?hl=zh-cn
  客户端与服务器之间多次通信开销巨大，所以缓存并重复利用之前获取的资源是性能优化的一种
  最佳无需与服务器通信
  浏览器自带http缓存
  先检查本地缓存，如果已过期，将验证令牌放在If-None-Match，发送至服务器，如果资源未发生变化，则返回304（Not Modified），此时直接用原资源，这些浏览器都会自动做
  Etag：验证令牌
  Cache-Control：
  no-cache 必须和服务器确认响应是否有变化
  no-store 禁止缓存，必须向服务器请求，下载完整资源
  max-age 允许缓存被重用的最长事件
  private 不可以用中间缓存
  在资源内容发生变化时更改其网址，强制用户下载新响应 
  8、http
  11、webpack打包性能优化
  12、websocket实现原理
  长连接
  13、css3性能优化
  14、url打开过程
  主要是http协议和DOM渲染机制
  15、AST 抽象语法树
  数组  ====>   树形
  CST 具体语法树  
  16、koa
  中间件原理：https://juejin.im/post/5a5f5a126fb9a01cb0495b4c
  17、盒模型
  Box-sizing
  content-box: width只包括内容
  border-box：包括内容、内边距、边框
  18、写一个处理跨域的jsonp
  19、实现parseInt一样功能的函数（需要考虑的情况满多的，当时写得不好）
  20、实现快排
  21、有n个棋子，A/B互相取棋子，每个人最多拿4个，实现一个算法保证你赢
  22、BFC   https://juejin.im/post/59e7190bf265da4307025d91
  内外元素的定位不会相互影响
  23、CDN
  24、容器
  25、请求方法
  GET 查看一个资源 
  POST 更新一个资源
  HEAD 
  DELETE 删除一个资源
  PUT  新建一个资源
  CONNECT
  Cookie 用户是不是第一次访问网站
  1、服务器像客户端发cookie
  2、浏览器保存Cookie
  3、之后每次浏览器都将Cookie发向服务器
  req.headers.cookie
  响应的Cookie Set-Cookie
  Expires max-age 如果不设置，关闭浏览器就会丢失掉Cookie
  httponly 不允许通过document.cookie更改Cookie值
  secure https中才有效
  session：数据只能保留在服务器端，数据安全性得到保障
  基于cookie实现用户和数据的映射，cookie存口令，配合session
  Symbol：新的原始数据类型
  Undefined null string number object boolean symbol
  CommonJs AMD 只能在运行时确定模块的依赖关系，无法再编译时做静态优化
  es6 import 编译时加载
  es6模块自动采用严格模式
  变量必须声明后再使用
  函数参数不能重名
  不能对只读属性赋值
  不能删除不可删除的属性
  eval和arguments不能被重新赋值
  不能使用arguments.callee&caller
  不能使用前缀0表示8进制

  export {
    v1 as streamV1  //重命名
  }
  只执行一次的函数
  export function once (fn: Function): Function {
    let called = false
    return function () {
      if (!called) {
        called = true
        fn.apply(this, arguments)
      }
    }
  }
  创建一个缓存函数
  export function cached<F: Function> (fn: F): F {
    const cache = Object.create(null)
    return (function cachedFn (str: string) {
      const hit = cache[str]
      return hit || (cache[str] = fn(str))
    }: any)
  }

