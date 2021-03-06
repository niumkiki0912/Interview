### 面试题
#### 1. webpack中的bundle、module、chunk分别是什么

#### 2. webpack与gulp的不同

---
<br/>

#### 3. 有哪些常见的Loader？他们是解决什么问题的？
+ 文件：
    + file-loader
    + url-loader：功能类似于file-loader, 但可以设置limit大小来对图片进行处理，对小于limit的图片转化为base 64 格式
+ 样式: 
    + style-loader： 将css代码注入到js中，插入到DOM中，`<style></style>`
    + css-loader: 加载css，支持模块化、压缩、文件导入
    + less-loader
    + sass-loader
    + postcss-loader: autoprefixer 自动增加浏览器前缀
+ 转换编译
    + babel-loader
        + babel-loader: 开通webpack与babel的桥梁
        + @babel/preset-env: 包含es6、7、8转换es5的规则
        + @babel/polyfill： 垫片，把es新特性加载进来，来弥补一版本浏览器中缺失的特性，例如promise
        + @babel/plugin-transform-runtime：当开发组件库时，polyfill就不适合了，所以推荐闭包方式
+ 框架
    + vue-loader: 加载和转移vue组件

--- 
<br/>

#### 4. 有哪些常见的Plugin？他们是解决什么问题的？
+ htmlWebpackPlugin: 自动生成html文件，并且将js文件自动引入

+ cleanWebpackPlugin: 重新打包时，清除打包文件中的呢日哦那个

+ HotModuleReplacementPlugin: 热模块替换，webpack自带

+ PurifyCSS: tree Shaking， 打包时去掉没有用到的css代码

+ optimizeCssAssetsPlugin: 压缩css代码

+ MiniCssExtractPlugin： css模块化

+ BundleAnalyzerPlugin：分析打包后各模块的依赖关系

+ dllPlugin: 
    + 打包第三方类库，拆分bundles，优化构建的性能，节省打包速度；dll---动态链接库，用来处理缓存
    + 这个插件，是在独立的webpack设置中创建一个只有dll的bundle。该插件会生成一个manifest.json文件，这个文件是用来让DllReferencePlugin映射到相关依赖上去的

+ DllReferencePlugin：
    + 在webpack主配置文件中设置，通过引用 dll 的 manifest 文件来把依赖的名称映射到模块的 id 上

+ HardSourceWebpackPlugin：硬件缓存

---
<br/>

#### 5. Loader和Plugin的不同？
+ 作用不同
    + loader: 加载器，webpack本身只能解析js文件，如果想要解析和打包其它文件的话，就需要用到loader了，所以其作用是让webpack能够加载和解析非js文件
    + plugin: 插件，可以扩展webpack的功能，用于执行范围更广的任务，让webpack更加灵活
+ 用法不同
    + loader: 在module.rules中配置
    ```javascript
        module: {
            rules: [
                {
                    test: /\.css$/,
                    use: ['style-loader', 'css-loader']
                }
            ]
        }
    ```
    + plugin: 在plugin中直接配置
    ```javascript
        plugin: [
            new HtmlWebpackPlugin()
        ]
    ```

---
<br/>

#### 6. webpack的构建流程是什么?从读取配置到输出文件这个过程尽量说全
+ 在webpack.js文件中，生成一个Webpack的类，new 一个Webpack对象
+ 从webpack.config.js中读取配置项，传入Webpack中
+ 确定入口：根据配置项中的entry找出入口文件
+ 开始编译：执行对象的run方法开始编译，调用`parse(this.entry)`,并且递归parse直到所有入口依赖的文件都经过了parse处理，保存返回的数据，然后调用file方法
+ 编译模块：parse方法
    + 从入口文件出发，使用fs读取文件内容，使用bable/parser中的parse方法，将内容转化为AST抽象语法树。
    + 然后使用bable/traverse方法，处理文件依赖，保存'引用文件：引用文件地址'到dependencies中
    + 使用babel/core中的 transformFromAst 处理代码，并将code,dependencies还有文件地址作为对象一起返回
+ 创建自运行函数
调用所有的Loader对模块进行编译，再找出该模块

---
<br/>

#### 7. 是否写过Loader和Plugin？描述一下编写loader或plugin的思路？

