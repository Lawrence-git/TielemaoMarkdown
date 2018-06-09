Node.JS和npm入门
===

[[toc]]

文：铁乐猫

# 引子

2018年6月1日的儿童节，我在北京学习python。
课程来到前端的javascript 和 jquery，今天老师还给科普了node.js的包管理npm。
虽然以前工作时己有安装和使用npm，但毕竟只知其然不知其所以然。
直到今天听老师科普了一下才恍然大悟它是怎样用法的。
后面还参考引用了一些网上的npm教程，更加深刻理解了。

# 何为node.js 和 npm

>Node.js是一个基于Chrome JavaScript运行时建立的平台， 用于方便地搭建响应速度快、易于扩展的网络应用。Node.js 使用`事件驱动`， `非阻塞I/O `模型而得以轻量和高效，非常适合在分布式设备上运行数据密集型的实时应用。

以上摘自百度百科，简单来说，Node.js很适合搭建轻量的服务器（应用），所以它又被人称为服务器语言，前端中的后端语言。

node翻译过来是节点的意思，而node.js后面特地带了.js，就表示它与JavaScript有莫大的关系。
node.js是javascript的一种运行环境，是服务器端的javascript的解释器。

![](https://www.tielemao.com/wp-content/uploads/2018/06/Nodejs.jpg)

npm则是包含在node.js里面的一个包管理工具，就如同linux中的yum仓库，rpm包管理；如同python中的pip包管理工具一样。

而这些包管理工具都是予以使用的人们方便，同时解决各种包依赖之间的关系的。
等下面演示后，就会知道有npm去解决项目及包之间的依赖关系是多么的便利，省去了人手上的多少心力。让开发人员专注于代码上。

既然npm是包管理工具，那么它自己也和node.js分开自成一个网站，在npm的网站上面，就如同github，其仓库中保管了N多的开源项目，有世界上众多开发者提供的项目。我们只需要在npm的网站上搜索相关的就可以找到，然后在线上下载也行，直接在自己的项目中使用命令行安装也行。

npm 由三个独立的部分组成：

*   npm官方网站（仓库源）
*   注册表（registry）（package.json)
*   命令行工具 (CLI)

