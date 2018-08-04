# npm安装vue

by 铁乐猫
@toc

## Vue.js 是什么

Vue (读音 /vjuː/，类似于 **view**) 是一套用于构建用户界面的**渐进式框架**。与其它大型框架不同的是，Vue 被设计为可以自底向上逐层应用。Vue 的核心库只关注视图层，不仅易于上手，还便于与第三方库或既有项目整合。另一方面，当与现代化的工具链以及各种支持类库结合使用时，Vue 也完全能够为复杂的单页应用提供驱动。

* 数据驱动视图
  * 数据驱动是vuejs最大的特点。
  * 在vuejs中，所谓的数据驱动就是**当数据发生变化的时候，用户界面发生相应的变化，开发者不需要手动的去修改dom**。

* 声明式渲染
  * Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统。
  
## 直接用script引入

直接下载并用 `<script>` 标签引入，`Vue` 会被注册为一个全局变量。
注：在开发环境下不要使用压缩版本，不然你就失去了所有常见错误相关的警告!

* [开发版本](https://vuejs.org/js/vue.js)包含完整的警告和调试模式

* [生产版本](https://vuejs.org/js/vue.min.js)删除了警告，30.90KB min+gzip

## 安装vue

在用 Vue 构建大型应用时推荐使用 NPM 安装。NPM 能很好地和诸如 [webpack](https://webpack.js.org/) 或 [Browserify](http://browserify.org/) 模块打包器配合使用。同时 Vue 也提供配套工具来开发[单文件组件](https://cn.vuejs.org/v2/guide/single-file-components.html)。

```bash
# 最新稳定版
$ npm install vue
```

## 对不同构建版本的解释

在 [NPM 包的 `dist/` 目录](https://cdn.jsdelivr.net/npm/vue/dist/)你将会找到很多不同的 Vue.js 构建版本。
这里列出了它们之间的差别：

|  | UMD | CommonJS | ES Module |
| --- | --- | --- | --- |
| **完整版** | vue.js | vue.common.js | vue.esm.js |
| **只包含运行时版** | vue.runtime.js | vue.runtime.common.js | vue.runtime.esm.js |
| **完整版 (生产环境)** | vue.min.js | - | - |
| **只包含运行时版 (生产环境)** | vue.runtime.min.js | - |  |

## 安装命令行工具 (CLI)

Vue 提供了一个[官方的 CLI](https://github.com/vuejs/vue-cli)，为单页面应用快速搭建 (SPA) 繁杂的脚手架。它为现代前端工作流提供了 batteries-included 的构建设置。只需要几分钟的时间就可以运行起来并带有热重载、保存时 lint 校验，以及生产环境可用的构建版本。更多详情可查阅 [Vue CLI 的文档](https://cli.vuejs.org)。

### 安装cnpm

在国内，使用淘宝的镜像会比较快安装一些包。
![cnpm-install]($resource/cnpm-install.jpg)
我前面己经安装好了node.js和npm，上图是进入命令行，npm安装cnpm：
```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```
这样就可以使用 cnpm 命令来安装模块了：
```
$ cnpm install [name]
```

### 安装vue-cli

`vue-cli`用于快速搭建大型单页应用,可创建并启动一个带热重载、保存时静态检查以及可用于生产环境的构建配置的项目。

`$ cnpm install --global vue-cli`
![cnpm-install-vue-cli]($resource/cnpm-install-vue-cli.jpg)
如上图，使用`--global`选项是因为这样可以在全局下使用vue-cli工具来创建vue项目，而不是要在特定的安装目录下才能使用vue-cli。

`$ vue -v`查看版本验证安装成功与否
![vue-v]($resource/vue-v.jpg)

因为vue命令去初始化项目的时候实际上还是使用的是npm去安装各种模块，并没有使用cnpm，所以还是先设置npm使用淘宝中的镜像比较快。
![npm-set-registry-taobao]($resource/npm-set-registry-taobao.jpg)

## 新建vue项目
新建一个项目文件夹，进入该文件夹后敲以下命令初始化一个vue项目
`vue init webpack 项目名称`

其中webpack为vue的其中一个模板。
查看帮助得知，如果自己的github仓库上 己有模板也可指定github上的仓库来初始化项目:
![vue-init-webpack-project]($resource/vue-init-webpack-project.jpg)

下图开始初始化一个vue项目，利用的就是vue-cli和webpack：
`vue init webpack my-project`
![vue-init-webpack-myproject]($resource/vue-init-webpack-myproject.jpg)

```
? Project name vue-start //项目名称
? Project description A Vue.js project // 项目描述
? Author  // 作者名称
? Vue build standalone // 推荐选前者

? Install vue-router?  
// 是否安装vue-router路由组件，也可不安装使用第三方或简单的项目自己写

? Use ESLint to lint your code? 
// 是否使用eslint管理代码，个人项目不推荐

? Set up unti tests?
// 是否使用karma来做单元测试

? Setup e2e tests with Nightwatch?
// 是否安装e2e测试

? Should we run 'npm install' for you after the project has been created?
// 选择使用npm或yarn进行安装模块
```

一路填写所需信息后，回车执行，一段时间安装完模块等后初始化完成。
![vue-init-project]($resource/vue-init-project.jpg)

没安装那几个模块，大小也去到100多M了，果然是要建立大型的项目时才去做vue-cli init 项目的事情比较好阿。平常的就直接使用vue.js好了。

### 运行服务
进入项目目录，按之前看到的提示，运行`npm run dev`命令进入开发：
![vue-run-dev]($resource/vue-run-dev.jpg)
默认监听8080端口，服务器己经启动，目前是在开发环境下。

访问默认的`localhost:8080`，出现的就是vue的欢迎页面如下，表示正常：
![vue]($resource/vue.jpg)

退出监听，直接关闭cmd窗口即可。

### 目录结构

> *   build -- 大部分是webpack的配置文件
> *   config -- 配置文件，比如配置监听端口
> *   node_modules -- 依赖包都在这里
> *   src -- 主工程文件夹，基本上所有的开发都在这个文件夹进行
> *   static -- 静态文件目录
> *   package.json -- 项目的一些配置信息

![vue_dir]($resource/vue_dir.jpg)

【end】