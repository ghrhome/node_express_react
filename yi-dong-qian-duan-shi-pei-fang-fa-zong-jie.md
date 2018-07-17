# 移动前端适配方法总结

原文链接：

[http://caibaojian.com/mobile-responsive.html](http://caibaojian.com/mobile-responsive.html)

所谓前端适配，就是为了让移动设计稿在大部分的移动设备上看起来有一致的展示效果，目前比较流行的方法有两种。各有利弊，使用第一种在某些浏览器的webview里面会出现兼容问题，而且对于1像素会无法渲染。而用[rem](http://caibaojian.com/t/rem)的方案在背景和字体上也会有某些问题。[·](http://caibaojian.com/mobile-responsive.html)

**方案一：强制meta viewport的宽度为设计稿的宽度**

把下面的代码放在头部，然后制作稿跟PC上一样的制作就行

```
// 根据设计稿的宽度来传参 比如640 750 1125
!function(designWidth){
    if (/Android(?:\s+|\/)(\d+\.\d+)?/.test(navigator.userAgent)) {
        var version = parseFloat(RegExp.$1);
        if (version > 2.3) {
            var phoneScale = parseInt(window.screen.width) / designWidth;
            document.write('<meta name="viewport" content="width=' + designWidth + ',minimum-scale=' + phoneScale + ',maximum-scale=' + phoneScale + ', target-densitydpi=device-dpi">');
        } else {
            document.write('<meta name="viewport" content="width=' + designWidth + ',target-densitydpi=device-dpi">');
        }
    } else {
        document.write('<meta name="viewport" content="width=' + designWidth + ',user-scalable=no,target-densitydpi=device-dpi,minimal-ui,viewport-fit=cover">');
    }
}(640);
```

demo

```
<!doctype HTML>
<html>
<head>
<meta http-equiv="content-type" content="text/html; charset=utf-8"/>
<meta content="telephone=no" name="format-detection" />
<title>model</title>
<script type="text/javascript">
// 根据设计稿的宽度来传参 比如640 750 1125
!function(e){if(/Android(?:\s+|\/)(\d+\.\d+)?/.test(navigator.userAgent)){var t=parseFloat(RegExp.$1);if(t>2.3){var i=parseInt(window.screen.width)/e;document.write('<meta name="viewport" content="width='+e+",minimum-scale="+i+",maximum-scale="+i+', target-densitydpi=device-dpi">')}else{document.write('<meta name="viewport" content="width='+e+',target-densitydpi=device-dpi">')}}else{document.write('<meta name="viewport" content="width='+e+',user-scalable=no,target-densitydpi=device-dpi,minimal-ui,viewport-fit=cover">')}}(640);
</script>
<style>
body,dl,dd,ul,ol,h1,h2,h3,h4,h5,h6,pre,form,input,textarea,p,hr,thead,tbody,tfoot,th,td{margin:0;padding:0;}
ul,ol{list-style:none;}
a{text-decoration:none;}
html{-ms-text-size-adjust:none;-webkit-text-size-adjust:none;text-size-adjust:none;font-size:32px;}
body{font-size:32px;line-height:1.5;}
body,button,input,select,textarea{font-family:'helvetica neue',tahoma,'hiragino sans gb',stheiti,'wenquanyi micro hei',5FAE8F6F96C59ED1,5B8B4F53,sans-serif;}
b,strong{font-weight:bold;}
i,em{font-style:normal;}
table{border-collapse:collapse;border-spacing:0;}
table th,table td{border:1px solid #ddd;padding:5px;}
table th{font-weight:inherit;border-bottom-width:2px;border-bottom-color:#ccc;}
img{border:0 none;width:auto9;max-width:100%;vertical-align:top;}
button,input,select,textarea{font-family:inherit;font-size:100%;margin:0;vertical-align:baseline;}
button,html input[type="button"],input[type="reset"],input[type="submit"]{-webkit-appearance:button;cursor:pointer;}
button[disabled],input[disabled]{cursor:default;}
input[type="checkbox"],input[type="radio"]{box-sizing:border-box;padding:0;}
input[type="search"]{-webkit-appearance:textfield;-moz-box-sizing:content-box;-webkit-box-sizing:content-box;box-sizing:content-box;}
input[type="search"]::-webkit-search-decoration{-webkit-appearance:none;}
@media screen and (-webkit-min-device-pixel-ratio:0){
input{line-height:normal!important;}
}
select[size],select[multiple],select[size][multiple]{border:1px solid #AAA;padding:0;}
article,aside,details,figcaption,figure,footer,header,hgroup,main,nav,section,summary{display:block;}
audio,canvas,video,progress{display:inline-block;}
</style>
</head>
<body>
    <!-- 页面结构写在这里 -->
    <!-- 页面结构写在这里 -->
    <!-- 页面结构写在这里 -->
    <!-- 页面结构写在这里 -->
</body>
</html>
```

原文链接：

[http://caibaojian.com/mobile-responsive.html](http://caibaojian.com/mobile-responsive.html)

**方案2：使用淘宝的**[**rem**](http://caibaojian.com/t/rem)**精简版**[·](http://caibaojian.com/mobile-responsive.html)

计算尺寸时，只需要将对应的尺寸除以100。

源码：

```
function(designWidth, maxWidth) {
    var doc = document,
        win = window;
    var docEl = doc.documentElement;
    var metaEl,
        metaElCon;
    var styleText,
        remStyle = document.createElement("style");
    var tid;

    function refreshRem() {
        // var width = parseInt(window.screen.width); // uc有bug
        var width = docEl.getBoundingClientRect().width;
        if (!maxWidth) {
            maxWidth = 540;
        };
        if (width > maxWidth) { // 淘宝做法：限制在540的屏幕下，这样100%就跟10rem不一样了
            width = maxWidth;
        }
        var rem = width * 100 / designWidth;
        // var rem = width / 10; // 如果要兼容vw的话分成10份 淘宝做法
        //docEl.style.fontSize = rem + "px"; //旧的做法，在uc浏览器下面会有切换横竖屏时定义了font-size的标签不起作用的bug
        remStyle.innerHTML = 'html{font-size:' + rem + 'px;}';
    }

    // 设置 viewport ，有的话修改 没有的话设置
    metaEl = doc.querySelector('meta[name="viewport"]');
    // 20171219修改：增加 viewport-fit=cover ，用于适配iphoneX
    metaElCon = "width=device-width,initial-scale=1,maximum-scale=1.0,user-scalable=no,viewport-fit=cover";
    if(metaEl) {
        metaEl.setAttribute("content", metaElCon);
    }else{
        metaEl = doc.createElement("meta");
        metaEl.setAttribute("name", "viewport");
        metaEl.setAttribute("content", metaElCon);
        if (docEl.firstElementChild) {
            docEl.firstElementChild.appendChild(metaEl);
        }else{
            var wrap = doc.createElement("div");
            wrap.appendChild(metaEl);
            doc.write(wrap.innerHTML);
            wrap = null;
        }
    }

    //要等 wiewport 设置好后才能执行 refreshRem，不然 refreshRem 会执行2次；
    refreshRem();

    if (docEl.firstElementChild) {
        docEl.firstElementChild.appendChild(remStyle);
    } else {
        var wrap = doc.createElement("div");
        wrap.appendChild(remStyle);
        doc.write(wrap.innerHTML);
        wrap = null;
    }

    win.addEventListener("resize", function() {
        clearTimeout(tid); //防止执行两次
        tid = setTimeout(refreshRem, 300);
    }, false);

    win.addEventListener("pageshow", function(e) {
        if (e.persisted) { // 浏览器后退的时候重新计算
            clearTimeout(tid);
            tid = setTimeout(refreshRem, 300);
        }
    }, false);

    if (doc.readyState === "complete") {
        doc.body.style.fontSize = "16px";
    } else {
        doc.addEventListener("DOMContentLoaded", function(e) {
            doc.body.style.fontSize = "16px";
        }, false);
    }
})(750, 750);
```

demo

```
<!doctype HTML>
<html>
<head>
<meta http-equiv="content-type" content="text/html; charset=utf-8"/>
<meta content="telephone=no" name="format-detection" />
<meta name="viewport" content="width=device-width,initial-scale=1,maximum-scale=1.0,user-scalable=no,viewport-fit=cover"/>
<title>lib.flexible 简化版</title>
<script type="text/javascript">
(function(e,t){var i=document,n=window;var l=i.documentElement;var r,a;var d,o=document.createElement("style");var s;function m(){var i=l.getBoundingClientRect().width;if(!t){t=540}if(i>t){i=t}var n=i*100/e;o.innerHTML="html{font-size:"+n+"px;}"}r=i.querySelector('meta[name="viewport"]');a="width=device-width,initial-scale=1,maximum-scale=1.0,user-scalable=no,viewport-fit=cover";if(r){r.setAttribute("content",a)}else{r=i.createElement("meta");r.setAttribute("name","viewport");r.setAttribute("content",a);if(l.firstElementChild){l.firstElementChild.appendChild(r)}else{var c=i.createElement("div");c.appendChild(r);i.write(c.innerHTML);c=null}}m();if(l.firstElementChild){l.firstElementChild.appendChild(o)}else{var c=i.createElement("div");c.appendChild(o);i.write(c.innerHTML);c=null}n.addEventListener("resize",function(){clearTimeout(s);s=setTimeout(m,300)},false);n.addEventListener("pageshow",function(e){if(e.persisted){clearTimeout(s);s=setTimeout(m,300)}},false);if(i.readyState==="complete"){i.body.style.fontSize="16px"}else{i.addEventListener("DOMContentLoaded",function(e){i.body.style.fontSize="16px"},false)}})(750,750);
</script>
<style>
/*
需要注意的是：
样式的reset中需先设置html字体的初始化大小为50px，这是为了防止js被禁用或者加载不到或者执行错误而做的兼容
样式的reset中需先设置body字体的初始化大小为16px，这是为了让body内的字体大小不继承父级html元素的50px，而用系统默认的16px
*/
body,dl,dd,ul,ol,h1,h2,h3,h4,h5,h6,pre,form,input,textarea,p,hr,thead,tbody,tfoot,th,td{margin:0;padding:0;}
ul,ol{list-style:none;}
a{text-decoration:none;}
html{-ms-text-size-adjust:none;-webkit-text-size-adjust:none;text-size-adjust:none;font-size:50px;}
body{line-height:1.5;font-size:16px;}
body,button,input,select,textarea{font-family:'helvetica neue',tahoma,'hiragino sans gb',stheiti,'wenquanyi micro hei',\5FAE\8F6F\96C5\9ED1,\5B8B\4F53,sans-serif;}
b,strong{font-weight:bold;}
i,em{font-style:normal;}
table{border-collapse:collapse;border-spacing:0;}
table th,table td{border:1px solid #ddd;padding:5px;}
table th{font-weight:inherit;border-bottom-width:2px;border-bottom-color:#ccc;}
img{border:0 none;width:auto\9;max-width:100%;vertical-align:top;}
button,input,select,textarea{font-family:inherit;font-size:100%;margin:0;vertical-align:baseline;}
button,html input[type="button"],input[type="reset"],input[type="submit"]{-webkit-appearance:button;cursor:pointer;}
button[disabled],input[disabled]{cursor:default;}
input[type="checkbox"],input[type="radio"]{box-sizing:border-box;padding:0;}
input[type="search"]{-webkit-appearance:textfield;-moz-box-sizing:content-box;-webkit-box-sizing:content-box;box-sizing:content-box;}
input[type="search"]::-webkit-search-decoration{-webkit-appearance:none;}
@media screen and (-webkit-min-device-pixel-ratio:0){
input{line-height:normal!important;}
}
select[size],select[multiple],select[size][multiple]{border:1px solid #AAA;padding:0;}
article,aside,details,figcaption,figure,footer,header,hgroup,main,nav,section,summary{display:block;}
audio,canvas,video,progress{display:inline-block;}


.g-doc{width:6.4rem;margin:0px auto;}

</style>
</head>
<body>
    <div class="g-doc">
        <!-- 页面结构写在这里 -->
        <!-- 页面结构写在这里 -->
        <!-- 页面结构写在这里 -->
        <!-- 页面结构写在这里 -->
    </div>
</body>
</html>
```



