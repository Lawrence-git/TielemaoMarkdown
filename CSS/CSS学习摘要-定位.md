CSS学习摘要-定位
===
注：全文摘自[MDN-CSS定位](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/CSS_layout/%E5%AE%9A%E4%BD%8D)

[[toc]]

定位允许您从正常的文档流布局中取出元素，并使它们具有不同的行为，例如放在另一个元素的上面，或者始终保持在浏览器视窗内的同一位置。 本文解释的是定位([`position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position "CSS position属性用于指定一个元素在文档中的定位方式。top，right，bottom 和 left 属性则决定了该元素的最终位置。"))的各种不同值，以及如何使用它们。

## 文档流

定位是一个相当复杂的话题，所以我们深入了解代码之前，让我们审视一下布局理论，并让我们了解它的工作原理。

首先，环绕元素内容添加任何内边距、边界和外边距来布置单个元素盒子——这就是 [盒模型](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Box_model) ，我们前面看过。 默认情况下，块级元素的内容宽度是其父元素的宽度的100％，并且与其内容一样高。内联元素高宽与他们的内容高宽一样。您不能对内联元素设置宽度或高度——它们只是位于块级元素的内容中。 如果要以这种方式控制内联元素的大小，则需要将其设置为类似块级元素 `display: block;`。

这只是解释了单个元素，但是元素相互之间如何交互呢？ **正常的布局流**（在布局介绍文章中提到）是将元素放置在浏览器视口内的系统。默认情况下，块级元素在视口中垂直布局——每个都将显示在上一个元素下面的新行上，并且它们的外边距将分隔开它们。

内联元素表现不一样——它们不会出现在新行上；相反，它们互相之间以及任何相邻（或被包裹）的文本内容位于同一行上，只要在父块级元素的宽度内有空间可以这样做。如果没有空间，那么溢流的文本或元素将向下移动到新行。

如果两个相邻元素都在其上设置外边距，并且两个外边距接触，则两个外边距中的较大者保留，较小的一个消失——这叫[外边距折叠](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing), 我们之前也遇到过。

让我们来看一个简单的例子来解析这一切：

```html
<h1>Basic document flow</h1>

<p>I am a basic block level element. My adjacent block level elements sit on new lines below me.</p>

<p>By default we span 100% of the width of our parent element, and we are as tall as our child content. Our total width and height is our content + padding + border width/height.</p>

<p>We are separated by our margins. Because of margin collapsing, we are separated by the width of one of our margins, not both.</p>

<p>inline elements <span>like this one</span> and <span>this one</span> sit on the same line as one another, and adjacent text nodes, if there is space on the same line. Overflowing inline elements will <span>wrap onto a new line if possible (like this one containing text)</span>, or just go on to a new line if not, much like this image will do: <img src="https://mdn.mozillademos.org/files/13360/long.jpg"></p>
```

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
![css-location01]($res/css-location01.jpg)

在我们阅读本文时，我们将多次重复这个例子，因为我们要展示不同定位选项的效果。

如果可能，我们希望您在本地计算机上跟随练习 — 从GitHub仓库下载一个`[0_basic-flow.html](http://mdn.github.io/learning-area/css/css-layout/positioning/0_basic-flow.html)` ([源代码](https://developer.mozilla.org/zh-CN/docs/Mozilla/Developer_guide/Source_Code/Downloading_Source_Archives)) 然后用它作为我们的起步点。

## 介绍定位

定位的整个想法是允许我们覆盖上面描述的基本文档流行为，以产生有趣的效果。如果你想稍微改变布局中一些盒子的位置，使它们的默认布局流程位置稍微有点古怪，不舒服的感觉呢？定位是你的工具。或者，如果您想要创建一个浮动在页面其他部分顶部的UI元素，并且/或者始终停留在浏览器窗口内的相同位置，无论页面滚动多少？定位使这种布局工作成为可能。

有许多不同类型的定位，您可以对HTML元素生效。要使某个元素上的特定类型的定位，我们使用[`position`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position "CSS position属性用于指定一个元素在文档中的定位方式。top，right，bottom 和 left 属性则决定了该元素的最终位置。")属性。

### 静态定位

静态定位是每个元素获取的默认值——它只是意味着“将元素放入它在文档布局流中的正常位置 ——这里没有什么特别的。

为了演示这一点，并为以后的部分设置示例，首先在HTML中添加一个`positioned` 的 `class` 到第二个`<p>`：

```html
<p class="positioned"> ... </p>
```

现在，将以下规则添加到CSS的底部：

```css
.positioned {
   position: static;
   background: yellow;
}
```

如果现在保存和刷新，除了第2段的更新的背景颜色，根本没有差别。这很好——正如我们之前所说，静态定位是默认行为！

### 相对定位

相对定位是我们将要看的第一个位置类型。 它与静态定位非常相似，占据在正常的文档流中，除了你仍然可以修改它的最终位置，包括让它与页面上的其他元素重叠。 让我们继续并更新代码中的 position 属性：

```css
position: relative;
```

如果您在此阶段保存并刷新，则结果根本不会发生变化——那么如何修改元素的位置呢？ 您需要使用`top`，`bottom`，`left`和`right`属性 ，我们将在下一节中解释。

### 介绍 top, bottom, left,  right

* `top` top样式属性定义了定位元素的上外边距边界与其包含块上边界之间的偏移，非定位元素设置此属性无效。
* `bottom` bottom样式属性定义了定位元素下外边距边界与其包含块下边界之间的偏移，非定位元素设置此属性无效。
*  `left` left属性定义了定位元素的左外边距边界与其包含块左边界之间的偏移，非定位元素设置此属性无效。
*  `right` right样式属性定义了定位元素的右外边距边界与其包含块右边界之间的偏移，非定位元素设置此属性无效。 

使用上述四个属性来精确指定要将定位元素移动到的位置。 要尝试这样做，请在CSS中的 `.positioned` 规则中添加以下声明：

```html
top: 30px;
left: 30px;
```

**注意**：这些属性的值可以采用逻辑上期望的任何单位 ——px，mm，rems，％等。

如果你现在保存和刷新，你会得到一个这样的结果：

![css-position01]($res/css-position01.jpg)

酷，是吗？ 好吧，所以这个结果这可能不是你期待的——为什么它移动到底部和右边，但我们指定顶部和左边？ 听起来不合逻辑，但这只是相对定位工作的方式——你需要考虑一个看不见的力，推动定位的盒子的一侧，移动它的相反方向。 所以例如，如果你指定 `top: 30px;`一个力推动框的顶部，使它向下移动30px。

### 绝对定位

绝对定位带来了非常不同的结果。让我们尝试改变代码中的位置声明如下：

```html
position: absolute;
```
如果你现在保存和刷新，你应该看到这样：

![css-position02]($res/css-position02.jpg)

首先，请注意，定位的元素应该在文档流中的间隙不再存在——第一和第三元素已经关闭在一起，就像它不再存在！好吧，在某种程度上，这是真的。绝对定位的元素不再存在于正常文档布局流中。相反，它坐在它自己的层独立于一切。这是非常有用的——这意味着我们可以创建不干扰页面上其他元素的位置的隔离的UI功能 ——例如弹出信息框和控制菜单，翻转面板，可以在页面上的任何地方拖放的UI功能等。

第二，注意元素的位置已经改变——这是因为`top`，`bottom`，`left`和`right`以不同的方式在绝对定位。 它们指定元素应距离每个包含元素的边的距离，而不是指定元素应该移入的方向。 所以在这种情况下，我们说的绝对定位元素应该位于从“包含元素”的顶部30px，从左边30px。

**注意**：您可以使用`top`，`bottom`，`left`和`right`如果需要，调整元素大小。 尝试设置 `top: 0; bottom: 0; left: 0; right: 0;` 和 `margin: 0;` 对你定位的元素，看看会发生什么！ 之后再回来。

**注意**：是的，外边距仍会影响定位的元素。 然而，外边距折叠不会。

### 定位上下文

哪个元素是绝对定位元素的“包含元素”？ 默认情况下，它是`<html>`元素——定位的元素是被嵌套在`<body>`中的HTML源代码，但在最终的布局，它离页面边缘的顶部和左侧30px距离，这是`<html>`元素。 这更准确地称为元素的**定位上下文**。

我们可以改变定位上下文——绝对定位的元素相对于其定位的元素。 这是通过在元素的其他祖先之一上设置定位来实现的——它是嵌套在其中的元素之一（你不能相对于其中没有嵌套的元素来定位它）。 为了演示这一点，将以下声明添加到您的body规则中：

```html
position: relative;
```
这应该给出以下结果：

![css-position03]($res/css-position03.jpg)

定位的元素现在相对于`<body>`元素。


### 介绍 z-index

所有这些绝对定位很有趣，但还有另一件事我们还没有考虑到 ——当元素开始重叠，什么决定哪些元素出现在其他元素的顶部？ 在我们已经看到的示例中，我们在定位上下文中只有一个定位的元素，它出现在顶部，因为定位的元素胜过未定位的元素。 当我们有不止一个的时候呢？

尝试添加以下到您的CSS，使第一段也是绝对定位：

```html
p:nth-of-type(1) {
  position: absolute;
  background: lime;
  top: 10px;
  right: 30px;
}
```

此时，您将看到第一段的颜色为绿色，移出文档流程，并位于原始位置上方一点。它也堆叠在原始的 `.positioned` 段落下，其中两个重叠。这是因为 `.positioned` 段落是源顺序中的第二个段落，并且源顺序中后定位的元素将赢得先定位的元素。

您可以更改堆叠顺序吗？是的，您可以使用[`z-index`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/z-index "z-index 属性指定了一个具有定位属性的元素及其子代元素的 z-order。 当元素之间重叠的时候，z-order 决定哪一个元素覆盖在其余元素的上方显示。 通常来说 z-index 较大的元素会覆盖较小的一个。")属性。 “z-index”是对z轴的参考。你可以从源代码中的上一点回想一下，我们使用水平（x轴）和垂直（y轴）坐标来讨论网页，以确定像背景图像和阴影偏移之类的东西的位置。 （0,0）位于页面（或元素）的左上角，x和y轴跨页面向右和向下（适合从左到右的语言，无论如何）。

网页也有一个z轴——一条从屏幕表面到你的脸（或者在屏幕前面你喜欢的任何其他东西）的虚线。`z-index`值影响定位元素位于该轴上的位置——正值将它们移动到堆栈上方，负值将它们向下移动到堆栈中。默认情况下，定位的元素都具有z-index为auto，实际上为0。

要更改堆叠顺序，请尝试将以下声明添加到 `p:nth-of-type(1)` 规则中：

```html
z-index: 1;
```
你现在应该看到完成的例子：

![css-position04]($res/css-position04.jpg)

请注意，z-index只接受无单位索引值；你不能指定你想要一个元素是Z轴上23像素—— 它不这样工作。 较高的值将高于较低的值，这取决于您使用的值。 使用2和3将产生与300和40000相同的效果。


### 固定定位

还有一种类型的定位覆盖——fixed。 这与绝对定位的工作方式完全相同，只有一个主要区别——绝对定位固定元素是相对于 `<html>`元素或其最近的定位祖先，而固定定位固定元素则是相对于浏览器视口本身。 这意味着您可以创建固定的有用的UI项目，如持久导航菜单。

让我们举一个简单的例子来说明我们的意思。首先，从CSS中删除现有的 `p:nth-of-type(1)` 和`.positioned` 规则。

现在，更新 `body` 规则以删除`position: relative;` 声明并添加固定高度，如此：

```html
body {
  width: 500px;
  height: 1400px;
  margin: 0 auto;
}
```

现在我们要给`<h1>`元素 `position: fixed;`，并让它坐在视口的顶部中心。将以下规则添加到CSS：

```html
h1 {
  position: fixed;
  top: 0;
  width: 500px;
  margin: 0 auto;
  background: white;
  padding: 10px;
}
```

 `top: 0;`是要使它贴在屏幕的顶部；我们然后给出标题与内容列相同的宽度，并使用可靠的老技巧 `margin: 0 auto;` 使它居中。 然后我们给它一个白色背景和一些内边距，所以内容将不会在它下面可见。

如果您现在保存并刷新，您会看到一个有趣的小效果，标题保持固定，内容显示向上滚动并消失在其下。 但是我们可以改进这一点——目前标题下面挡住一些内容的开头。这是因为定位的标题不再出现在文档流中，所以其他内容向上移动到顶部。 我们要把它向下移动一点；我们可以通过在第一段设置一些顶部边距来做到这一点。添加：

```html
p:nth-of-type(1) {
  margin-top: 60px;
}
```

你现在应该看到完成的例子：

![css-position05]($res/css-position05.gif)


### 实验属性: position sticky

有一个新的定位值可用称为 `position: sticky`，尚未广泛支持。 这基本上是相对位置和固定位置之间的混合，其允许定位的元件像它被相对定位一样动作，直到其滚动到某一阈值点（例如，从视口顶部10像素），之后它变得固定。阅读我们的[position:sticky 引用入口](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position#Sticky_positioning) 查看详情和例子。

## 总结

我相信你用基本定位愉快地玩耍——它是创建复杂的CSS布局和UI功能背后的基本工具之一。 考虑到这一点，在下一篇文章中，我们将更有趣的定位——我们将进一步，开始建立一些真实世界有用的东西。


【end】