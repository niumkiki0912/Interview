## vue面试训练营 --- kkb
#### 1. v-if和v-for哪个优先级高？ 如果两个同时出现，应该怎么优化得到更好的性能？
+ v-for优先级高于v-if
    + 在vue源码, src\core\instance\state.js中，会有一个if判断，v-for的判断优先于v-if，也就是说只要有v-for，就会直接进入其语句判断中
    ![avatar](./微信截图_20200804122518.png)
+ 如果同时出现，每次渲染都会先执行循环再判断，无论如何都无法避免循环，浪费性能
+ 如果要避免： 再外层嵌套template， 在这一层进行v-if判断，然后再内部循环遍历
+ 如果条件出现在循环内部，可以通过计算属性过滤掉不需要的项


#### 2.Vue组件data为什么必须是个函数而Vue的根实例则没有此限制？

#### 3.你知道vue中key的作用和工作原理吗？说说你对它的理解。
+ key的主要作用是为了高效的更新虚拟DOM
    + 源码：在patch的过程中会执行pacthVnode,pacthVnode的过程中会执行updateChildren这个方法；会去更新所有新旧子元素。那么在这个过程中，通过key就可以精确的判断两个节点是否是同一个。如果没有key的话就会永远认为是一个相同节点，那么所作的操作就是一定会强制更新，那么就无法避免频繁更新dom元素，性能会很差；如果加上key, 就会利用内部diff算法，避免频繁更新不同元素，使得整个patch过程更加高效，减少DOM操作，提高性能
+ 如果不设置key, 有可能会引起一些隐蔽的bug
+ vue中在使用相同标签名元素的过度切换时，也会用到key，目的也是为了让vue可以区分塔曼，否则vue只会替换其内部属性而不会触发过渡效果

#### 4.你怎么理解vue中的diff算法？
+ diff算法是用来计算虚拟dom中被改变的部分。在vue，react中都有使用
+ vue中的diff算法
    + 必要性： lifecycle.js - mountComponent()
               组件中可能存在多个data中的key的使用，一个组件一个watcher,那么就使用diff就很必要了
    + 执行方式


#### 5.vue-router原理
+ 通过vue的插件机制，使用vue.use(vueRouter)时，会调用vue-router的install方法，装载$router
+ 通过vue的混入机制，在beforeCreate中将$store挂载到vue上
+ 原理核心：更新视图但是不重新请求页面
    + hash: 利用url的hash
        + hash虽然出现在url中，但他不会被包括在http请求中，是用来指导浏览器动作的，对服务器端完全无用；改变hash并不会重新加载页面
        + 利用hashchange事件，当监听到hash发生变化时，可以通过window.location.hash拿到当前hash值，
    + history： 利用h5中history新方法
        + 通过pushState和replaceState来对浏览器历史记录修改，当调用这两个方法时，虽然url改变了，但是浏览器并不会立即发送请求
        + 当浏览器跳转到新的状态时，将触发popState事件
        + 问题：当用户直接在地址栏中输入并回车，浏览器重启重新加载应用时，history模式会将URL修改得就和正常请求后端的URL一样，在此情况下重新向后端发送请求，如后端没有配置对应/  user/id的路由处理，则会返回404错误。所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。
+ 并利用Vue提供的defineReactive做响应化，当_router发生变化时，会自动调用vue的render方法，此时会将routes表中相对应的components替换当前router-view中的components
+ 有两个组件router-view、router-link
+ 模式参数： 通过mode控制


#### 6.vuex相关面试题
+ 是一个专门为vue开发的状态管理模式。 实现了一个单向数据流，在全局拥有一个state存放数据
+ 和全局对象的区别
    + 状态存储是响应式的
    + 不能直接修改state的状态：改变store状态的唯一途径就是显示的提交(commit)mutation，这样可以方便我们跟踪每一个状态的变化
    + 异步: 异步操作都在actions中，action提交mutation

        dispatch            commit
    |-----------> action --------> |  
    |                              |  
    vue                            mutations <===> devtools 
    |                              |  
    |  render             mutate   |  
    |-----------> state ---------->|  

+ vuex借鉴了flux和redux,将数据存放到全局的store中，再将store挂载到每个vue实例组件中，利用vue的数据响应式机制来进行状态更新
+ vue的store是如何挂载注入到组件中呢
    + 利用vue的插件机制，使用vue.use(vuex)时，会调用vuex的install方法，装载vuex
    + 利用vue的mixin混入机制，在beforecreate中将store挂载到vue原型上
+ vuex的state和getters是如何映射到各个组件实例中响应式更新状态呢
    + 借助vue的数据响应式，将state存入vue实例组件的data中；getter借助vue的计算属性computed实现数据实时监听


#### 7.vue3 proxy的原理


#### 8. 
