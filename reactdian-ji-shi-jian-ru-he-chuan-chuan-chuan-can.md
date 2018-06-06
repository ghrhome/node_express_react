# react点击事件如何传传传参 {#questionTitle}

> 建议使用es6箭头函数

```
<button onClick={(ev, arg1, arg2,……) => {this.handleClick(ev, arg1, arg2,……)}}/> 

handleClick(ev, arg1, arg,……) {
    //code
}
```

```
<button onClick={this.handleClick.bind(this, props0, props1, ...}></button>

handleClick(porps0, props1, ..., event) {
    // your code here
}
```



