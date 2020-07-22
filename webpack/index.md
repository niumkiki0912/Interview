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

#### 7. 是否写过Loader和Plugin？描述一下编写loader或plugin的思路？

#### 8. webpack的热更新是如何做到的？说明其原理？

#### 9. 如何利用webpack来优化前端性能？（提高性能和体验）

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