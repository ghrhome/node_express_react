# es6的Object.keys\(\)和map\(\)组合使用的案例

ES6语法，把对象转化为对象数组。

{k1:v1.k2:v2,k3:v3}转化成\[{key:k1,value:v1},{key:k2,value:v2},{key:k3,value:v3}\]的形式。

把对象的key取出来作为对象数组里对象的key的值，把对象的value取出来，作为对象数组伦理对象的value值。怎么做呢？代码如下：

```
const p={width:30,height:20,weight:60};
    const pArr= Object.keys(p).map(key=>({
        key,
        value:p[key]
    }));
```

> 这个pArr就是新的数组。



