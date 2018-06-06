注：全文摘自[MDN-介绍CSS布局](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/Introduction)

[[toc]]

CSS页面布局技术允许我们拾取网页中的元素，并且控制它们相对正常布局流、周边元素、父容器或者主视口/窗口的位置。在这个模块中将涉及更多关于页面[布局技术](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Layout_mode)的细节：

*   浮动
*   定位
*   CSS 表格
*   弹性盒子
*   网格

每种技术都有它们的用途，各有优缺点。

## 正常布局流

正常布局流是指在不对页面进行任何布局控制时，浏览器默认的HTML布局方式。让我们快速地看一个HTML的例子：

```html
<p>I love my cat.</p>

<ul>
  <li>Buy cat food</li>
  <li>Exercise</li>
  <li>Cheer up friend</li>
</ul>

<p>The end!</p>
```

默认情况下，浏览器的显示如下:

![css-layout01]($res/css-layout01.jpg)

注意，HTML元素完全按照源码中出现的先后次序显示——第一个段落、无序列表、第二个段落。

布局技术会覆盖默认的布局行为：

*   `position`属性 — 正常布局流中，默认为 `static` ，使用其它值会引起元素不同的布局方式，例如将元素固定到浏览器视口的左上角。
*   浮动——应用 `float`值，诸如 `left` 能够让块级元素互相并排成一行，而不是一个堆叠在另一个上面。
*   `display` 属性——标准值 `block`, `inline` or `inline-block` 会改变元素在正常布局流中的行为方式(见 [Types of CSS boxes](https://developer.mozilla.org/en-US/docs/Learn/CSS/Introduction_to_CSS/Box_model#Types_of_CSS_boxes) )，而一些不常见或特殊的值允许我们使用完全不同的方式进行布局，使用工具比如Flexbox。

## 浮动

浮动技术允许元素浮动到另外一个元素的左侧或右侧，而不是默认的一个堆叠另一个。float 的主要用途是布置出多个列并且浮动文字以环绕图片。下面我们快速浏览一下 `float` 属性并通过一个例子来说明。

float 属性有四个可能的值：

*   `left` — 将元素浮动到左侧。
*   `right` — 将元素浮动到右侧。
*   `none` — 默认值, 不浮动。
*   inherit — 继承父元素的浮动属性。

### 简单HTML示例

下面展示了如何用浮动来创建一个简单的两列布局。首先看一下HTML：

```html
<h1>2 column layout example</h1>
<div>
  <h2>First column</h2>
  <p> Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla luctus aliquam dolor, eu lacinia lorem placerat vulputate. </p>
</div>

<div>
  <h2>Second column</h2>
  <p>Nam vulputate diam nec tempor bibendum. Donec luctus augue eget malesuada ultrices. Phasellus turpis est, posuere sit amet dapibus ut.</p>
</div>
```

代码中有一个一级标题和两个简单的 `<div>`元素。每个又各自包含一个二级标题和一个段落。默认情况下。HTML内容自上而下显示：

![css-layout02]($res/css-layout02.jpg)

### 整列浮动

下面让我们将两个  `<div>`元素排成一行。下面的代码可以实现这个效果. 注意这两个 `<div>`一个浮动值为 `left`，另外一个浮动为 `right`。这意味着它们其中一个往左靠，另外一个往右靠。给这两个元素分别设置 `width`值，使得它们能够在同一行放下来，并且设置一个水平的间距(总宽度不要大于100!).

```css
div:nth-of-type(1) {
  width: 48%;
  float: left;
}

div:nth-of-type(2) {
  width: 48%;
  float: right;
}
```
修改过的代码如下：

![css-layout03]($res/css-layout03.jpg)

## 定位技术

定位技术(position)允许我们将一个元素从它在页面的原始位置准确地移动到另外一个位置。

有四种主要的定位类型需要我们了解：

*   **静态定位(Static positioning)** 是每个元素默认的属性——它表示“将元素放在文档布局流的默认位置——没有什么特殊的地方”。
*   **相对定位(Relative positioning)** 允许我们相对元素在正常的文档流中的位置移动它——包括将两个元素叠放在页面上。这对于微调和精准设计(design pinpointing)非常有用。
*   **绝对定位(Absolute positioning)** 将元素完全从页面的正常布局流中移出，类似将它单独放在一个图层中. 我们可以将元素相对于页面的 `<html>` 元素边缘固定，或者相对于离元素最近的被定位的祖先元素(ancestor element)。绝对定位在创建复杂布局效果时非常有用，例如通过标签显示和隐藏的内容面板或者通过按钮控制滑动到屏幕中的信息面板.
*   **固定定位(Fixed positioning)** 与绝对定位非常类似，除了它是将一个元素相对浏览器视口固定，而不是相对另外一个元素。 在创建类似页面滚动总是处于页面上方的导航菜单时非常有用。

### 简单定位示例

我们将展示一些示例代码来熟悉这些布局技术. 这些示例代码都作用在相同的HTML上：

```html
<h1>Positioning</h1>

<p>I am a basic block level element.</p>
<p class="positioned">I am a basic block level element.</p>
<p>I am a basic block level element.</p>
```
该HTML将使用以下CSS默认样式：

```css
body {
  width: 500px;
  margin: 0 auto;
}

p {
  background: aqua;
  border: 3px solid blue;
  padding: 10px;
  margin: 10px;
}

span {
  background: red;
  border: 1px solid black;
}
```

渲染效果如下:

![css-layout04]($res/css-layout04.jpg)

### 相对定位

相对定位通常用来对布局进行微调，比如将一个图标往下调一点，以便放置文字. 我们可以通过下面的规则添加相对定位来实现效果: 

```html
.positioned {
  position: relative;
  background: yellow;
  top: 30px;
  left: 30px;
}
```

这里我们给中间段落一个`position``relative`值——这属性本身不做任何事情，所以我们还添加了`top`和`left`属性。这些可以将受影响的元素向下向右移——这可能看起来和你所期待的相反，但你需要把它看成是左边和顶部的元素被“推开”一定距离，这就导致了它的向下向右移动。

添加此代码将给出以下结果：

![css-layout05]($res/css-layout05.jpg)

### 绝对定位

绝对定位用于将元素移动到web页面的任何位置，以创建复杂的布局。有趣的是，它经常被用于与相对定位和浮动的协同工作。

回到我们最初的非定位示例，我们可以添加以下的CSS规则来实现绝对定位：

```css
.positioned {
  position: absolute;
  background: yellow;
  top: 30px;
  left: 30px;
}
```

这里我们给我们的中间段一个`position`的 `absolute`值，并且和前面一样加上 `top`和`left`属性。但是，添加此代码将给出以下结果：

![css-layout06]($res/css-layout06.jpg)

这和之前截然不同！定位元素现在已经与页面布局的其余部分完全分离，并位于页面的顶部。其他两段现在靠在一起，好像之前那个中间段落不存在一样。`top`和`left`属性对绝对位置元素的影响不同于相对位置元素。在这种情况下，他们没有指定元素相对于原始位置的移动程度。相反，它们指定元素应该从页面边界的顶部和左边的距离(确切地说，是 `<html>`元素的距离)。

我们现在暂时不讨论固定定位（ fixed positioning ）——它基本上以相同的方式工作，除了它仍然固定在浏览器窗口的边缘，而不是它定位的父节点的边缘。

## CSS 表格

HTML表格对于显示表格数据是很好的，但是很多年前——在浏览器中支持基本的CSS之前——web开发人员过去也常常使用表格来完成整个网页布局——将它们的页眉、页脚、不同的列等等放在不同的表行和列中。这在当时是有效的，但它有很多问题——表布局是不灵活的，非常重的标记，难以调试和语义上的错误（比如，屏幕阅读器用户在导航表布局方面有问题）。

CSS表格的存在是为了让您能够像表格一样布局元素，而不需要上面描述的任何问题——这听起来可能有些奇怪，您应该使用表格元素作为表格数据，但有时这可能是有用的。例如，您可能想要列出一个表单，其中有标签和文本输入；这可能很棘手，但是CSS表使其变得容易。

让我们来看一个例子。首先，创建HTML表单的一些简单标记。每个输入元素都有一个标签，我们还在一个段落中包含了一个标题。为了进行布局，每个标签/输入对都封装在`<div>`中。

```html
<form>
  <p>First of all, tell us your name and age.</p>
  <div>
    <label for="fname">First name:</label>
    <input type="text" id="fname">
  </div>
  <div>
    <label for="lname">Last name:</label>
    <input type="text" id="lname">
  </div>
  <div>
    <label for="age">Age:</label>
    <input type="text" id="age">
  </div>
</form>
```

现在，我们例子中的CSS。除了使用 `display`属性外，大多数CSS都是相当普通的。 `<form>`, `<div>`,  `<label>`和`<input>`被告知要分别显示表、表行和表单元——基本上，它们会像HTML表格标记一样，导致标签和输入在默认情况下排列整齐。我们所要做的就是添加一些大小、边缘等等，让一切看起来都好一点，我们就完成了。

你会注意到标题段落已经给出了 `display: table-caption;`——这使得它看起来就像一个表格`<caption>` ——和`caption-side: bottom;` 让标题在表格的底部下进行设计，即使标记是在源的输入之前。这就能让你有一点灵活的弹性。

```css
html {
  font-family: sans-serif;
}

form {
  display: table;
  margin: 0 auto;
}

form div {
  display: table-row;
}

form label, form input {
  display: table-cell;
  margin-bottom: 10px;
}

form label {
  width: 200px;
  padding-right: 5%;
  text-align: right;
}

form input {
  width: 300px;
}

form p {
  display: table-caption;
  caption-side: bottom;
  width: 300px;
  color: #999;
  font-style: italic;
}
```
结果如下：

![css-layout07]($res/css-layout07.jpg)

## 柔性盒子

CSS是一种功能强大的语言，它可以做很多事情，但它却在布局上有所下降。传统的老式布局方法，如`float`和`positioning`工作，但有时它们会感觉比他们需要的更复杂、更灵活、更有弹性。例如，如果你想要：

*   垂直中心盒子的内容(不仅仅是文本；`line-height` 将会失效)。
*   制作几列有相同的高度包含不同数量内容的列，不使用固定的高度，或用背景图像伪装。
*   在一行中创建几个盒子，占用相同数量的可用空间，不管有多少个，并且如果它们有内边距，外边距等就应用它。

上面的例子几乎不可能通过常规的CSS实现——柔性盒子(或flexbox)是为了让这些东西更容易实现而被发明的。

让我们来看一个例子；首先，一些简单的HTML：

```html
<section>
  <div>This is a box</div>
  <div>This is a box</div>
  <div>This is a box</div>
</section>

<button class="create">Create box</button>
<button class="reset">Reset demo</button>
```

这里我们有一个`<section>`元素，里面有三个`<div>`，再加上几个按钮来创建一个新盒子，和重新设置demo。默认情况下，子元素不会被布局或改变大小，使用传统的方法，我们必须仔细地对每个元素设置大小，允许宽度、外边距、边框和内边距，如果我们添加了另一个子元素，我们就必须完全改变所有的值。

使用Flexbox替代它：

```css
html {
  font-family: sans-serif;
}

section {
  width: 93%;
  height: 240px;
  margin: 20px auto;
  background: purple;
  display: flex;
}

div {
  color: white;
  background: orange;
  flex: 1;
  margin-right: 10px;
  text-shadow: 1px 1px 1px black;
}

div:last-child {
  margin-right: 0;
}

section, div {
  border: 5px solid rgba(0,0,0,0.85);
  padding: 10px;
}
```

这个CSS的两行非常有趣：

*   `display: flex;` 告诉`<section>`元素的子元素作为flexible boxes——默认情况下，它们都将展开以填充父类的可用高度，不管它是什么，并将其列出来——有足够的宽度来包装他们的内容。
*   `flex: 1;` 告诉每个`<div>`元素，在行中获得相同数量的空间，不管有多少。

为了进一步说明这是多么的神奇，我们还将添加一些JavaScript，以便您可以通过按下_Create box_按钮来添加进一步的子 `<div>`。

```js
var section = document.querySelector('section');
var createBtn = document.querySelector('.create');
var resetBtn = document.querySelector('.reset');

function createBox() {
  var box = document.createElement('div');
  box.textContent = 'This is a box';
  section.appendChild(box);
}

createBtn.onclick = createBox;

resetBtn.onclick = function() {
  while (section.firstChild) {
      section.removeChild(section.firstChild);
  }
  createBox();
  createBox();
  createBox();
}
```

这是一个例子——多试试见证Flexbox作为一个布局工具的强大。

![css-layout08]($res/css-layout08.gif)

## 网格布局

这里提到的最具实验性的特性是CSS网格，它在浏览器中还没有得到广泛的支持。Web页面通常使用网格系统布局，与打印媒体相同，这里的想法是通过定义一个网格来简化这个过程，然后定义内容的哪些部分位于网格的每个区域。

目前的CSS网格在任何地方都还没有得到真正的支持(除了Firefox和Chrome的实验性版本)。
IE和Edge支持一种更旧的、过时的技术。这是我们将来可以期待的！

**注意：**为了更好地了解当前的网格框架和其他正在使用的技术，以及即将到来的原生CSS网格规范，请参阅我们的 [Grids](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Grids) 文章。

end