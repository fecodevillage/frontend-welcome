在现在前端项目开发的过程中，大家或多或少都会接触过Babel 这一个工具，但由于有很多的项目模板比如[vue-cli](https://github.com/vuejs/vue-cli)  和[create-react-app](https://github.com/facebook/create-react-app)  都是开箱即用，生成的模板里面已经帮大家集成了相关的工具，一部分同学并不了解其中Babel 的功能作用、以及从Babel 衍生出来的各种插件和不同阶提案的转码规则，下面我就结合本身自己的学习和经验大家捋一捋这个Babel 。

## 什么是Babel

官网：https://new.babeljs.io/

github：https://github.com/babel/babel

模块介绍： https://github.com/babel/babel/tree/master/packages

### Babel 是一个Javascript 语法的转换器

Babel 是用在将 ECMAScript2015+ 的代码转化成向后兼容的版本，使得转换后的代码能够在当前或者旧版本浏览器以及对应的环境上运行的工具链。

简而言之，现在的浏览器版本很多，如果需要在低版本的浏览器运行新的代码特性，那就需要用Babel 来转换成旧版本浏览器可以运行的版本。

下面是官方列出的几种功能：

- 转换语法（新语法转成低版本支持的语法）
- 补充特性（将新增的特性补充到低版本的环境里）[@babel/polyfill](https://babeljs.io/docs/en/next/babel-polyfill)
- 源代码转换（将类似react或者vue这些有特殊语法结构的框架转成javascript 语法）
- [其他更多](https://babeljs.io/videos.html)

 
### Babel 的核心

#### core [@babel/core](https://babeljs.io/docs/en/next/babel-core)

主要的转换代码在这个模块，提供一个babel.transform 的方法

可以参考一下[this-super-tiny-compiler](https://github.com/jamiebuilds/the-super-tiny-compiler)

#### cli [@babel/cli](https://babeljs.io/docs/en/next/babel-cli)

提供在命令行上执行转换的工具

#### polyfill [@babel/polyfill](https://babeljs.io/docs/en/next/babel-polyfill)

polyfill 包含了整个ES2015+ 以上语法新增的对象特性以及对象方法，比如Promise, Array.from, Object.assign 这些方法等。

主要由[core-js](https://github.com/zloirock/core-js) 和[regenerator-runtime] 这两部分构成，

其中

#### transform-runtime

简单说就是就是不需要polyfill 的插入，也能够使用Promise，Set 这些新对象特性和方法，从而可以避免引入polyfill 而导致的全局污染。

做三件事：
- 当你使用generators/async 方法的时候自动引入 @babel/runtime/regenerator 模块（需要配置 regenerator 参数）
- 可以在必要的时候为core-js 提供帮助方法，而不是让用户猜想是否需要添加polyfill（需要配置corejs 参数）
- 通过替换@babel/runtime/helpers 来自动的去除内嵌的Babel 帮助方法（需要配置helpers 参数）

### Babel 的预置（Presets）

Babel 官方提供的一系列转换器，

#### [babel/env]() 默认的语法兼容转换器

几个重要的配置参数：

- [useBuiltIns](https://babeljs.io/docs/en/next/babel-preset-env#usebuiltins): 

































