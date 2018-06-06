[[toc]]

# 什么是JavaScript？


欢迎来到 MDN JavaScript 初学者的课程！ 在第一篇文章中，我们将会站在一定的高度来俯看 JavaScript，回答一些像“它是什么？”和“它能做什么？”的问题 。并确保你熟悉 JavaScript 的用途。

## 一个高水平的定义

JavaScript 是允许你在网页中实现复杂事情的一门编程语言 —— 每次当你浏览网页时不只是显示静态信息—— 显示即时更新的内容， 或者交互式的地图，或 2D/3D 图形动画，又或者自动播放视频等，你可以确信，JavaScript 参与其中。这是 Web技术的三层蛋糕标准:

![](https://mdn.mozillademos.org/files/13502/cake.png)

*   [HTML](https://developer.mozilla.org/en-US/docs/Glossary/HTML "HTML: HTML (HyperText Markup Language) is a descriptive language that specifies webpage structure.")是一种标记语言，用来结构化我们的网页内容和赋予内容含义，例如定义段落、标题、和数据表,或在页面中嵌入图片和视频。
*   [CSS](https://developer.mozilla.org/en-US/docs/Glossary/CSS "CSS: CSS (Cascading Style Sheets) is a declarative language that controls how webpages look in the browser.") 是一种样式规则语言，我们将样式应用于我们的 HTML 内容， 例如设置背景颜色和字体，在多个列种布局我们的内容。
*   [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript "JavaScript: JavaScript (JS) is a programming language mostly used to dynamically script webpages on the client side, but it is also often utilized on the server-side, using packages such as Node.js.") 是一种编程语言，允许你创建动态更新的内容，控制多媒体，图像动画，和一些其他的东西。好吧，虽然不是一切，但是它的神奇之处是你能够用几行JavaScript代码就能实现。

这三个层次规矩地建立在彼此之上。让我们用一个简单的文本标签作为例子。我们可以用 HTML 来标记它，以赋予它结构和目的：

```html
<p>Player 1: Chris</p>
```

![](https://mdn.mozillademos.org/files/13422/just-html.png)

然后我们可以加上一点 CSS 来使它看起来更好：

```css
p {
  font-family: 'helvetica neue', helvetica, sans-serif;
  letter-spacing: 1px;
  text-transform: uppercase;
  text-align: center;
  border: 2px solid rgba(0,0,200,0.6);
  background: rgba(0,0,200,0.3);
  color: rgba(0,0,200,0.6);
  box-shadow: 1px 1px 2px rgba(0,0,200,0.4);
  border-radius: 10px;
  padding: 3px 10px;
  display: inline-block;
  cursor:pointer;
}
```

![](https://mdn.mozillademos.org/files/13424/html-and-css.png)

而最后，我们可以加上一些 JavaScript 来实现动态行为：

```js
var para = document.querySelector('p');

para.addEventListener('click', updateName);

function updateName() {
  var name = prompt('Enter a new name');
  para.textContent = 'Player 1: ' + name;
}
```

尝试点击文本标签，观察会发生什么（同时注意，你可以在 GitHub 上找到这个演示—— [源代码](https://github.com/mdn/learning-area/blob/master/javascript/introduction-to-js-1/what-is-js/javascript-label.html)，或者 [实时运行](http://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/javascript-label.html)）！

![js-player]($res/js-player.gif)

JavaScript 可以做比这更多的东西——让我们详细探索它可以做些什么。

## 所以它_实际上_可以做什么？

JavaScript 语言的核心包含一些普遍的编程特点，以让你可以做到如下的事情：

*   在变量中储存有用的值。以上面的演示做例子，我们请求输入一个新的名字，然后把那个名字储存到一个叫 `name` 的变量.
*   对一段文本（在编程中被称为“字符串”）进行操作。在上面的例子中，我们取出字符串 "Player 1: "，然后把它和 name 变量连结起来，创造出完整的文本标签，例：''Player 1: Chris"。
*   运行代码以响应在网页中发生的特定事件。在上述的例子中，我们用了一个 `click` 事件来检测按钮什么时候被点击，然后运行更新文本标签的代码。
*   以及更多！

然而更令人兴奋的是建立在 JavaScript 语言的核心之上的功能。在你的 JavaScript 代码里，被称为**应用程序编程接口** [Application Programming Interfaces  (APIs) ]  的功能会提供额外的超能力给你使用。

APIs 是已经建立好的一套代码组件，目的是让开发者可以实现除此之外很难甚至不可能实现的程序。它们的作用就像是已经制作好的家具套件对家居建设的作用一样——从一堆已经切好的木板开始组装一个书柜，显然比自己设计，寻找合适的木材，裁切至合适的大小和形状，找到合适大小的螺丝钉，然后组装成一个书柜要简单得多。

它们 (APIs) 通常分成两个分类。

![](https://mdn.mozillademos.org/files/13508/browser.png)

**浏览器 APIs (Browser APIs)** 已经安装在你的网页浏览器中，而且能够从周围的计算机环境中揭露数据，或者做有用的复杂事情。举个例子：

*   文档对象模型 API [[DOM (Document Object Model) API](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model)] 允许你操作 HTML 和 CSS，创建，移除和修改 HTML，动态地应用新的样式到你的页面，等等。比如说每次你在一个页面里看到一个弹出窗口，或者显示一些新的内容（像我们在上面的简单演示中看到那样），这就是 DOM 在运作。

*   地理定位 API [[Geolocation API](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation)] 获取地理信息。这就是为什么谷歌地图 [[Google Maps](https://www.google.com/maps)] 可以找到你的位置，而且标示在地图上。

*   画布 [[Canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)] 和 [WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API) APIs 允许你创建生动的 2D 和 3D 图像。人们正运用这些网页技术进行一些令人惊叹的事情——比如说 [Chrome Experiments](https://www.chromeexperiments.com/webgl) 和 [webglsamples](http://webglsamples.org/)。

*   音像和影像 APIs [[Audio and Video APIs](https://developer.mozilla.org/en-US/Apps/Fundamentals/Audio_and_video_delivery)]，像 [`HTMLMediaElement`](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLMediaElement "从父级元素 HTML 元素继承属性") 和 [WebRTC](https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API) 允许你运用多媒体去做一些非常有趣的事情，比如在网页中播放音像和影像，或者从你的网页摄像头中获取获取录像，然后在其他人的电脑上展示（尝试我们的简单快照演示 [[Snapshot demo](http://chrisdavidmills.github.io/snapshot/)] 以理解这个概念）。

**Note**: 上述的很多演示都不能在旧的浏览器中运行——当进行实验时，在现代浏览器，像 Firefox, Chrome, Edge 或者 Opera，中运行会是一个好的想法。当你接近交付产品代码时，你会需要更深入地去考虑跨平台测试 [[cross browser testing](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing)]（例：现实客户会使用的实际代码）。

**第三方 APIs (Third party APIs)** 默认是没有安装到浏览器中的，而你通常需要从网络上的某些地方取得它们的代码和信息。举个例子：

*   推特 API [[Twitter API](https://dev.twitter.com/overview/documentation)] 允许你做一些像是在你的网站上展示你的最新推送之类的事情。

*   谷歌地图 API [[Google Maps API](https://developers.google.com/maps/)] 允许你去嵌入定制的地图到你的网站，和其他的功能。

**Note**: 这些 APIs 是高级的，而我们不会在课程中涉及任何的这些 APIs，但是如果你想了解更多，上述的链接提供延展的文档供参考。

这里还有更多可用的东西！然而，不要这么快就感到太过兴奋。你不可能只通过 24 小时的 JavaScript 学习，就能够构建下一个 Facebook, Google Maps 或者 Instagram——这里有很多的基础需要优先覆盖。而这就是为什么你在这里——让我们继续前进！

## JavaScript 在你的页面上做什么？

在这我们会开始确实地查看一些代码，而在这样做的同时，探索当你在你的页面上运行 JavaScript 的时候实际发生了什么。

让我们简单地回顾当你在浏览器中读取一个网页时发生什么（在文章 [How CSS works](https://developer.mozilla.org/en-US/Learn/CSS/Introduction_to_CSS/How_CSS_works#How_does_CSS_actually_work) 中第一次谈及到）。 当你在浏览器中读取一个网页，你在一个实行环境（浏览器标签）中运行你的代码（HTML, CSS 和 JavaScript）。这就像是一个工厂，获取原材料（代码）然后出产一个产品（网页）。

![](https://mdn.mozillademos.org/files/13504/execution.png)

在 HTML 和 CSS 已经被集合和组装成一个网页后，浏览器的 JavaScript 引擎执行 JavaScript。这保证了当 JavaScript 开始运行时，网页的结构和样式已经在该出现的地方了。

这是一个好事情，正如 JavaScript 的普遍用处是通过 DOM API（如之前提及的那样）动态地修改 HTML 和 CSS 来更新用户交界面。如果 JavaScript 在 HTML 和 CSS 加载完成之前加载运行，那么会发生错误。

### 浏览器安全

每个浏览器标签本身就是一个用来运行代码的分离的容器（这些容器用专业术语称为“运行环境”）——这意味着在大多数情况中，每个标签中的代码是完全分离地运行，而且在一个标签中的代码不能直接影响在另一个标签中的代码——或者在另一个网站中的。这是一个好的安全措施——如果不是这样的话，那么海盗们就可以开始写从其他网站偷取信息的代码，和其他像这样的坏事。

**Note**: 这里有安全的方式去在不同网站/标签中传送代码和数据，但这些方法是高级的技术，而我们不会在这门课里覆盖这些。

### JavaScript 运行顺序

当浏览器遇到一块 JavaScript 代码时，它通常会按顺序运行这代码块，从上往下。这意味着你需要注意你放置代码的顺序。举个例子，让我们回到我们在第一个例子中看到的 JavaScript 代码块：

```js
var para = document.querySelector('p');

para.addEventListener('click', updateName);

function updateName() {
  var name = prompt('Enter a new name');
  para.textContent = 'Player 1: ' + name;
}
```

在这里我们正选定一个文本段落 (line 1)，然后给它附上一个事件监听器 (line 3) 使得当这个段落被点击时，`updateName()` 代码块 (lines 5–8) 会被运行。`updateName()` 代码块（这类可以重复使用的代码块被称为“函数”）向用户请求一个新的名字，然后把这个名字插入到段落中以更新显示。

如果你互换了代码里最初两行的顺序，它将不会工作——取而代之的是，你会在浏览器的开发者控制台中得到一个错误——TypeError: para is undefined [类型错误：para没有被定义]。这意味着 para 对象还不存在，所以我们不能为它增添一个事件监听器。

**Note**: 这是一个很常见的错误——你需要注意在尝试对你的代码中引用的对象进行操作前，它已经存在。

### 解释代码 vs 编译代码

在编程环境中，你或许听说过这两个术语 **解释 [interpreted]** 和 **编译 [compiled]**。JavaScript 是一个解释语言——代码从上到下运行，而运行的结果会马上被返回。在浏览器运行代码前，你不必先把它转化为其他形式。

另一方面来说，编译语言则需要在运行前转化为另一种形式。比如说 C/C++ 则要先被编译成汇编语言，然后再由电脑运行。

两种方式都有不同的优势，然而就目前而言，我们不会谈论这些。

### 服务器端代码 vs 客户端代码

你或许也听说过  **服务器端 [server-side]** 和 **客户端 [client-side]**   代码这两个术语，尤其是在网页开发的语境中。客户端代码是在用户的电脑上运行的代码——当浏览一个网页时，这个网页的客户端代码就会被下载，然后由浏览器来运行和展示。在这个 JavaScript 模块，我们将会明确地探讨 **客户端 JavaScript [client-side JavaScript]**。

在另一方面，服务器端代码则在服务器上运行，然后它的结果会由浏览器进行下载和展示。流行的服务器端网页语言包含以下几个例子：PHP, Python, Ruby, ASP.NET 和 JavaScript！JavaScript 同时也能用作服务器端语言，比如说在流行的 Node.js 环境中——你可以在我们的 [动态网页 - 服务器端编程 [Dynamic Websites – Server-side programming]](https://developer.mozilla.org/en-US/docs/Learn/Server-side) 主题中找到更多关于服务器端 JavaScript 的知识。

**动态 [dynamic]** 这个词被用来描述客户端 JavaScript 和服务器端语言——它指的是能更新网页/应用的内容以在不同环境下显示不同事物，当有需要时产生新内容的能力。服务器端代码会动态地在服务器上产生新的内容，比如说从数据库中提取信息。反之，客户端 JavaScript则在用户的浏览器中动态地生成新的内容，比如说创建一个新的 HTML 表格，从中插入从服务器请求到的数据，然后在已经向用户展示了的网页中显示这个表格。在这两个语境中，动态的意义有细微的不同，但是有联系，而且两种方法（服务器端和客户端）通常是在一起工作的。

一个没有动态更新内容的网页被指作 **静态 [static]** ——它只会一直显示一样的内容。

## 怎样向你的页面添加 JavaScript？

JavaScript 以一种近似于 CSS 的方式应用到你的 HTML 页面中。尽管 CSS 使用 `<link>` 元素去应用外部的样式表 [stylesheet] 和 `<style>`元素去应用内部的样式表到 HTML，JavaScript 只需要在 HTML 世界里的一个元素—— `<script>` 元素。让我们学习一下它怎么工作。

### 内部的 JavaScript

1.  首先，复制我们的范例文件 [apply-javascript.html](https://github.com/mdn/learning-area/blob/master/javascript/introduction-to-js-1/what-is-js/apply-javascript.html) 到本地。储存到一个可以察觉的目录中。

```html
<!DOCTYPE html>
<html lang="en-US">
  <head>
    <meta charset="utf-8">
    <title>Apply JavaScript example</title>
  </head>
  <body>
    <button>Click me</button>
  </body>
</html>
```
2.  在你的浏览器和文本编辑器中打开这个文件。你会看到这个 HTML 创建了一个包含了一个可点击按钮的简单网页。

3.  `然后，在你的文本编辑器里，在你的结束 </body>` 标签前接上以下代码：

    ```html
    <script>
      // JavaScript goes here
    </script>
    ```

4.  现在我们会在我们的 `<script>`元素中加上一些 JavaScript 来让这个页面做一些更有趣的东西——在 "// JavaScript goes here" 这一行下面加上以下代码：

    ```js
    function createParagraph() {
      var para = document.createElement('p');
      para.textContent = 'You clicked the button!';
      document.body.appendChild(para);
    }
    var buttons = document.querySelectorAll('button')
    for(var i = 0; i < buttons.length ; i++) {
      buttons[i].addEventListener('click', createParagraph);
    }
    ```

5.  保存你的文件并刷新你的浏览器——现在当你点击按钮时，你应当会看到一个新的段落产生并在下方显示。

**Note**: 如果你的例子看上去不能工作，再检查所有的步骤和保证你都做对了。你有把原始代码作为 `.html` 文件保存为本地复件吗？你有刚好在`</body> `标签前加上 `<script>`元素吗？你有确切地输入所示的 JavaScript ？ 

**JavaScript 是区分大小写的，而且非常的讲究，所以你需要精确地输入所示的句法，不然它可能会无法工作.**

**Note**: 你可以在 GitHub 上看到这个版本 [apply-javascript-internal.html](https://github.com/mdn/learning-area/blob/master/javascript/introduction-to-js-1/what-is-js/apply-javascript-internal.html) ([see it live too](http://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/apply-javascript-internal.html)).

### 外部的 JavaScript

这方法很不错，但要是我们想要把我们的 JavaScript 放置在一个外部文件中呢？现在让我们探索这个。

1.  首先，在跟你的简单 HTML 文件的同一目录下创建一个新的文件。命名为 `script.js` ——保证它以 .js 为文件扩展名，因为这是它被认作是 JavaScript 的方式。

2.  然后，把所有在你现在的 `<script>`元素中的脚本 [script] 提取出来并粘贴到 .js 文件。保存这个文件。

3.  现在替换你的 `<script>`元素为如下：

    ```html
    <script src="script.js"></script>
    ```

4.  保存然后刷新你的浏览器，然后你应该看到同样的东西！它工作起来是一样的，但是现在我们把 JavaScript 写进了一个外部文件。对于规划你的代码来说，这通常是一件好事，而且让它可以在多个 HTML 文件中重复使用。再加上 HTML 中没有一大堆脚本的话，HTML 会更容易阅读。

**Note**: 你可以在 GitHub 上看到这个版本 as [apply-javascript-external.html](https://github.com/mdn/learning-area/blob/master/javascript/introduction-to-js-1/what-is-js/apply-javascript-external.html) and [script.js](https://github.com/mdn/learning-area/blob/master/javascript/introduction-to-js-1/what-is-js/script.js) ([see it live too](http://mdn.github.io/learning-area/javascript/introduction-to-js-1/what-is-js/apply-javascript-external.html)).

### 内联 JavaScript 处理器

注意，有时候你会遇到在 HTML 中存在着一丝真实的 JavaScript 代码。它或许看上去会像这样：

```js
function createParagraph() {
  var para = document.createElement('p');
  para.textContent = 'You clicked the button!';
  document.body.appendChild(para);
}
```

```html
<button onclick="createParagraph()">Click me!</button>
```

这个演示有着跟前两节的演示一模一样的功能，除了 [`<button>`](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/button "HTML <button> 元素表示一个可点击的按钮，可以用在表单或文档其它需要使用简单标准按钮的地方。") 元素中包含了一个内联的 `onclick` 处理器以至于函数会在按钮被按下时运行。

**然而请不要这样做。**  这是一个用 JavaScript 来污染你的 HTML 的坏实践，而且它还不高效——你会需要在每个想要 JavaScript 应用到的按钮上包含 `onclick="createParagraph()"` 属性。

使用一个纯 JavaScript 结构允许你使用一个指令来选取所有的按钮。我们在上面实现这一目的的代码看上去是这样的：

```html
var buttons = document.querySelectorAll('button');

for(var i = 0; i < buttons.length ; i++) {
  buttons[i].addEventListener('click', createParagraph);
}
```

这或许看上去比 `onclick` 属性要长一些，但是这会应用于所有的按钮，无论页面上有多少个，和有多少个按钮被添加或者移除。不需要对 JavaScript 进行任何修改。

**Note**: 尝试编辑你自己的 `apply-javascript.html` 版本并在文件中加上更多的按钮。当你重新加载时，你应该会发现所有的按钮被按下时都会创建一个段落。很简洁，不是吗？

## 注释

正如使用 HTML 和 CSS 一样，在你的 JavaScript 代码中书写会被浏览器忽略掉的注释是可行的，并且注释只用来为你的开发者同事提供关于代码如何工作的指引（包括你，如果你在 6 个月后回到你的代码并忘记了你做过些什么）。注释非常有用，而且你应该经常使用它们，尤其是在更大的应用程序中。这里有两类注释：

*   一个单行注释书写在一个双正斜杠后 (//)，比如：

    ```js
    // I am a comment
    ```

*   一个多行注释书写在字符串 /* 和 */ 之间， 比如：

    ```js
    /*
      I am also
      a comment
    */
    ```

所以举例说，我们可以用 “注释” 来为我们上一个演示的 JavaScript 注释：

```js
// Function: creates a new paragraph and append it to the bottom of the HTML body.

function createParagraph() {
  var para = document.createElement('p');
  para.textContent = 'You clicked the button!';
  document.body.appendChild(para);
}

/*
  1\. Get references to all the buttons on the page and soter them in an array.
  2\. Loop through all the buttons and add a click event listener to each one.

  When any button is pressed, the createParagraph() function will be run.
*/

var buttons = document.querySelectorAll('button');

for(var i = 0; i < buttons.length ; i++) {
  buttons[i].addEventListener('click', createParagraph);
}
```

## 总结

所以你到这里了，你在 JavaScript 世界中的第一步。我们仅仅从理论开始，让你熟悉为什么你会使用 JavaScript，和你可以用它做什么事情。在这过程中你看到了一些代码示例并且学习到了 JavaScript 是如何与你网站中的其他代码适配的。

JavaScript 现在或许看上去有一点令人畏惧，但不用担心——在这门课中我们会逐步地引领你。在下一篇文章我们会[全心投入到实践](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Introduction_to_JavaScript_1/A_first_splash)，让你专注其中并建立你自己的 JavaScript 例子。

【end】

