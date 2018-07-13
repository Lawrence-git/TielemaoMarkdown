[[toc]]

## Express介绍

npm提供了大量的第三方模块，其中不乏许多Web框架，比如轻量级的Web框架 ——— Express。

Express是一个简洁、灵活的node.js Web应用开发框架, 它提供一系列强大的功能，比如：模板解析、静态文件服务、中间件、路由控制等等,并且还可以使用插件或整合其他模块来帮助你创建各种 Web和移动设备应用,是基于Node.js的Web开发框架，并且支持Ejs、jade等多种模板，可以快速地搭建一个具有完整功能的网站。

## 安装express

```
npm install -g express 
```

## 安装express命令工具

```
npm install -g express-generator
```

## 查看express命令

```
express -h
```
![](https://www.tielemao.com/wp-content/uploads/2018/06/express-h.jpg)


## 创建初始项目

创建默认初始化项目是，其视图使用的是jade模板引擎

```
express 项目名
```

创建ejs模板引擎项目

```
express -e 项目名
```

## 下载相关的模组

进入项目目录，执行下面的命令

```
npm install
```

## 项目结构说明

![](https://www.tielemao.com/wp-content/uploads/2018/06/web_dir.jpg)
1\. 【bin】存放启动项目的脚本文件
2\. 【node_modules】 存放所有的项目依赖库
3\. 【public】静态文件(css,js,img)
4\. 【routes】路由文件(MVC中的C,controller)
5\. 【views】页面文件(Ejs模板)
6\. 【app.js】是项目的入口，设置项目的模板引擎类型，报错页面，端口设置等
7\. 【package.json】项目依赖配置及开发者信息

## 启动

项目目录下执行

```
npm start
```

![](https://www.tielemao.com/wp-content/uploads/2018/06/npm_start.jpg)

## 查看

默认查看地址：[http://127.0.0.1:3000/]

![](https://www.tielemao.com/wp-content/uploads/2018/06/express-3000.jpg)

## 获取、引用

```
var express = require('express');
var app = express();
```

通过变量“app”我们就可以调用express的各种方法了。
