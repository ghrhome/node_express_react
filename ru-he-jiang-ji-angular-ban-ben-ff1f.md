## 如何降级Angular版本？

我最近安装了Angular 6并希望回到使用Angular 5.2如何从我选择的任何版本更改我的Angular版本？

答：项目中使用的RIAN版本是由RangeCli安装的版本决定的。任何特定版本的角CLI都可以通过以下命令安装：

`npm install --global @angular/cli@x.x.x`。

例如：

`npm install --global @angular/cli@1.6.6`

即使安装了另一个版本的角CLI\(新版本或旧版本\)，也不会引起问题。

**首先需要卸载，安装cli。**

```
npm uninstall -g angular-cli
npm cache clean
npm install -g angular-cli@1.6.1
```

**在此之后，删除node\_modules目录**

**然后将你的软件包版本更改`package.json`为具有以下版本的版本**

```
{
  ...
  },
  ...
  "dependencies": {
    "@angular/animations": "5.2.2",
    "@angular/cdk": "^5.2.2",
    "@angular/common": "5.2.2",
    "@angular/compiler": "5.2.2",
    "@angular/core": "5.2.2",
    "@angular/forms": "5.2.2",
    "@angular/http": "5.2.2",
    "@angular/material": "^5.2.2",
    "@angular/platform-browser": "5.2.2",
    "@angular/platform-browser-dynamic": "5.2.2",
    "@angular/router": "5.2.2",
    "@ngrx/core": "^1.2.0",
    "@ngrx/store": "^4.0.3",
    "core-js": "^2.5.1",
    "hammerjs": "^2.0.8",
    "rxjs": "^5.5.2",
    "typescript": "^2.4.2",
    "web-animations-js": "^2.3.1",
    "zone.js": "^0.8.18"
  },
  "devDependencies": {
    "@angular/cli": "1.6.1",
    "@angular/compiler-cli": "5.2.2",
    "@angular/language-service": "5.2.2",
    "@types/jasmine": "~2.5.53",
    "@types/jasminewd2": "~2.0.2",
    "@types/node": "~6.0.60",
    ...
  }
}

```

**并安装软件包**

```
npm install
```



