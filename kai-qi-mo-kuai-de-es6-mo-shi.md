## 开启模块的 ES6 模式 {#开启模块的-ES6-模式}

Babel 是一个插件式的 JavaScript 编译器， 能将一些当前 JS 引擎中不支持的特性和语法， 通过一个个特定插件，转换成当前引擎可以理解的 JS 脚本。 我们可以使用 Babel 来转换我们的 Node.js 脚本。

接下来， 我们就可以去 Babel 插件列表去选择对应的转换插件来为我们的 Node.js 插上隐形的翅膀了。

### 首先， 我们需要确认我们需要 Babel 添加哪些特性支持。 {#首先，-我们需要确认我们需要-Babel-添加哪些特性支持。}

**选择 ES 转换的原则**

1. 优先使用原生特性
2. 优先选择那些稳定实现的特性。 由于一些 ES 特性需要引擎的底层支持才能完美支持， 通过代码转换可能很难完美支持， 对于这种特性只能不用或少用

基于这个原则， 小明筛选出如下插件。

* transform-strict-mode （由于很多 ES 特性需要 严格模式才能打开， 添加这个插件就会自动在所有文件上添加
  `'use strict';`
  ）
* transform-es2015-modules-commonjs （将 ES6 模块标准 转换成 Node.js 用的 CMD 模块标准）
* transform-es2015-spread （支持
  [ES6 的 spread 操作符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)
  ）
* transform-es2015-destructuring （支持
  [赋值解构](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
  ）
* transform-es2015-parameters （支持默认参数， 参数解构， 以及其他参数）

#### 转换示例： import {#转换示例：-import}

from

```
import Mod from './mod';
new Mod();
```

to

```
'use strict';

var _mod = require('./mod');
var _mod2 = _interopRequireDefault(_mod);

function _interopRequireDefault(obj) {
  return obj && obj.__esModule ? obj : { default: obj }; 
}

new _mod2.default();
```

#### 转换示例： export {#转换示例：-export}

from:

```
export default class Mod {

}

```

to:

```
"use strict";

Object.defineProperty(exports, "__esModule", {
  value: true
});
class Mod {}
exports.default = Mod;

```

上面这些选择的插件可以根据个人口味以及 Node.js 版本 进行添加或删除。选好模块， 我们就可以安装插件以及创建对应得babel配置文件去处理

### 安装插件 {#安装插件}

在 npm 模块或应用目录下执行

```
# 安装 core 和命令行工具
$ npm install --save-dev babel-core babel-cli 
# 安装所有插件
$ npm install --save-dev babel-plugin-transform-strict-mode babel-plugin-transform-es2015-modules-commonjs  babel-plugin-transform-es2015-spread babel-plugin-transform-es2015-destructuring babel-plugin-transform-es2015-parameters
```

### 添加配置文件 {#添加配置文件}

在应用根目录下面创建对应的配置文件`.babelrc`,

```
{
  "plugins": [
    "transform-strict-mode",
    "transform-es2015-modules-commonjs",
    "transform-es2015-spread",
    "transform-es2015-destructuring",
    "transform-es2015-parameters"
  ]
}
```

### 文件组织 {#文件组织}

由于 Node.js本身有加载器， 所以不需要将所有文件打包成一个文件， 推荐的做法是， 添加一个`src`目录， 用于存放 ES6 脚本， 然后将整个目录打包到`lib`目录下， 对应的脚本为

```
babel src --out-dir lib

```

开发调试的时候， 可以直接用 babel-cli 模块提供`babel-node`代替`node`直接启动`src`目录下面的入口脚本。

```
babel-node src/index.js

```

最后， 将命令封装到`package.json`里面

```
{
  "name": "my-awasome-es6-package",
  "version": "1.0.0",
  "description": "use es6 in node",
  "main": "lib/index.js",
  "scripts": {
    "start": "babel-node src/index.js",
    "build": "babel src --out-dir lib"
  },
  "devDependencies": {
    "babel-cli": "~6.3.17",
    "babel-core": "~6.3.26",
    "babel-plugin-syntax-object-rest-spread": "^6.3.13",
    "babel-plugin-transform-es2015-modules-commonjs": "^6.3.16",
    "babel-plugin-transform-es2015-spread": "^6.3.14",
    "babel-plugin-transform-object-rest-spread": "^6.3.13",
    "babel-polyfill": "^6.3.14"
  }
}

```

我们就可以使用下面的命令启动和编译我们的代码了

```
# npm run build 构建脚本
# npm start 使用 babel-node 启动进程

```

这里的注意点：

* 模块入口 main 应该指向构建后的脚本， 这样用你模块的用户不需要去进行编译， 以及线上运行得时候不用去编译。
* `babel-node`
  不适合用于生产环境，生产环境应使用编译后的代码。 不过在开发环境里面使用还是挺方便的。

### 接下来要做的事情 {#接下来要做的事情}

接下来你可能会问

* 测试怎么办
* 怎么 Debugging

大家可以在这里 \([Using Babel](https://babeljs.io/docs/setup/)\) 查到更多感兴趣的信息

## 参考地址 {#参考地址}

* [Node.js ES6 支持文档](https://nodejs.org/en/docs/es6/)
* [Babel 插件列表](http://babeljs.io/docs/plugins/)
* [ES6 兼容列表](http://kangax.github.io/compat-table/es6/)



