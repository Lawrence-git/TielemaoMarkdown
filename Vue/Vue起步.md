# Vue起步

## 声明式渲染

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：

例，HTML页面如下：
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue-app</title>
</head>
<body>
    <div id="app">
        {{ message }}
    </div>
</body>
    <script src="vue.js"></script>
    <script>
        var app = new Vue({
            el: "#app",
            data: {
                message: "Hello Hero!"
            }
        })
    </script>
</html>
```
这里主要看div中采用了模板语法，在id为app的div标签中，放入了message的变量。
而在js部份，var声明一个变量，new一个Vue实例对象，里面就放了data数据为message的键值:
```js
var app = new Vue({
    el: "#app",
    data: {
        message: "Hello Hero!"
    }
})
```
看起来这跟渲染一个字符串模板非常类似，不过Vue 在背后做了大量工作。现在数据和 DOM 已经被建立了关联，所有东西都是**响应式的**。怎么确认呢？打开你的浏览器的 JavaScript 控制台 ，并修改 `app.message` 的值，将会看到上例相应地更新。

![vue-message]($resource/vue-message.gif)

除了文本插值，还可以像这样来绑定元素特性：
HTML部分代码：
```html
<div id="app-2">
        <span v-bind:title = "message">
            鼠标悬停几秒钟查看此处动态绑定的提示信息！
        </span>
    </div>
```
*  `v-bind` 特性被称为**指令**。
   * 指令带有前缀 `v-`，以表示它们是 Vue 提供的特殊特性。
   * 它们会在渲染的 DOM 上应用特殊的响应式行为。

在这里，该指令的意思是：“将这个元素节点的 `title` 特性和 Vue 实例的 `message` 属性保持一致”。

JS部分：
```javascript
var app2 = new Vue({
    el: "#app-2",
    data: {
        message: "英雄现身于" + new Date().toLocaleString()
    }
})
```
效果：
![vue-message2]($resource/vue-message2.gif)

## 条件与循环

控制切换一个元素是否显示（使用if ture）：
```html
<div id="app">
    <p v-if="see">现在，你看到我了</p>
</div>
```
JS部分：
```js
var app;
app = new Vue({
    el: "#app",
    data: {
        see: true
  }
});
```
效果
![vue-if]($resource/vue-if.gif)

在控制台输入 `app.see = false`，你会发现之前显示的消息消失了。

这个例子演示了不仅可以把数据绑定到 DOM 文本或特性，还可以绑定到 DOM **结构**。
此外，Vue 也提供一个强大的过渡效果系统，可以在 Vue 插入/更新/移除元素时自动应用[过渡效果](https://cn.vuejs.org/v2/guide/transitions.html)。

还有其它很多指令，每个都有特殊的功能。例如，`v-for` 指令可以绑定数组的数据来渲染一个项目列表：
```html
<div id="app-2">
        <ol>
            <li v-for="todo in todos">
                {{ todo.text }}
            </li>
        </ol>
    </div>
```
JS部分
```js
var app2 = new Vue({
            el: "#app-2",
            data:{
                todos:[
                    { text: "你一定能成为一个英雄！" },
                    { text: "首先每天800米跑50圈"},
                    { text: "接着俯卧撑500个"},
                    { text: "最后你就秃了，然后就变强了！"}
                ]
            }
        })
```
效果：
![vue-todos]($resource/vue-todos.gif)

## 处理用户输入

为了让用户和你的应用进行交互，我们可以用 `v-on` 指令添加一个事件监听器，
通过它调用在 Vue 实例中定义的方法：

```html
<div id="app">
    <p>{{ message }}</p>
    <button v-on:click="reverseMessage">逆转裁判</button>
</div>
```

js部分：
```js
var app = new Vue({
    el: "#app",
    data: {
        message: "Hello Hero"
    },
    methods: {
        reverseMessage: function () {
                this.message = this.message.split('').reverse().join('')
                }
        }
    })
```
效果：
![vue-reverse]($resource/vue-reverse.gif)

注意在 `reverseMessage` 方法中，我们更新了应用的状态，但没有触碰 DOM——所有的 DOM 操作都由 Vue 来处理，你编写的代码只需要关注逻辑层面即可。

Vue 还提供了 `v-model` 指令，它能轻松实现表单输入和应用状态之间的双向绑定。
```html
<div id="app-2">
        <p> {{ message2 }}</p>
        <input v-model="message3">
    </div>
```

JS部分：
```javascript
var app2 = new Vue({
    el: "#app-2",
    data: {
        message2: "Hello Hero!",
        message3: "一拳超人"
    }
})
```
![vue-model]($resource/vue-model.jpg)

## 组件化应用构建

组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用。
仔细想想，几乎任意类型的应用界面都可以抽象为一个组件树：

![vue-components]($resource/vue-components.jpg)

在 Vue 里，一个组件本质上是一个拥有预定义选项的一个 Vue 实例。
在 Vue 中注册组件很简单：

Html中创建组件模板：
```html
  <ol>
      <!-- 创建一个 todo-item 组件的实例 -->
      <todo-item></todo-item>
  </ol>
```
JS部分：
```javascript
// 定义名为 todo-item 的新组件
    Vue.component('todo-item', {
        template: "<li>待办项：后补英雄绿谷骚年</li>"
    })
```
但这样会为每个待办项渲染同样的文本，这看起来并不炫酷。我们应该能从父作用域将数据传到子组件才对。
让我们来修改一下组件的定义，使之能够接受一个 `prop`：

