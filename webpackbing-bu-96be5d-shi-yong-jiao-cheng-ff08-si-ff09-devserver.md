# \[Webpack并不难\]使用教程（四）--- devServer {#articleTitle}

> [使用教程（一）--- entry，devtool，output，resolve](https://segmentfault.com/a/1190000012334562)  
> [使用教程（二）--- module.loaders](https://segmentfault.com/a/1190000012351195)  
> [使用教程（三）--- plugins](https://segmentfault.com/a/1190000012367082)
>
> ##### **我的**_**Webpack**_**版本是**_**3.10.0**_

## DevServer （[官方的文档](https://webpack.js.org/configuration/dev-server/#devserver-hot)） {#articleHeader0}

* 在开发模式下，_**DevServer**_提供虚拟服务器，让我们进行开发和调试。
* 而且提供实时重新加载。简直美滋滋。大大减少开发时间。
* 它不是_webpack_内置插件哦，要安装！！！

```
// 安装
npm install webpack-dev-server --save-dev

// 在 package.json 配置下，方便使用。
"scripts": {
    "dev": "webpack-dev-server" // 运行命令会自动在node_modules文件夹找 webapck-dev-server模块。
 }

// webpack.config.js 配置一下 devServer
devServer: {
    clientLogLevel: 'warning',
    historyApiFallback: true,
    hot: true,
    compress: true,
    host: 'localhost',
    port: 8080
  }
```

如果没在

1. _package.json_
   配置且还是局部安装，你就要在命令行输入
   `node_modules/.bin/webpack-dev-server`
   。若你
   _package.json_
   配置好了，在命令行输入
   `npm run dev`
   。
2. 下面说说
   _devServer_
   配置中每一项有什么用。

### Hot （[文档](https://webpack.js.org/configuration/dev-server/#devserver-hot)） {#articleHeader1}

* 热模块更新作用。即修改或模块后，保存会自动更新，页面不用刷新呈现最新的效果。
* 这不是和_webpack.HotModuleReplacementPlugin （HMR）_这个插件不是一样功能吗？是的，不过请注意了，
  `HMR这个插件是真正实现热模块更新的`。而_devServer_里配置了_hot: true_,
  _webpack_会自动添加_HMR_插件。所以模块热更新最终还是_HMR_这个插件起的作用。

### host （[文档](https://webpack.js.org/configuration/dev-server/#devserver-host)） {#articleHeader2}

* 写主机名的。默认_localhost_

### prot （[文档](https://webpack.js.org/configuration/dev-server/#devserver-port)） {#articleHeader3}

* 端口号。默认_8080_

### historyApiFallback （[文档](https://github.com/bripkens/connect-history-api-fallback)） {#articleHeader4}

* 如果为_**true**_，页面出错不会弹出_404_页面。
* 如果为_{...}_, 看看一般里面有什么。

  * **rewrites**

  ```
  rewrites: [
      { from: /^\/subpage/, to: '/views/subpage.html' },
      {
        from: /^\/helloWorld\/.*$/,
        to: function() {
            return '/views/hello_world.html;
        }
      }
  ]
  // 从代码可以看出 url 匹配正则，匹配成功就到某个页面。
  // 并不建议将路由写在这，一般 historyApiFallback: true 就行了。
  ```

**verbose:**如果_true_，则激活日志记录。

* **disableDotRule**： 禁止_url_带小数点`.`。

### compress \([文档](https://webpack.js.org/configuration/dev-server/#devserver-compress)\) {#articleHeader5}

* 如果为_true_，开启虚拟服务器时，为你的代码进行压缩。加快开发流程和优化的作用。

### contentBase （[文档](https://webpack.js.org/configuration/dev-server/#devserver-contentbase)） {#articleHeader6}

* 你要提供哪里的内容给虚拟服务器用。这里最好填**绝对路径**。

### compress \([文档](https://webpack.js.org/configuration/dev-server/#devserver-compress)\) {#articleHeader5}

* 如果为_true_，开启虚拟服务器时，为你的代码进行压缩。加快开发流程和优化的作用。

### contentBase （[文档](https://webpack.js.org/configuration/dev-server/#devserver-contentbase)） {#articleHeader6}

* 你要提供哪里的内容给虚拟服务器用。这里最好填**绝对路径**。

```
// 单目录
contentBase: path.join(__dirname, "public")

// 多目录
contentBase: [path.join(__dirname, "public"), path.join(__dirname, "assets")]

```

默认情况下，它将使用您当前的工作目录来提供内容。

### Open （[文档](https://webpack.js.org/configuration/dev-server/#devserver-open)） {#articleHeader7}

* _true_，则自动打开浏览器。

### overlay \([文档](https://webpack.js.org/configuration/dev-server/#devserver-overlay)\) {#articleHeader8}

* 如果为_true_，在浏览器上全屏显示编译的errors或warnings。默认_false_（关闭）
* 如果你只想看_error_，不想看_warning_。

```
overlay：{
    errors：true，
    warnings：false
}
```

### quiet （[文档](https://webpack.js.org/configuration/dev-server/#devserver-quiet-)） {#articleHeader9}

* _true_，则终端输出的只有初始启动信息。
  _webpack_的警告和错误是不输出到终端的。

### publicPath （[文档](https://webpack.js.org/configuration/dev-server/#devserver-publicpath-)） {#articleHeader10}

* 配置了_publicPath_后，_`url`_`= '主机名' + '`_`publicPath`_`配置的' +`
  `'原来的`_`url.path`_`'`。这个其实与_output.publicPath_用法大同小异。
* _output.publicPath_是作用于_js, css, img_。而_devServer.publicPath_则作用于请求路径上的。















