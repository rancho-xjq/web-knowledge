1. [h]chunk、bundle和module有什么区别？
参考：
[webpack 中，module，chunk 和 bundle 的区别是什么？](https://www.cnblogs.com/skychx/p/webpack-module-chunk-bundle.html)

2. [h]hash、chunkhash和contenthash的区别？
hash是跟整个项目的构建相关，只要项目里有文件更改，整个项目构建的hash值都会更改，并且全部文件都共用相同的hash值。(粒度整个项目)
chunkhash是根据不同的入口进行依赖文件解析，构建对应的chunk(模块)，生成对应的hash值。只有被修改的chunk(模块)在重新构建之后才会生成新的hash值，不会影响其它的chunk。(粒度entry的每个入口文件)
contenthash是跟每个生成的文件有关，每个文件都有一个唯一的hash值。当要构建的文件内容发生改变时，就会生成新的hash值，且该文件的改变并不会影响和它同一个模块下的其它文件。(粒度每个文件的内容)
参考：
[webpack4中hash、chunkhash和contenthash三者的区别](https://blog.csdn.net/bubbling_coding/article/details/81561362)

3. [h]简要介绍一下WebPack的底层实现原理？
参考：
[搞点硬货」从源码窥探Webpack4.x原理](https://juejin.cn/post/6844904046294204429)

4. [h]webpack 插件原理，如何写一个插件
参考：
[webpack 插件原理](https://www.cnblogs.com/tugenhua0707/p/11332463.html)

5. [h]webpack 优化
- 构建速度的优化


- 包大小的优化

    - 异步加载模块（代码分割）；
    - 提取第三方库（使用cdn或者vender）；
    - 代码压缩；
    - 去除不必要的插件；
    - 去除devtool选项等等。
参考：
[如何优化 Webpack 的构建速度？](https://juejin.cn/post/6844904094281236487#heading-15)
6. webpack 的 require 是如何查找依赖的
参考：
[webpack如何查找模块依赖](https://segmentfault.com/a/1190000019751035)

7. webpack 如何实现动态加载

8. webpack的作用是什么？开发环境和生产环境之间的配置文件有什么区别？

9. [h]webpack treeShaking原理，是靠什么才能实现？
[Tree-Shaking性能优化实践 - 原理篇](https://juejin.cn/post/6844903544756109319)
[Tree-Shaking性能优化实践 - 实践篇](https://juejin.cn/post/6844903544760336398)


9. webpack的入口文件怎么配置，多个入口怎么分割

13. js的uglify如何实现

14. webpack的css-loader原理讲一下

15. [h]使用过Webpack里面哪些Plugin和Loader
raw-loader：加载文件原始内容（utf-8）
file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件 (处理图片和字体)
url-loader：与 file-loader 类似，区别是用户可以设置一个阈值，大于阈值会交给 file-loader 处理，小于阈值时返回文件 base64 形式编码 (处理图片和字体)
source-map-loader：加载额外的 Source Map 文件，以方便断点调试
svg-inline-loader：将压缩后的 SVG 内容注入代码中
image-loader：加载并且压缩图片文件
json-loader 加载 JSON 文件（默认包含）
handlebars-loader: 将 Handlebars 模版编译成函数并返回
babel-loader：把 ES6 转换成 ES5
ts-loader: 将 TypeScript 转换成 JavaScript
awesome-typescript-loader：将 TypeScript 转换成 JavaScript，性能优于 ts-loader
sass-loader：将SCSS/SASS代码转换成CSS
css-loader：加载 CSS，支持模块化、压缩、文件导入等特性
style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS
postcss-loader：扩展 CSS 语法，使用下一代 CSS，可以配合 autoprefixer 插件自动补齐 CSS3 前缀
eslint-loader：通过 ESLint 检查 JavaScript 代码
tslint-loader：通过 TSLint检查 TypeScript 代码
mocha-loader：加载 Mocha 测试用例的代码
coverjs-loader：计算测试的覆盖率
vue-loader：加载 Vue.js 单文件组件
i18n-loader: 国际化
cache-loader: 可以在一些性能开销较大的 Loader 之前添加，目的是将结果缓存到磁盘里

define-plugin：定义环境变量 (Webpack4 之后指定 mode 会自动配置)
ignore-plugin：忽略部分文件
html-webpack-plugin：简化 HTML 文件创建 (依赖于 html-loader)
web-webpack-plugin：可方便地为单页应用输出 HTML，比 html-webpack-plugin 好用
uglifyjs-webpack-plugin：不支持 ES6 压缩 (Webpack4 以前)
terser-webpack-plugin: 支持压缩 ES6 (Webpack4)
webpack-parallel-uglify-plugin: 多进程执行代码压缩，提升构建速度
mini-css-extract-plugin: 分离样式文件，CSS 提取为独立文件，支持按需加载 (替代extract-text-webpack-plugin)
serviceworker-webpack-plugin：为网页应用增加离线缓存功能
clean-webpack-plugin: 目录清理
ModuleConcatenationPlugin: 开启 Scope Hoisting
speed-measure-webpack-plugin: 可以看到每个 Loader 和 Plugin 执行耗时 (整个打包耗时、每个 Plugin 和 Loader 耗时)
webpack-bundle-analyzer: 可视化 Webpack 输出文件的体积 (业务组件、依赖第三方模块)

16. Dev-Server是怎么跑起来

17. 介绍AST（Abstract Syntax Tree）抽象语法树

19. [h]Webpack如何配Sass，需要配哪些Loader
``` js
module.exports = {
  module: {
    rules: [
      {
        test: /\.s[ac]ss$/i,
        use: [
          // 将 JS 字符串生成为 style 节点
          'style-loader',
          // 将 CSS 转化成 CommonJS 模块
          'css-loader',
          // 将 Sass 编译成 CSS
          'sass-loader',
        ],
      },
    ],
  },
};
```

20. 配CSS需要哪些Loader

21. 如何配置把JS、CSS、Html单独打包成一个文件

22. 如何实现异步加载

23. 如何实现分模块打包（多入口）

24. 打包时Hash码是怎么生成的

25. 使用Webpack构建时有无做一些自定义操作

26. [h]webpack中的loader和plugin有什么区别
(答案参考童欧巴的一篇webpack面试文章哦：[「吐血整理」再来一打Webpack面试题(持续更新)](https://juejin.cn/post/6844904094281236487)
Loader 本质就是一个函数，在该函数中对接收到的内容进行转换，返回转换后的结果。
因为 Webpack 只认识 JavaScript，所以 Loader 就成了翻译官，对其他类型的资源进行转译的预处理工作。
Plugin 就是插件，基于事件流框架 Tapable，插件可以扩展 Webpack 的功能，在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。
Loader 在 module.rules 中配置，作为模块的解析规则，类型为数组。每一项都是一个 Object，内部包含了 test(类型文件)、loader、options (参数)等属性。
Plugin 在 plugins 中单独配置，类型为数组，每一项是一个 Plugin 的实例，参数都通过构造函数传入。


28. webpack如果使用了hash命名，那是每次都会重写生成hash吗
有三种情况：

如果是hash的话，是和整个项目有关的，有一处文件发生更改则所有文件的hash值都会发生改变且它们共用一个hash值；
如果是chunkhash的话，只和entry的每个入口文件有关，也就是同一个chunk下的文件有所改动该chunk下的文件的hash值就会发生改变
如果是contenthash的话，和每个生成的文件有关，只有当要构建的文件内容发生改变时才会给该文件生成新的hash值，并不会影响其它文件。

29.  [h]webpack中如何处理图片的？
在webpack中有两种处理图片的loader：

file-loader：解决CSS等中引入图片的路径问题；(解决通过url,import/require()等引入图片的问题)
url-loader：当图片小于设置的limit参数值时，url-loader将图片进行base64编码(当项目中有很多图片，通过url-loader进行base64编码后会减少http请求数量，提高性能)，大于limit参数值，则使用file-loader拷贝图片并输出到编译目录中；

参考： [霖呆呆的webpack之路-loader篇](https://github.com/LinDaiDai/niubility-coding-js/blob/master/%E5%89%8D%E7%AB%AF%E5%B7%A5%E7%A8%8B%E5%8C%96/webpack/%E9%9C%96%E5%91%86%E5%91%86%E7%9A%84webpack%E4%B9%8B%E8%B7%AF-loader%E7%AF%87.md#file-loader)
