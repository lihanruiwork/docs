# node基础
* Js限制在浏览器中运行的，他的能力取决于浏览器中间层提供的支持有多少
* js的表现能力取决于宿主环境中的API的支持程度
* Node的结构与Chrome相似，除了html、ui相关
* 基于事件驱动、异步架构
* node通过事件驱动服务IO
* 在node中，js可以访问本地文件、搭建websocket服务器、连接数据库、多进程
* node打破了js只能在浏览器中运行的局面
* 前后端编程的环境统一，大大降低前后端转换所需要的上下文交换代价
* Node-webkit桌面应用开发
* 单线程
* 子进程使得node可以应对单线程在健壮性和无法利用多核CPU方面的问题
* I/O密集型：面向网络、擅长并行I/O，能够有效地组织起更多硬件资源
* 利用事件循环处理，而不是启动每一个线程为每一个请求服务，资源占用极少
* 模块
    * common.js   require(“math”)
    * 在node中，一个文件就是一个模块
    * 在node中引入模块，3个步骤 路径分析、文件定位、编译执行
    * 核心模块 & 文件模块
    * 核心模块在node进程启动时，就已经加载进内存了，加载速度超快
        * 顺序：缓存、核心模块、. .. /(当做文件模块，将路径转为真实路径)
        * 只有个文件名（当前文件的node_modules、父目录下node_modules、。。。直到根目录下）
        * 文件路径越深，查找耗时越多
* 浏览器端js从同一个服务器分发到多个客户端执行，瓶颈带宽，网络加载
* 服务器端js相同的代码多次执行，瓶颈CPU和内存等资源、磁盘加载
* AMD CMD define模块方式不同
* 单线程：远离多线程死锁
* 异步I/O：让单线程远离阻塞，更好的使用CPU
* Tick：进程启动，开启循环，查看是否有事件待处理，如果有就取出事件和回调，如果存在回调就执行，然后进入下一个循环
    * 如何判断是否有事件待处理：观察者
    * 异步I/O、网络请求是事件的生产者
    * 异步API：I/O、setTimeout、setInterval、setImmediate、process.nextTick
    * 定时器观察者，每次Tick执行时，跟定时器观察者取出定时器对象，检查是否超过定时时间，yes就形成一个事件，他的回调函数会立即执行
        * 问题：尽管事件循环非常快，如果某次循环占用的时间较多，不精确
        * setTimeout浪费性能，调红黑树
    * Process.nextTick：回将回调函数放入队列，在下一轮Tick时取出
    * setImmediate：优先级低于process.nextTick
    * 事件循环对观察者的检查是有先后顺序的
        * idle观察者 先于 I/O 先于 check
            * Process.nextTick： idle观察者
            * process.nextTick 回调函数保存在数组中，每轮循环中会将数组中的回调函数全部执行完
        * setImmediate：check观察者
            * setImmediate 链表，每次只执行一个回调
    * 事件驱动的实质：主循环+事件触发
    * 宏任务：setTimeout setImmediate
    * 微任务 promise.then  process.nextTick
    * 执行一个宏任务，遇到微任务，就加到微任务队列中，然后执行所有微任务，然后开启下一次事件循环
