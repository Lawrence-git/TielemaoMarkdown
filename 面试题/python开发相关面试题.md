# python开发相关面试题

## 为什么要做前后端分离？

前后端的分离也实现了前后端架构的分离，带来的好处是：
* 整个项目的开发权重往前移，实现真正的前后端解藕，动态资源和静态资源分离，提高了性能和扩展性。
* 前后端交给不同的人来编写，明确划分职责，发现bug的时候可以快速定位。
* 在大并发情况下，可以同时水平扩展前后端服务器。
* 减少后端服务器的并发压力，除了接口以外的其他所有http请求全部转移到前端服务器上。
* 即使后端服务暂时超时或者宕机了，前端页面也会正常访问，只不过数据刷不出来而已，当然现在一般是服务器集群，少有出现这种现象。
* 后端统一API接口，接口完全可以共用，提供给不同的前端（IOS,安卓,PC,微信小程序等）。只要通过一些代码重构，就可以大量复用接口，提升效率。
* 页面不再是全局刷新，而是异步加载，局部刷新，减轻压力。
* vue.js等框架编写前端时，会比之前写jquery更简单快捷。

## 谈谈你对restful规范的理解？

- restful：宁静的
- 对于后端人员，主要为前端提供API(接口）
- 以前的你的接口：
  - <http://127.0.0.1:8000/index>
  - <http://127.0.0.1:8000/users>
  - <http://127.0.0.1:8000/add_users/>
  - <http://127.0.0.1:8000/del_users/>
  - <http://127.0.0.1:8000/edit_users/>
- restful规范：
  - <https://127.0.0.1:8000/users/>
  - 1.使用https代替http
  - 2.在URL体现自己写的是API
    - <https://www.tielemao.com/api/>
    - <https://api.tielemao.com> 也可以，但可能会引发跨域问题
      - 浏览器有一个同源策略
  - 3.在URl中体现版本 
    - <https://www.tielemao.com/api/v1/users>
    - <https://www.tielemao.com/api/v2/users>
  - 4.在URL中一般使用名词（面向资源编程）
    - <https://www.tielemao.com/api/v1/song>
  - 5.根据行为（method方法不同）
    - get，获取
    - post，新建
    - put，更新
    - patch，局部更新
    - delete，删除
  - 6.条件
    - <https://www.tielemao.com/api/v2/users?page=1>
    - <https://www.tielemao.com/api/v2/users?page=1&gender=2>
  - 7.状态码
    - 简单的可以用状态码
    - 复杂的一般都会用code字段
    - 推荐用code,例支付宝API
  - 8.错误信息
    - 一旦有错误的时候，接口统一返回错误信息
    - 后端返回，前端只做展示就可以
    - 例：
    ```
    { code:10001,
        error:“用户名或密码错误“}
     ``` 
 - 9.返回结果
   - 根据用户的请求方式不同返回不同的结果
   - 视情况而定，前端不要就别返回
   - 例：get请求
   - https://www.tielemao.com/api/v1/users
   - 响应：
 - 10.hyper link (超链接）

## django rest framework框架的作用？
- 帮助开发者可以快速开发出遵循restful规范的API

## django rest framwork框架都有哪些组件？
- 版本
- 权限
- 认证
- 节流
- 分页
- 路由
- 视图      [ **** ]
- 渲染器
- 解析器   [ **** ]
- 序列化  ［***** 最重要］


