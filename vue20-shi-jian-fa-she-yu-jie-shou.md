```
Vue2.0 事件发射与接收
```

由于vue2.0 移除了1.0中的$dispatch 和$broadcast 这两个组件之间通信传递数据的方法 ,官方的给出的最简单的升级建议是使用集中的事件处理器,而且也明确说明了 一个空的vue实例就可以做到,因为Vue 实例实现了一个事件分发接口.

请直接看代码,在初始化web app的时候，给data添加一个 名字为eventhub 的空vue对象

```
new Vue({
  el: '#app',
  router,
  render: h => h(App),
  data: {
    eventHub: new Vue()
  }
})
```

好的 这个时候 你就可以一劳永逸了，在任何组件都可以调用事件发射 接受的方法了.

如何获取到这个空的vue对象 eventhub呢.在组件里面直接调用这个

某一个组件内调用事件触发

```
//通过this.$root.eventHub获取此对象
//调用$emit 方法
this.$root.eventHub.$emit('YOUR_EVENT_NAME', yourData)
```

另一个组件内调用事件接受,当然在组件销毁时接触绑定,使用$off方法

```
this.$root.eventHub.$on('YOUR_EVENT_NAME', (yourData)=>{
    handle(yourData)
} )
```

遇到一个问题 ，考虑特定场景：

跳转路由之前我们调用了$emit方法,这个方法在A组件里面处理数据，但是A组件绑定$on事件之前 $emit事件已经发射，所以这会导致一直接受不到消息，看来这个事件绑定有时效性问题，你可以setTimeout来做一下延时，但是这个特别奇怪，那就把数据存到store然后等A组件加载完了再去取。。。。

---

record on 12 24

在stackoverflow 发现一个更加简洁的方法,因为本质上vue是一个js对象，我们想保存一个全局对象，只需要在Vue的prototype上面增加一个属性即可,本质上所有Vue组件都是继承全局的Vue。只要在初始化Vue对象之前给原生Vue对象prototype增加属性，那样所有的组件（因为都是继承自它的实例）都可以访问到这个属性。相关资料请参考我之前的文章[关于函数的构造函数和prototype&lt;四&gt;](http://blog.csdn.net/a5534789/article/details/40918955)

在初始化Web app 之前 加上这样一句：

```
Vue.prototype.$eventHub= Vue.prototype.$eventHub ||  new Vue()
```

当然我们可以定义其他的全局变量.比如当前app的系统配置文件，名字为sysconfig.json,你可以这样定义

```
Vue.prototype.$config =Vue.prototype.$config||require('path/sysconfig.json')
```

这样我们在组件内部 就可以直接调用

$eventHub 和

$config对象了。

比如 在mounted函数里面直接 console.log\($config.yourKey\)

---

刚才看到了webpack的插件里面有一个[definePlugin](https://webpack.github.io/docs/list-of-plugins.html#defineplugin)它可以帮我们定义全局的常量。

如何使用，很简单但是更好，我们不用去修改Vue对象：

```
new webpack.DefinePlugin({
  CONFIG: require('path/sysconfig.json')
});
```

然后我们也可以在全局内使用CONFIG对象了。





