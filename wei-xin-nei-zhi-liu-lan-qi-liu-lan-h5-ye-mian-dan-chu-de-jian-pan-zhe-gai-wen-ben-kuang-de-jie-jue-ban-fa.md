# 微信内置浏览器浏览H5页面弹出的键盘遮盖文本框的解决办法

```
 if(/Android [4-6]/.test(navigator.appVersion)) {
2     window.addEventListener("resize", function() {
3         if(document.activeElement.tagName=="INPUT" || document.activeElement.tagName=="TEXTAREA") {
4             window.setTimeout(function() {
5                 document.activeElement.scrollIntoViewIfNeeded();
6             },0);
7         }
8     })
9 }
```

Android微信内置浏览H5页面，因为其中的文本输入框（input）放置在靠近页面的中下方，点击文本框以后，则输入框会被弹出的手机输入法键盘遮盖住。

找到一段js代码直接解决之，点击时强制滚动之，好像也解决了在Android浏览器下浏览的同样问题。

