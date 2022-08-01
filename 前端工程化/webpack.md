# webpack
`webpack`是一款热门的模块打包工具，其本质是将多个模块打包成一个或多个bundle。

## 打包原理

1. 读取webpack参数
2. 启动webpack，从入口文件（entry）开始解析，对其中导入的模块，递归遍历分析，形成依赖树。
3. 对不同的文件类型的依赖模块文件使用对应的`loader`编译为js文件。
4. webpack执行的生命周期内会通过发布订阅模式发布一些事件，而webpack插件可以通过监听这些事件，执行插件任务进而干预编译结果。

## Loader
webpack默认只能够处理js文件，对于一些非js类型文件，会通过对应的loader转为js类型。loader可以通过数组形式配置多个，webpack在转换时会链式调用每个loader，前一个loader返回的内容会作为后一个loader的入参。

## Plugin
`plugin`负责webpack功能扩展，也就是webpack本身不具有某些功能，但是它预留的接口，我们就可以通过添加插件来完成某些拓展的功能。

## sourceMap
`sourceMap`是将编译、打包、压缩后的代码映射回源代码的一种技术。由于打包后的代码简直不可阅读，对于debug来说很糟糕，但是sourceMap可以定位其源代码的位置，提高开发效率。

## Tree-Shaking
Tree-Shaking 是一种基于 ES Module 规范的 `Dead Code Elimination` 技术。也就是说Tree-Shaking会保留有实际作用的模块代码，对于一些导出但没有使用的模块，将其删除，实现打包产物优化。

使用Tree-Shaking必须使用ES Module规范编写代码，因为ESM下的模块之间的依赖关系与运行状态无关，且它要求所有导入导出只能出现在模块顶层，且导入导出模块名为字符串常量。这样，编译工具只需要对ESM模块做静态分析，就可以推断出哪些模块未被其他模块使用。
而对于CommonJs、AMD、CMD等，导入导出的行为是动态的，难以预测。

## 热更新
`Hot Module Replacement`，简称`HMR`，无需刷新整个页面的同时，更新模块。在日常开发中，节省时间、提升体验

## 优化

1. 费时分析
   **speed-measure-webpack-plugin**这个插件可以测量各个插件和loader所花费时间
2. 缩小范围
   **include/exclude**在配置loader时加上这两个属性，更精准的去确定loader的作用目录或需要排除的目录。
   `include`表示要包含的文件
   `exliude`表示要排除的文件。优先级更高。
3. 优化resolve配置
   resolve配置webpack如何寻找模块对应的文件。
   `alias`创建import和require的别名
   `extensions`引入模块不带扩展名时会按照extensions的配置数组从左到右去解析模块
4. noParse
   对于不需要依赖解析的大型第三方库，如jquery，可以通过`noParse`字段忽略模块文件不解析import和require
5. ignorePlugin
  `webpack`的内置插件，忽略第三方指定目录
6. 多进程
7. thread-loader
  配置在 thread-loader 之后的 loader 都会在一个单独的 worker 池（worker pool）中运行
8. 缓存
   babel-loader将bable转译js结果缓存起来
   cache-loader缓存loader的处理结果
9. 压缩css 
  optimize-css-assets-webpack-plugin
10. 压缩jswebpack5 
  内置了terser-webpack-plugin 插件
11. 清除无用的 CSS 
  purgecss-webpack-plugin 会单独提取 CSS 并清除用不到的 CSS
