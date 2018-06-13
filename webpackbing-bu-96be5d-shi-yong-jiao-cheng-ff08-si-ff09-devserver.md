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

* 配置了_publicPath_后，`url= '主机名' + 'publicPath配置的' +`
  `'原来的url.path'`。这个其实与_output.publicPath_用法大同小异。
* _output.publicPath_是作用于_js, css, img_。而_devServer.publicPath_则作用于请求路径上的。

```
// devServer.publicPath
publicPath: "/assets/"

// 原本路径 --> 变换后的路径
http://localhost:8080/app.js --> http://localhost:8080/assets/app.js
```

### proxy \([文档](https://webpack.js.org/configuration/dev-server/#devserver-proxy)\) {#articleHeader11}

* 当您有一个单独的API后端开发服务器，并且想要在同一个域上发送API请求时，则代理这些
  _url_。看例子好理解。

```
proxy: {
    '/proxy': {
        target: 'http://your_api_server.com',
        changeOrigin: true,
        pathRewrite: {
            '^/proxy': ''
        }
  }

```



1. 假设你主机名为_localhost:8080_, 请求_API_的_url_是_http：//your\_api\_server.com/user/list_
2. _**'/proxy'**_：如果点击某个按钮，触发请求_API_事件，这时请求_url_是`http：//localhost:8080`**`/proxy`**`/user/list`。
3. _**changeOrigin**_：如果_true_，那么`http：//localhost:8080/proxy/user/listhttp：//your_api_server.com/proxy/user/list`
   。但还不是我们要_url_。

4. _**pathRewrite**_：重写路径。匹配_/proxy_，然后变为`''`，那么_url_最终为`http：//your_api_server.com/user/list`。

### watchOptions （[文档](https://webpack.js.org/configuration/dev-server/#devserver-watchoptions-)） {#articleHeader12}

* 一组自定义的监听模式，用来监听文件是否被改动过。

```
watchOptions: {
  aggregateTimeout: 300,
  poll: 1000，
  ignored: /node_modules/
}
```



1. **aggregateTimeout**：一旦第一个文件改变，在重建之前添加一个延迟。填以毫秒为单位的数字。
2. **ignored**：观察许多文件系统会导致大量的CPU或内存使用量。可以排除一个巨大的文件夹。
3. **poll**：填以毫秒为单位的数字。每隔（你设定的）多少时间查一下有没有文件改动过。不想启用也可以填`false`。

---

## ~~完结，希望大家喜欢！~~并未完结，敬请期待！ {#articleHeader13}















