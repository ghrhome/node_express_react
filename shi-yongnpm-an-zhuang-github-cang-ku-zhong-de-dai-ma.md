# 使用npm安装github仓库中的代码

我们在使用npm下载包的时候，一般是下载在[npm官网](https://www.npmjs.com/)发布过的。可以指定版本，指定依赖等等。

但是，对于一个团队或公司，需要从自己的工作账号拉取代码，npm是直接支持从git仓库安装的。

----------------------

npm是nodejs的官方包管理，有成千上万的包，方便了前端模块化开发。但有些前端库并没有发布到npm,但有时候项目又需要它。本文介绍通过npm如何安装github仓库代码。从而达到模块化开发的目的：



## 安装方法

## 初始化 {#初始化npm-init}

```
npm init
```

```
//安装依赖 npm install
npm install <git remote url>

<protocol>://[<user>[:<password>]@]<hostname>[:<port>][:][/]<path>[#<commit-ish>]
<protocol> is one of git, git+ssh, git+http, git+https, or git+file. If no <commit-ish> is specified, then master is used
```

示例:

```
$ npm install git://git@github.com:huixisheng/zepto-lazyload.git
$ npm install https://github.com/huixisheng/zepto-lazyload.git
```

`package.json`配置

```
"dependencies": {
  "zepto-lazyload": "git://git@github.com:huixisheng/zepto-lazyload.git"
},
```

定义包的版本

```
"dependencies": {
  "package1": "1.0.0",         // 只接受1.0.0的版本
  "package2": "1.0.x",         // 接受1.0版本的bug修复或小更新
  "package3": "*",             // 最新的版本，不推荐
  "package4": ">=1.0.0",       // 接受任何1.0.0版本后的更新
  "package5": "<1.9.0",        // 接受不超过1.9.0版本的更新
  "package6": "~1.8.0",        // 接受 >= 1.8.0 并 < 1.9.0 的版本
  "package7": "^1.1.0",        // 接受 >=1.1.0 并 < 2.0.0 的版本
  "package8": "latest",        // tag name for latest version
  "package9": "",              // 同 * 即最新版本。
  "packageX": "<1.0.0 || >=2.3.1 <2.4.5 || >=2.5.2 <3.0.0"
}
```

## 命令说明 {#命令说明}

```
npm install gitlab:<gitlabname>/<gitlabrepo>[#<commit-ish>]
npm install bitbucket:<bitbucketname>/<bitbucketrepo>[#<commit-ish>]
npm install gist:[<githubname>/]<gistID>[#<commit-ish>]
npm install github:<githubname>/<githubrepo>[#<commit-ish>]
npm install <githubname>/<githubrepo>[#<commit-ish>]
npm install <git remote url>
npm install (with no args, in package dir)
npm install [<@scope>/]<name>
npm install [<@scope>/]<name>@<tag>
npm install [<@scope>/]<name>@<version>
npm install [<@scope>/]<name>@<version range>
npm install <tarball file>
npm install <tarball url>
npm install <folder>
```

------------------问题

最近遇到的一个问题，只能从我自己fork的[vue2-scrollbar](https://github.com/ohhoney1/vue2-scrollbar)项目安装。虽然已有人PR，但是作者不更新了。

安装方法

![](https://images2015.cnblogs.com/blog/997049/201705/997049-20170516134951682-1317173695.png)

或

![](https://images2015.cnblogs.com/blog/997049/201705/997049-20170516135135541-1956834693.png)

此时，package.json文件中，会出现对应的项目依赖，后面的版本号会替换成git地址

![](https://images2015.cnblogs.com/blog/997049/201705/997049-20170516135327400-1548582192.png)

这样，只要这个json文件不变，就能正确的安装这个组件了。









































