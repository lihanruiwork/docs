# VUE-MVVM框架
* 过程
  * new Vue()
  * _init：初始化
    * 生命周期、事件、props、methods、data、computed、watch
    * defineProperty: setter、getter
      * 响应式，依赖收集
  * 编译
    * parse
      * 正则解析template，形成AST
    * optimize
      * 标记static静态节点
        *更新界面时，diff算法会跳过静态节点
        * 文本节点是静态节点
        * 子节点是非静态节点，则当前节点也为非静态
    * generate
      * 将AST转成render function字符串
      * render function字符串是VNode节点
      ```javascript
      render() {
        return isShow ? new VNode('div',{
          'statiClass': 'demo'
        }, [子节点]) : createEmptyVNode();
      }
      ```
* 响应式
  * render function被渲染时，触发getter,进行依赖收集
    * 将观察者对象存放在当前闭包中订阅者Dep的subs中
  * 修改对象值会触发setter
    * setter会通知每一个watcher，需要重新渲染视图
    * watcher 调用update来更新视图
    源码模拟
    ```javascript
    class Vue {
      constructor(options) {
        observe(options.data)
        new Watcher()
      }
    }
    function observe(value) {
      Object.keys(value).forEach(key=>{
        defineReactive(value, key, value[key])
      })
    }
    function defineReactive(obj, key, val) {
      let dep = new Dep()
      Object.defineProperty(obj, key, {
        enumerable: true,
        configurable: true,
        get: function() {
          dep.addSub(Dep.target)
          return val
        },
        set: function(newVal) {
          if(val===newVal) return ;
          dep.notify()
        }
      })
    }
    class Watcher {
      constructor() {
        Dep.target = this
      }
    }
    class Dep {
      constructor() {
        this.subs = []
      }
      addSub(sub) {
       this.subs.push(sub)
      }
      notify() {
        this.subs.forEach(sub=>{
          sub.update()
        })
      }
    }
    ```
* virtual DOM
  * render function 转成 VNode节点 
  * 对象属性描述节点
  * 对真实dom的抽象
  * 以js对象为基础，不依赖真实平台环境，拥有跨平台能力
    * 适配层，将不同平台的api封装在内
      * 根据不同platform执行对应的api（一些DOM操作）
        ```javascript
          node.parentNode.setAttr('value', text) //weex
          node.textContent = text  //web
        ```
    * 对外提供一致的接口，供VNode调用
    
* 更新视图
  * 数据变化，执行render function，得到新的VNode节点
  * patch
    * 将新的VNode节点与旧VNode节点传入patch比较
    * diff算法得出差异
      * 比对两棵树的差异
      * 同层节点进行比较，时间复杂度为O(n)
    * 将这些差异对应的DOM进行修改即可
* 批量更新
  * 目前浏览器平台没有实现nextTick
  * 将watcher存在一个队列中
  * 对重复的watcher做过滤
  * setTimeout 当前栈执行完毕才执行并清空队列
