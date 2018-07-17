# 移动前端自适应适配布局解决方案和比较

原文链接：

[http://caibaojian.com/mobile-responsive-example.html](http://caibaojian.com/mobile-responsive-example.html)

互联网上的[自适应](http://caibaojian.com/t/自适应)方案到底有几种呢？就我个人实践所知，有这么几种方案：[·](http://caibaojian.com/mobile-responsive-example.html)

1. 固定一个某些宽度，使用一个模式，加上少许的媒体查询方案
2. 使用flexbox解决方案
3. 使用百分比加媒体查询
4. 使用
   [rem](http://caibaojian.com/t/rem)

淘宝最近开源的一个框架和网易的框架有同工之异。都是采用[rem](http://caibaojian.com/t/rem)实现一稿解决所有设置[自适应](http://caibaojian.com/t/自适应)。在没出来这种方案之前，第一种做法的人数也不少。类似以下说到的拉钩网。看一下流云诸葛的文章。

以下摘自：[从网易与淘宝的font-size思考前端设计稿与工作流](http://www.cnblogs.com/lyzg/p/4877277.html)

## 1. 简单问题简单解决 {#t1}

我觉得有些[web](http://caibaojian.com/c/web) app并一定很复杂，比如拉勾网，你看看它的页面在iphone4,iphone6,ipad下的样子就知道了：

不同点[·](http://caibaojian.com/mobile-responsive-example.html)

* * 淘宝的设计稿是基于750的横向分辨率，网易的设计稿是基于640的横向分辨率，还要强调的是，虽然设计稿不同，但是最终的结果是一致的，设计稿的尺寸一个公司设计人员的工作标准，每个公司不一样而已
  * 淘宝还需要动态设置viewport的scale，网易不用
  * **最重要的区别就是：网易的做法，rem值很好计算，淘宝的做法肯定得用计算器才能用好了**
     。不过要是你使用了less和sass这样的css处理器，就好办多了，以淘宝跟less举例，我们可以这样编写less：

```
//定义一个变量和一个mixin
@baseFontSize: 75;//基于视觉稿横屏尺寸/100得出的基准font-size
.px2rem(@name, @px){
    @{name}: @px / @baseFontSize * 1rem;
}

//使用示例：

.container {
    .px2rem(height, 240);
}
//less翻译结果：
.container {
    height: 3.2rem;
}
```



