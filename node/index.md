### 面试题
#### 1. 跨域相关
+ 如何解决跨域
    + JSONP
    + njnx 反向代理
    + cors跨域资源共享
        + 服务器通过设置'Access-control-Allow-Origin'来实现跨域
        + 如果要带coockie的话要设置'Access-control-Allow-Credentials'来处理,否则仍会跨域
        + 要注意对OPTIONS方法也要处理跨域问题
+ 跨域跨的哪?谁不让过?
    + 域名,端口,协议
    + 浏览器
+ 正向代理与反向代理

#### 2.OPTIONS 预请求
    + 资料: https://www.jianshu.com/p/b55086cbd9af

#### 3.node有哪些常用模块?
  + fs -- 文件模块
  + http -- 处理网络客户端请求
  + url模块 --- 处理客户端请求过来的url
  + Query Strings模块 --- 处理客户端传递过来的参数
  + path模块 --- 处理文件路径
  + net -- 网络通信协议
  + process -- 进程模块
     + 常用与获取用户系统信息，如操作系统，环境变量等

#### 4.事件循环
+ https://juejin.im/post/6844904056457003021
+ 概念： 线程、进程、cpu
##### 浏览器
+ Browser进程: 浏览器主进程
+ 第三方插件进程
+ GPU进程： 最多一个，用于3d绘制
+ 浏览器渲染进程(内核)-- 存在多个线程
    + GUI渲染线程
        + 负责渲染浏览器界面，解析HTML、CSS
        + 当页面重绘、回流，该线程会执行
        + 与js线程互斥
    + JS引擎线程
        + 负责js脚本程序
        + 与GUI线程互斥
    + 事件触发线程
        + 控制事件循环
        + 当js引擎执行代码时，会将对应任务添加到事件触发线程中
        + 当对应事件触发，线程会把事件添加到任务队列对位，等线应
    + 定时触发线程
        + setTimeout()/setInterval()所在的线程
    + 异步http请求线程
    + XMLHttpRequest
+ 为什么js引擎是单线程的
    + JavaScript的主要用途是与用户互动，以及操作DOM。这决定它只能是单线程，否则会带来很复杂的同步问题。比如，假JavaScript同时有两个线程，一个线程在某个DOM节点上添加容，另一个线程删除了这个节点，这时浏览器应该以哪个线程准？
+ 事件循环
    + js分为同步任务和异步任务
    + 所有的同步任务都在主线程上执行,形成一个执行栈
    + 主线程之外,存在一个任务队列。只要异步任务有了运行结果，在"任务队列"之中放置一个事件。
    + 一旦执行栈中的所有同步任务执行完毕,系统就会读取任务队列后渲染,将队列中的事件放到之执行栈中依次执行
    + 主线程从任务队列中读取事件,这个过程是循环不断的
+ 宏任务与微任务
    + 任务队列分为宏任务与微任务.
    + microtask: 更新应用程序状态的任务，包括promise回调，MutationObserver，process.nextTick，Object.observe
    + macrotask: 包含执行整体的js代码，事件回调，XHR回调，定时器（setTimeout/setInterval/setImmediate），IO操作，UI render
    + 事件循环分为三步
        1. 执行macrotask队列的一个任务
        2. 执行完当前microtask队列的所有任务
        3. UI render
##### node
+ 采用事件驱动和异步I/O的方式,实现了一个单线程、高并发的js运时环境，底层封装的是libuv
+ 运行机制
    1. V8引擎解析JavaScript脚本。
    2. 解析后的代码，调用Node API。
    3. libuv库负责Node API的执行。它将不同的任务分配给不同线程，形成一个Event Loop（事件循环），以异步的方式将任务执行结果返回给V8引擎。
    4. V8引擎再将结果返回给用户。
+ 初始化
    + 同步代码
    + 清空NextTick Quene
    + 清空Microtask Quene
+ event Loop
    + timers---执行满足条件的`setTimeout() 和 setInterval()`回调
    + I/O callbacks --- 执行延迟到下一个循环迭代的I/O回调
    + poll --- 等待还没完成的I/O事件,会因timers和超时结束等待
    + checks --- 执行setImmediate() 回调
    + close callbacks --- 关闭所有closing handles

