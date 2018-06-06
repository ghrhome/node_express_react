# vue踩坑之全局使用axios

> Vue 原本有一个官方推荐的 ajax 插件 vue-resource，但是自从 Vue 更新到 2.0 之后，尤雨溪宣布停止更新vue-resource，并推荐大家使用axios之后，越来越多的 Vue 项目，都选择 axios 来完成 ajax 请求，而大型项目会使用 Vuex 来管理数据

之前一直使用的是 vue-resource插件，在主入口文件引入`import VueResource from 'vue-resource'`之后，直接使用`Vue.use(VueResource)`之后即可将该插件全局引用了；

初用axios时，无脑的按照上面的步骤进行全局引用，结果当时是惨惨的。  
看了看文档，Axios 是一个基于 promise 的 HTTP 库

> axios并没有install 方法，所以是不能使用vue.use\(\)方法的。
>
> 那么难道每个文件都要来引用一次？解决方法有很多种：
>
> **1.结合 vue-axios使用**
>
> **2. axios 改写为 Vue 的原型属性**
>
> **3.结合 Vuex的action**

## 结合 vue-axios使用 {#结合-vue-axios使用}

看了vue-axios的源码，它是按照vue插件的方式去写的。那么结合vue-axios，就可以去使用vue.use方法了

首先在主入口文件main.js中引用

```
mport axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios,axios);
```

之后就可以使用了，在组件文件中的methods里去使用了

```
getNewsList(){
      this.axios.get('api/getNewsList').then((response)=>{
        this.newsList=response.data.data;
      }).catch((response)=>{
        console.log(response);
      })


    },
```

## axios 改写为 Vue 的原型属性 {#axios-改写为-vue-的原型属性}

首先在主入口文件main.js中引用，之后挂在vue的原型链上

```
import axios from 'axios'
Vue.prototype.$ajax= axios
```

在组件中使用

```
this.$ajax.get('api/getNewsList').then((response)=>{
        this.newsList=response.data.data;
      }).catch((response)=>{
        console.log(response);
      })
```

结合 Vuex的action

在vuex的仓库文件store.js中引用，使用action添加方法

```
import Vue from 'Vue'
import Vuex from 'vuex'

import axios from 'axios'

Vue.use(Vuex)
const store = new Vuex.Store({
  // 定义状态
  state: {
    user: {
      name: 'xiaoming'
    }
  },
  actions: {
    // 封装一个 ajax 方法
    login (context) {
      axios({
        method: 'post',
        url: '/user',
        data: context.state.user
      })
    }
  }
})

export default store
```

在组件中发送请求的时候，需要使用 this.$store.dispatch

```
methods: {
  submitForm () {
    this.$store.dispatch('login')
  }
}
```



















