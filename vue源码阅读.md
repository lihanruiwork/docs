# vue源码阅读
https://juejin.im/book/5a36661851882538e2259c0f/section/5a3bb17af265da4307037186
* 依赖收集
    * observe(data)
    New Dep()
    内部 遍历data，执行defineReactive(key,val)
    内部 Object.defineProperty(
    Get
    Set
    Get: 收集依赖  dep.addSub
    Dep 内部一个数组 subs[]
    this.subs.push(sub)
    sub=Dep.target=new Watcher()
    Dep.target = this
    Set 通知watcher
    newval ===val
    dep.notify
    遍历subs，挨个执行update
    Class Watcher{
      constructor() {
        Dep.target = this
      }
      update(){}
    }
    每个data属性都会有自己的new Dep(),闭包
    当前的watcher对象
    render function 返回一个 虚拟dom
    对真实DOM的抽象
    跨平台能力
    tag data(props attrs) children text elm
* Compile编译 
    * Parse（AST）  
    * Optimize：优化，标记静态节点
    * Generate：生成render function 字符串
* 最终render function
* patch 
    * 根据platform不同，执行当前平台的API，对外提供一致的接口，供虚拟dom调用
* nextTick
    * 在当前调用栈执行完毕后才去执行这个事件
    * 目前浏览器平台没有nextTick方法
    * 用Promise、setTimeout、setImmediate
    * 定义一个数组存储，在下一个tick处理这些回调
    
