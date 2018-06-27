CSP解决页面劫持

## 内容劫持原理

通过上图我们就分析出了，一般劫持是通过在页面加入引入一个JS文件，然后这个JS文件会执行各种逻辑，根据新引入的JS域名搜索，你就会发现很多东西了，

## 解决方案

知道了病状不代表就一定可以解决，例如我们公司的就很奇葩，因为广告的插入导致JS全部出错用户无法进行操作（太可\*，插入广告就算了，还导致别人无法正常运营）。通过搜索我发现了可以通过CSP解决这个问题

## CSP

### 定义

CSP 全称为 Content Security Policy，即内容安全策略。主要以白名单的形式配置可信任的内容来源，在网页中，能够使白名单中的内容正常执行（包含 JS，CSS，Image 等等），而非白名单的内容无法正常执行，从而减少跨站脚本攻击（XSS），当然，也能够减少运营商劫持的内容注入攻击。

### 使用方式一：Meta标签

在 HTML 的 Head 中添加如下 Meta 标签，将在符合 CSP 标准的浏览器中使非同源的 script 不被加载执行。不支持 CSP 的浏览器将自动会忽略 CSP 的信息，不会有什么影响。具体兼容性可在本文末尾参考资料中找到

```
<meta http-equiv="Content-Security-Policy" content="script-src 'self'">
```

### 使用方式二：Http 头部

```
Content-Security-Policy:
script-src 'unsafe-inline' 'unsafe-eval' 'self'  *.54php.cn *.yunetidc.com *.baidu.com *.cnzz.com  *.duoshuo.com *.jiathis.com;report-uri /error/csp
```



