# Angular6项目构建

安装Nodejs长期支持版本\(LTS\)

* **设置npm淘宝代理： npm config set registry**
  [**https://registry.npm.taobao.org**](https://registry.npm.taobao.org/)
* **npm install -g @angular/cli**
* **ng new projectName –sass**
* **ng serve –open**

项目打包

* ng build –prod –aot –build-optimizer –source-map=false
* 优点
* -1.预编译
* -2.代码压缩
* -3.去掉源程序映射，优化项目体积，打包后vender不到250kb,服务器启用gzip后，vender 大约85kb

## 添加组件 {#添加组件}

* ng g c componentName

## 添加服务 {#添加服务}

* ng g s serviceName

### 浏览器兼容处理 {#浏览器兼容处理}

把polyfills中针对IE兼容依赖引入，例如：

    /** IE9, IE10 and IE11 requires all of the following polyfills. **/
    import 'core-js/es6/symbol';
    import 'core-js/es6/object';
    import 'core-js/es6/function';
    import 'core-js/es6/parse-int';
    import 'core-js/es6/parse-float';
    import 'core-js/es6/number';
    import 'core-js/es6/math';
    import 'core-js/es6/string';
    import 'core-js/es6/date';
    import 'core-js/es6/array';
    import 'core-js/es6/regexp';
    import 'core-js/es6/map';
    import 'core-js/es6/weak-map';
    import 'core-js/es6/set';
    import 'core-js/es6/promise';

    /** IE10 and IE11 requires the following for NgClass support on SVG elements */
    // import 'classlist.js';  // Run `npm install --save classlist.js`.

    /** IE10 and IE11 requires the following for the Reflect API. */
    import 'core-js/es6/reflect';






