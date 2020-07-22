### 面试题
#### 1. webpack中的bundle、module、chunk分别是什么

#### 2. webpack与gulp的不同

-------------------------
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

---------------------   
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
----------------------

#### 5. Loader和Plugin的不同？
#### 6. webpack的构建流程是什么?从读取配置到输出文件这个过程尽量说全
#### 7. 是否写过Loader和Plugin？描述一下编写loader或plugin的思路？
#### 8. webpack的热更新是如何做到的？说明其原理？
#### 9. 如何利用webpack来优化前端性能？（提高性能和体验）
#### 10. 如何提高webpack的构建速度？
#### 11. 怎么配置单页应用？怎么配置多页应用？
#### 12. npm打包时需要注意哪些？如何利用webpack来更好的构建？
#### 13. 如何在vue项目中实现按需加载？