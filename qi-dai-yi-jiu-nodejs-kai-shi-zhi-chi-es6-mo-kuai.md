# 期待已久 Nodejs 开始支持 ES6 模块

前几天，看到 Chrome 61 发布，支持 ES6 模块。

[Chrome 61 Beta：JavaScript 模块，桌面端的支付请求 API](https://segmentfault.com/a/1190000010749468)

```
<script type="module" src="main.js"></script>

<script type="module">
    import { addTextToBody } from './utils.js';
    addTextToBody('Modules are pretty cool.');
</script>
```

补充：ES6 模块在浏览器上使用参考，[ECMAScript modules in browsers](https://jakearchibald.com/2017/es-modules-in-browsers/)

随后，Nodejs 迅速跟上，更新到 v8.5.0，实验性的支持 ES6 模块。

### 支持 ES6 模块

在 v8.5.0 的提案中，可以看到 Nodejs 开始实验性支持 ES6 模块，不过需要命令来开启。[v8.5.0 proposal \#15308](https://github.com/nodejs/node/pull/15308)

![](https://dn-cnode.qbox.me/Fukg8MOYu_EH4RZdZ4km9W9FZjM0 "20170914093620.png")

这个新功能的大部分功劳都归功于 **Bradley Farias**。[module: Allow runMain to be ESM \#14369](https://github.com/nodejs/node/pull/14369)

### 使用姿势

文件结构：

```
demo/
    lib.mjs
    main.mjs
```

```
// lib.mjs [注意后缀是 mjs]
export function add(x, y) {
    return x + y;
}

// main.mjs [注意后缀是 mjs]
import { add } from './lib.mjs';
console.log('Result: '+add(2, 3));
```

运行结果：

```
$ node main.mjs --experimental-modules  # 注意加 --experimental-modules
Result: 5
```

目前来说，使用起来还是比较麻烦的。那么 ES6 模块什么时候可以无需命令行选项就能使用？目前的计划是在

**Node.js 10 LTS**

中默认支持 ES6 模块。当然，大家的热情肯定不只如此，也可能会在 v10.0.0 之前就可以稳定支持了