[网站](https://npmjs.com) 是开发者查找包（package）、设置参数以及管理 npm 使用体验的主要途径。

_注册表_ 是一个巨大的数据库，保存了每个包（package）的信息。

[_CLI_](https://docs.npmjs.com/cli/npm) 通过命令行或终端运行。开发者通过 CLI 与 npm 打交道。

![npm](https://www.tielemao.com/wp-content/uploads/2018/06/npm.jpg)

# 怎么使用node.js和npm

## 安装node.js & npm

官方下载安装包链接： https://nodejs.org/en/download/
windows下安装msi方式的安装包，很简单，基本就是一路点击鼠标确认下来的，所以就不详说了。
这里放上一个详细截图说明安装的链接地址：
https://www.runoob.com/nodejs/nodejs-install-setup.html

其实安装完node.js后就已经将npm也安装上了。不过若是想要单独更新npm时怎么办？
可以使用以下两个命令进行更新：
`npm install npm@latest -g`
`npm install npm@next -g`

## npm init 初始化项目(创建node.js模块）

`npm init` 在项目中引导创建一个package.json文件。
安装包的信息可保持到项目的package.json文件中，以便后续的其它的项目开发或者他人合作使用，package.json在项目中是必不可少的。

语法：
`npm init [-f|--force|-y|--yes]`

在本地创建一个项目文件夹，例如我这次的例子目录是GhostScoolScreatBase。
cmd中进入到该目录下。
开始初始化项目：`npm init`
和git的初始化仓库是不是很像。
这样初始化之后，项目目录下会自动生成一个`package.json`文件。

注意的是，`npm init`命令后，npm会询问你一系列问题，当你填入答案后才会正式结束初始化，如果不太想自定义一些关于项目的描述，可以不敲`npm init`，而是直接敲`npm init --yes`

命令行中将会提示 `package.json` 字段中需要你输入的值。
`名称（name）` 和 `版本（version）` 这两个字段是必填的。
你还需要输入 `入口文件字段（main）` 字段，默认值是 `index.js`。

在index.js文件中，添加一个函数，作为 `exports`对象的一个属性。这样，require 此文件之后，这个函数在其他代码中就可以使用了。
```
exports.printMsg = function() {
  console.log("This is a message from the demo package");
}
```
如图：
![](https://www.tielemao.com/wp-content/uploads/2018/06/npm_init.jpg)

如果想为作者（author）字段添加信息，可以使用以下格式（邮箱、网址都是选填的）：

```
Your Name <email@example.com> (http://example.com)
```
打开生成的package.json文件，可以看到类似如下的：
```json
{
  "name": "ghostschoolscreatbase",
  "version": "1.0.0",
  "description": "铁血丹心-剧场经典大戏-幽灵学院第六部2018夏季特别篇-你和我的秘密基地",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "铁乐猫",
  "license": "ISC"
}
```
### Package.json 属性说明

*   **name** - 包名。

*   **version** - 包的版本号。

*   **description** - 包的描述。

*   **homepage** - 包的官网 url 。

*   **author** - 包的作者姓名。

*   **contributors** - 包的其他贡献者姓名。

*   **dependencies** - 依赖包列表。如果依赖包没有安装，npm 会自动将依赖包安装在 node_module 目录下。

*   **repository** - 包代码存放的地方的类型，可以是 git 或 svn，git 可在 Github 上。

*   **main** - main 字段指定了程序的主入口文件，require('moduleName') 就会加载这个文件。这个字段的默认值是模块根目录下面的 index.js。

*   **keywords** - 关键字

## npm install 下载安装依赖包

前面有提到初始化项目，可视为创建node.js模块的时候，会生成package.json文件。

而我们的项目显然会在途中用上很多模块，这些模块是不便全部上传到github仓库供用户下载的（github有限制仓库大小不能超过100M）。且用户还需自己手动安装这些依赖包也容易出错。

为此，npm提供了很大的便利，比如这次的项目，我们会用上jquery。
命令行进入项目目录；
执行`npm install jquery --save`

```
npm install <package_name>  # 安装一个本地包,如果加上-g选项则是安装全局包
```
>有两种方式用来安装 npm 包：本地安装和全局安装。至于选择哪种方式来安装，取决于我们如何使用这个包。

>*   如果你自己的模块依赖于某个包，并通过 Node.js 的 `require` 加载，那么你应该选择本地安装，这种方式也是 `npm install` 命令的默认行为。
>*   如果你想将包作为一个命令行工具，（比如 grunt CLI），那么你应该选择[全局安装](https://www.npmjs.com.cn/getting-started/installing-npm-packages-globally)。

回到实例，这时项目下自动会生成一个node_modules，并且node_modules文件下有jquery包，另外在package.json文件中也会有当前记录：
```
"dependencies": {
    "jquery": "^3.
}
```
>安装好之后，express 包就放在了工程目录下的 node_modules 目录中，因此在代码中只需要通过 **require('express')** 的方式就好，无需指定第三方包路径。
```
var express =  require('express');
```
同理，你在npm中下载别人发布的项目或模块后，也需要`npm install`来安装好依赖以便运行。

### 本地安装和全局安装区别
* 本地安装
	*   1\. 将安装包放在 ./node_modules 下（运行 npm 命令时所在的目录），如果没有 node_modules 目录，会在当前执行 npm 命令的目录下生成 node_modules 目录。
	*   2\. 可以通过 require() 来引入本地安装的包。

* 全局安装
	*   1\. 将安装包放在 /usr/local 下或者你 node 的安装目录。
	*   2\. 可以直接在命令行里使用。

如果你希望具备两者功能，则需要在两个地方安装它或使用 **npm link**。

## 补充-模块操作

### 卸载模块

我们可以使用以下命令来卸载 Node.js 模块。
```
$ npm uninstall express
```
卸载后，你可以到 /node_modules/ 目录下查看包是否还存在，或者使用以下命令查看：
```
$ npm ls
```
* * *

### 更新模块

我们可以使用以下命令更新模块：
```
$ npm update express
```
* * *

### 搜索模块

使用以下来搜索模块：
```
$ npm search express
```
### 创建模块和发布模块

创建模块使用的命令和步骤在前面`npm init`中己提到。

使用以下命令在 npm 资源库中注册用户（使用邮箱注册）：
```
$ npm adduser Username: tielemao
Password:  
Email:  (this IS public)tieleyumao@gmail.com
```
用以下命令来发布模块：
```
$ npm publish
```
如果你以上的步骤都操作正确，你就可以跟其他模块一样使用 npm 来安装自己发布的模块。

## 版本号

使用NPM下载和发布代码时都会接触到版本号。NPM使用语义版本号来管理代码，这里简单介绍一下。

语义版本号分为X.Y.Z三位，分别代表主版本号、次版本号和补丁版本号。当代码变更时，版本号按以下原则更新。

*   如果只是修复bug，需要更新Z位。
*   如果是新增了功能，但是向下兼容，需要更新Y位。
*   如果有大变动，向下不兼容，需要更新X位。

版本号有了这个保证后，在申明第三方包依赖时，除了可依赖于一个固定版本号外，还可依赖于某个范围的版本号。例如"argv": "0.0.x"表示依赖于0.0.x系列的最新版argv。

NPM支持的所有版本号范围指定方式可以查看[官方文档](https://npmjs.org/doc/files/package.json.html#dependencies)。

## 使用淘宝 NPM 镜像

大家都知道国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。

淘宝 NPM 镜像是一个完整 npmjs.org 镜像，可以用此代替官方版本(只读)，同步频率目前为 10分钟一次，以保证尽量与官方服务同步。

使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```
这样就可以使用 cnpm 命令来安装模块了：
```
$ cnpm install [name]
```
更多信息可以查阅：[http://npm.taobao.org/](http://npm.taobao.org/)。

参考和引用：
[npm使用介绍|菜鸟教程](https://www.runoob.com/nodejs/nodejs-npm.html)
[npm中文文档|npm中文网](https://www.npmjs.com.cn/)