---
<br/>

#### 8. webpack的热更新是如何做到的？说明其原理？
---
<br/>

#### 9. 如何利用webpack来优化前端性能？（提高性能和体验）
+ 对图片进行压缩
    + url-loader: 可以设置limit大小来对图片进行处理，对小于limit 的图片转化为base64格式
    + 对于较大的图片，可以用image-webpack-loader来压缩图片
    ```javascript
        {
            test: /\.(png|jpg|jpe?g|gif)$/,
            use: [
                {
                    loader: 'url-loader', 
                    options: {
                        name: '[name]_[hash:6].[ext]', 
                        outputPath: 'images',
                        limit: 1024 * 12 //单位是字节
                    }
                },
                {
                    loader: 'image-webpack-loader', 
                    options: {
                        bypassOnDebug: true
                    }
                },
            ]
        }
    ```

+ 生产环境压缩代码
    + optimizeCssAssetsPlugin：使用cssnano压缩css代码
    + HtmlWebpackPlugin插件中的minify选项: 压缩html代码
    
+ tree-shaking
    + 删除没有用到的死代码

+ 利用cdn加速
    + 在构建过程中，将引用的静态资源路径修改为cdn上对应的路径，可以利用webpack outPut中的publicPath去修改资源路径

+ 减少es6转es5冗余代码
    + Babel插件会在将ES6转换为ES5时注入一些辅助代码，例如：
    `class HelloWebpack extends Component{...}`

    + 这段代码再被转换成正常运行的es5代码时，需要一下两个辅助函数
    ```javascript
        babel-runtime/helpers/createClass // 用于实现class语法
        babel-runtime/helpers/inherits // 用于实现extends语法
    ```

    + 默认情况下，Bable会在每个输出文件中内嵌这些依赖的辅助函数代码，如果多个源文件都依赖这些辅助函数，那么就会出现很多次，造成代码冗余

    + 为了解决这个问题，使用`babel-plugin-transform-runtime`，将相关辅助函数替换成导入语句，从而减小babel编译出来的代码的文件大小

    ```javascript
        // 在.babelrc配置文件中添加
        plugin: [
            "transform-runtime"
        ]
    ```

+ 提取公共代码
    + commonChunkPlugin(webpack v4 移除): `new webpack.optimize.CommonsChunkPlugin(options)`
    + SplitChunksPlugin(webpack v4 替代上者): https://juejin.im/post/5b99b9cd6fb9a05cff32007a
---
<br/>

#### 10. 如何提高webpack的构建速度？
+ 使用DllPlugin和DllReferencePlugin来拆分bundles：

    > 项目中引入了很多第三方类库，这个库很长时间不会有变更，打包的时候分开打包能够提升打包速度；而dllPlugin(动态链接库插件),原理就是把网页依赖的基础模块抽离出来打包到dll文件中，当需要导入的模块存在于dll中时，这个模块不再被打包，而是去dlll中获取
    + dllPlugin: 用于打包出一个单独的动态链接库文件
    + DllReferencePlugin：用于在主配置的文件中引入dllPlugin打包好的动态链接库文件， 通过引用 dll 的 manifest 文件来把依赖的名称映射到模块的 id 上

+ HardSourceWebpackPlugin
    + 硬件缓存：提供中间缓存的作用，首次构建时没有太大的变化，第二次构建时间就会有较大节省

+ happyPack
    + 多线程预编译：能够将任务分解给多个子进程去并发执行，子进程处理完后再将结果发送给主进程

+ tree-shaking
    + 摇树，打包时剔除调多余代码，进而提升构建速度

+ 多入口情况下，使用CommonsChunkPlugin来提取公共代码 ?
+ 通过externals配置来提取常用库 ?
+ 使用webpack-uglify-parallel来提升uglifyPlugin的压缩速度。 原理上webpack-uglify-parallel采用了多核并行压缩来提升压缩速度? 
---
<br/>

#### 11. 怎么配置单页应用？怎么配置多页应用？
---
<br/>

#### 12. npm打包时需要注意哪些？如何利用webpack来更好的构建？
---
<br/>

#### 13. 如何在vue项目中实现按需加载？
---
<br/>


https://zhuanlan.zhihu.com/p/44438844