# 我眼中的Babel（机翻官网）

在现在前端项目开发的过程中，大家或多或少都会接触过Babel 这一个工具，但由于有很多的项目模板比如[vue-cli](https://github.com/vuejs/vue-cli)  和[create-react-app](https://github.com/facebook/create-react-app)  都是开箱即用，生成的模板里面已经帮大家集成了相关的工具，一部分同学并不了解其中Babel 的功能作用、以及从Babel 衍生出来的各种插件和不同阶提案的转码规则，下面我就结合本身自己的学习和经验大家捋一捋这个Babel 。

## 什么是Babel

官网：https://new.babeljs.io/

github：https://github.com/babel/babel

模块介绍： https://github.com/babel/babel/tree/master/packages

### Babel 是一个Javascript 语法的转换器

Babel 是用在将 ES2015+ 的代码转化成向后兼容的版本，使得转换后的代码能够在当前或者旧版本浏览器以及对应的环境上运行的工具链。

解释一下就是现在的浏览器版本很多，而新推出的ECMAScript 语法或特性在低版本浏览器不能运行，你想使用这些新语法或特性写代码，那就需要用Babel 来转换成这些语法或者特性，使低版本浏览器也可以运行的新语法或特性写出的代码。

下面是官方列出的几种功能：

- 转换语法（新语法转成低版本浏览器支持的语法）
- 补充特性（将新增的特性或者对象属性补充到低版本的环境里）[@babel/polyfill](https://babeljs.io/docs/en/next/babel-polyfill)
- 源代码转换（将类似react或者vue这些有特殊语法结构的框架转成javascript 语法）
- 观看视频了解[更多](https://babeljs.io/videos.html)

 
### Babel 的核心

#### babel-core [@babel/core](https://babeljs.io/docs/en/next/babel-core)

该模块提供一个babel.transform 的方法。包含解析代码和输出代码部分的功能。

可以参考一下[this-super-tiny-compiler](https://github.com/jamiebuilds/the-super-tiny-compiler)

#### babel-cli [@babel/cli](https://babeljs.io/docs/en/next/babel-cli)

提供在命令行上执行转换的工具。

#### babel-polyfill [@babel/polyfill](https://babeljs.io/docs/en/next/babel-polyfill) (适用构建app)

polyfill 包含了整个ES2015+ 以上语法新增的对象特性以及对象方法，比如Promise, Array.from, Object.assign 这些方法等。

主要由[core-js](https://github.com/zloirock/core-js) 和[regenerator-runtime] 这两部分构成，


#### babel-transform-runtime [@babel-plugin-transform-runtime](https://babeljs.io/docs/en/next/babel-plugin-transform-runtime) （适用构建library模块）

简单说就是就是不需要polyfill 的插入，也能够使用Promise，Set 这些新对象特性和方法，从而可以避免引入polyfill 而导致的全局污染。

做三件事：
- 当你使用**generators/async** 方法的时候自动引入 @babel/runtime/regenerator 模块（需要配置 regenerator 参数）
- 可以在必要的时候为core-js 提供帮助方法，而不是让用户猜想是否需要添加polyfill（需要配置corejs 参数）
- 通过替换@babel/runtime/helpers 来自动的去除内嵌的Babel 帮助方法（需要配置helpers 参数）

### Babel 的预置（Presets）

presets 是一系列plugins 的集合。

Babel 官方提供的一系列预置编译器。

#### babel/preset-env [@babel/env](https://babeljs.io/docs/en/next/babel-preset-env) 

默认的**语法转换**以及**特性兼容**编译器。（包含transformation和polyfill 功能）

几个重要的配置参数：

- [useBuiltIns](https://babeljs.io/docs/en/next/babel-preset-env#usebuiltins): 这个参数控制polyfill 的使用方式，有三个值"usage"、"entry"、false，默认为false
    - false： 
    - 'entry': 在项目的入口处，引入polyfill即可，即**import "@babel/polyfill";**其他页面不需要再次额外引入 
    - 'usage': 不需要在项目内手动引入polyfill，babel会根据你页面的代码，以及你配置的targets 参数，在编译过程中自动引入对应的特性或者属性，如果targets 中的浏览器已支持该属性，则babel 不会引入任何模块。

- targets: 目标浏览器，设定该属性后，babel会将新特性转换成能够适配到目标浏览器中的兼容代码。这个配置项参数来自于[browserslist](https://github.com/browserslist/browserslist)。


#### babel/preset-stage-x

在babel7.0 以下的版本，有提出preset-stage-0、preset-stage-1（提议）、preset-stage-2（草案）、preset-stage-3(候选)这四种能够支持不同阶段的新特性的预制编译器，用户可以根据自己的喜好选择不同的编译器（一般会选择比较成熟的stage-3）。

但在babel7.0 以后，babel 提出不再维护以及不建议使用stage-x 相关的编译器，只需要使用preset-env 即可，[相关说明](https://babeljs.io/blog/2018/07/27/removing-babels-stage-presets)。


#### babel/preset-react [@babel/preset-react](https://babeljs.io/docs/en/next/babel-preset-react)

针对react 的语法进行转换，其中包含以下常用的插件。

- @babel/plugin-syxtax-jsx
- @babel/plugin-stransform-react-jsx
- @babel/plugin-transform-react-display-name
- @babel/plugin-transform-react-jsx-self
- @babel/plugin-transform-react-jsx-source


#### 其他常见的官方预置编译器

- [@babel/preset-flow](https://babeljs.io/docs/en/next/babel-preset-flow): 增加了对flow 强类型语法的转换。项目里面用到flow 的话就需要选用这个。
- [@babel/preset-minify](https://babeljs.io/docs/en/next/babel-preset-minify): 对js代码进行压缩转换。
- [@babel/preset-typescript](https://babeljs.io/docs/en/next/babel-preset-typescript): 对typescript 语法进行编译转换。



### Babel 的插件（plugins）

Babel 是一个编译器，提供解析代码输出代码的功能，但是具体要编译成什么样的代码，需要通过**插件（plugins）**来做。

Babel 的插件大致分成两种类型。

- Tranform Plugins：这类插件包含各种ES5+ 的新语法、新特性，以及minify、react、flow、typescript 这些特定语法的各种规则。

- Syntax Plugins：这类插件算是一个语法插件的集合，比如针对编译**jsx**、**flow** 等语法的集合。引入了这类插件就不需要特定的再引入**Transform Plugin** 里面的插件了。


注意：插件的执行的顺序：**从左到右**。presets 的执行顺序：**从右到左**




































