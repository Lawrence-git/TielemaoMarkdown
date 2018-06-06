# CSS学习摘要-数值与单位及色彩

[[toc]]

在CSS中，值的类型有很多种，一些很常见，一些你却几乎没怎么遇到过。我们不会在这篇文档中面面俱到地描述他们，而只是这些对于掌握CSS可能最有用处的这些。本文将会涉及如下CSS的值：

*   数值: 长度值，用于指定例如元素宽度、边框（border）宽度或字体大小；以及无单位整数。用于指定例如相对线宽或运行动画的次数。
*   百分比: 可以用于指定尺寸或长度——例如取决于父容器的长度或高度，或默认的字体大小。
*   颜色: 用于指定背景颜色，字体颜色等。
*   坐标位置: 例如，以屏幕的左上角为坐标原点定位元素的位置。
*   函数: 例如，用于指定背景图片或背景图片渐变。

## 数值
你会在很多地方看到CSS单位中使用到数值。这一部分将会讨论数值的最常用类别。

### 长度和尺寸

在CSS布局、排版等等的所有时候，你都会使用到长度/尺寸单位（`<length>`)。我们不妨先看一个简单的例子——先上HTML：

```html
 <p>This is a paragraph.</p>
<p>This is a paragraph.</p>
<p>This is a paragraph.</p>
```
然后是CSS：

```css
p {
  margin: 5px;
  padding: 10px;
  border: 2px solid black;
  background-color: cyan;
}

p:nth-child(1) {
  width: 150px;
  font-size: 18px;
}

p:nth-child(2) {
  width: 250px;
  font-size: 24px;
}

p:nth-child(3) {
  width: 350px;
  font-size: 30px;
}
```
结果如下：

![css-length]($res/css-length.jpg)

在这个例子中我们做了以下事情：

*   分别将每个段落的 [`margin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin "margin属性为给定元素设置所有四个（上下左右）方向的外边距属性。这是四个外边距属性设置的简写。四个外边距属性设置分别是： margin-top， margin-right， margin-bottom 和 margin-left 。指定的外边距允许为负数。")，[`padding`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding "padding属性设置一个元素的内边距，padding 区域指一个元素的内容和其边界之间的空间，该属性不能为负值。") 和 [`border-width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border-width "The border-width property sets the width of the border of a box. Using the shorthand property border is often more convenient.") 设置为5 pixels, 10 pixels 和 2 pixels。一个单独的margin/padding值表示所有的4个面都被设置成同样的值。边框也被设置成了 [`border`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border "CSS的border属性是一个用于设置各种单独的边界属性的简写属性。border可以用于设置一个或多个以下属性的值： border-width, border-style, border-color。") 的缩写值。
*   为三个不同的段落设置越来越大的宽度（[`width`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/width "width 属性指定了元素内容区的宽度. 内容区在元素padding，border和margin里面。") ）像素值，也就是意味着越往下盒子越大。
*   为三个不同的段落设置越来越大字号（ [`font-size`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font-size "font-size CSS 属性指定字体的大小。因为该属性的值会被用于计算em和ex长度单位，定义该值可能改变其他元素的大小。")）像素值，也就是意味着越往下文本越大。`font-size代表字体/字形的高度。`

像素 (px) 是一种绝对单位（**absolute units**）**，** 因为无论其他相关的设置怎么变化，像素指定的值是不会变化的。其他的绝对单位如下：

*   `mm`, `cm`, `in`: 毫米（Millimeters），厘米（centimeters），英寸（inches）
*   `pt`, `pc`: 点（Points (1/72 of an inch)）， 十二点活字（ picas (12 points.)）

除了px之外，你很可能都不怎么使用其他的单位。

也有相对单位，他们是相对于当前元素的字号（ `font-size` ）或者视口`viewport` 尺寸。

*   `em`:1em与当前元素的字体大小相同（更具体地说，一个大写字母M的宽度）。CSS样式被应用之前，浏览器给网页设置的默认基础字体大小是16像素，这意味着对一个元素来说1em的计算值默认为16像素。但是要小心—em单位是会继承父元素的字体大小，所以如果在父元素上设置了不同的字体大小，em的像素值就会变得复杂。现在不要过于担心这个问题，我们将在后面的文章和模块中更详细地介绍继承和字体大小设置。**em是Web开发中最常用的相对单位**。
*   `ex`, `ch`: 分别是小写x的高度和数字0的宽度。这些并不像em那样被普遍使用或很好地被支持。
*   `rem`: REM（root em）和em以同样的方式工作，但它总是等于默认基础字体大小的尺寸；继承的字体大小将不起作用，所以这听起来像一个比em更好的选择，虽然在旧版本的IE上不被支持。
*   `vw`, `vh`: 分别是视口宽度的1/100和视口高度的1/100，其次，它不像rem那样被广泛支持。

使用相对单位是非常有用的-你可以调整你的HTML元素大小相对于你的字体或视口大小，这意味着布局将保持看起来正确，假设举个例子是一个视障用户的话，整个网站上的文本大小是原来的两倍。

### 无单位的值

你有时候会在CSS遇到无单位的数值，这不总是一个错误，事实上它是在某些情况下，完全可以。例如，如果你想让一个元素完全去除外边框和内边框，你可以只使用无单位的0 — 0就是0，不管前面设置的单位是什么！

```css
margin: 0;
```

#### 无单位的行高

另一个例子是 `line-height` 设置元素中每行文本的高度。你可以使用单位设置特定的行的高度，但使用一个无量纲的值往往更容易，它就像一个简单的乘法因子。

注：`line-height `
CSS属性中设置用于行所需要的空间，例如文本。
对于块级元素，它指定元素中线框的最小高度。
在未替换的内联元素中，它指定用于计算线框高度的高度。
对于替代行内元素，如 button 或其他 input 元素，line-height 没有影响。
对于部分替代元素，line-height 依然可以影响元素的样式布局。

#### 动画的数值

`CSS动画`能够让页面上的HTML元素动起来。我们来看一个例子，当我们把鼠标浮动到一个段落上的时候，它能够旋转起来。这个例子的HTML代码很简单： 

```html
<p>Hello</p>
```
CSS有点复杂：

```css
@keyframes rotate {
  0% {
    transform: rotate(0deg);
  }

  100% {
    transform: rotate(360deg);
  }
}

p {
  color: red;
  width: 100px;
  font-size: 40px;
  transform-origin: center;
}

p:hover {
  animation-name: rotate;
  animation-duration: 0.6s;
  animation-timing-function: linear;
  animation-iteration-count: 5;
}
```
这里你可以看到一些我们之前没有明确提到的有趣单位，但是我们感兴趣的是这一行 `animation-iteration-count: 5;` ——此行控制着动画启动（这里是指光标浮动至段落上）时后会执行多少次，而且这是一个简短无单位纯数字（计算机中称之为整型）。

以下是代码的实际效果：

![css-length02]($res/css-length02.gif)


## 百分比

还可以使用百分比值指定可以通过特定数值指定的大部分内容。这使我们可以创建，例如，其宽度总是会移动到其父容器宽度的一定百分比的框中。这可以与那些将其宽度设置为某个单位值（如px或ems）的框进行比较，它们的长度总是保持相同的长度，即使它们的父容器的宽度发生变化。

让我们举个例子来说明这个问题。

首先，用HTML标记的两个相似的盒子：

```
<div>
  <div class="boxes">Fixed width layout with pixels</div>
  <div class="boxes">Liquid layout with percentages</div>
</div>
```

现在一些CSS来装饰这些盒子：

```
div .boxes {
  margin: 10px;
  font-size: 200%;
  color: white;
  height: 150px;
  border: 2px solid black;
}

.boxes:nth-child(1) {
  background-color: red;
  width: 650px;
}

.boxes:nth-child(2) {
  background-color: blue;
  width: 75%;
}
```
这给了我们以下结果：

![css-length03]($res/css-length03.gif)

在这里，我们给两个divs一些 [`margin`] [`height`] [`font-size`] [`border`] 和 [`color`] 。 

然后我们给第一个div和第二个div不同的 [`background-color`]，所以我们可以很容易地区分开他们。 我们还将第一个div的 [`width`]设置为650px，将第二个div的宽度设置为75％。 这样做的效果是，第一个div始终具有相同的宽度，即使视口大小被调整（当视口变得比屏幕更窄时，它将从屏幕上消失），而第二个div的宽度随着视口（viewport ）的变化而变化，使其始终保持其父元素的75％。 在这个例子中，div的父元素是`<body>`元素，默认情况下是视口宽度的100％。

**注意**: 您可以通过调整本文中的浏览器窗口来看到这种效果；
`margin`属性为给定元素设置所有四个（上下左右）方向的外边距属性。这是四个外边距属性设置的简写。四个外边距属性设置分别是： margin-top， margin-right， margin-bottom 和 margin-left 。指定的外边距允许为负数。

`font-size` CSS 属性中指定字体的大小。因为该属性的值会被用于计算em和ex长度单位，定义该值可能改变其他元素的大小。

`border`属性是一个用于设置各种单独的边界属性的简写属性。border可以用于设置一个或多个以下属性的值： border-width, border-style, border-color。

`background-color` 会设置元素的背景色, 属性的值为颜色值或关键字`transparent`二者选其一。

`width` 属性指定了元素内容区的宽度. 内容区在元素padding，border和margin里面。

我们也可以设置`font-size`的百分比为200%。它将和你预计的工作方式有一点不同，但这是讲得通的，它的新大小是相对于父容器的字体大小的，就像`em`一样。在这种情况下，父容器的字体大小为16px——页面的默认值，所以计算的值为16px的200%，或32px。这和`em`的风格确实很类似—200%基本上和2`em`一样。

这两种不同的框布局类型通常称为**动态（流体）布局**（如浏览器视口大小的变化），**固定宽度布局**（不管怎样都保持不变）：

*   可以使用动态布局来确保标准文档始终适合于屏幕，并且可以在不同的移动设备屏幕大小上表现良好。
*   一个固定宽度的布局可以用来保持在线地图的大小相同；浏览器视口可以在地图上滚动，只在一个时间内查看一定数量的地图。您可以立即看到的量取决于您的视口有多大。

## 颜色

CSS中指定颜色的方法有很多，其中一些是最近实现的。CSS中到处都可以使用相同的颜色值，无论是指定文本颜色、背景颜色，还是其他任何颜色。

现代计算机中可用的标准颜色系统是24位，通过不同的红、绿、蓝通道，每个通道有256种不同的值，从而显示出大约1670万种不同的颜色。  (256 x 256 x 256 = 16,777,216.)

让我们运行不同的可用类型的颜色值。

### 关键词（色彩关键字）

最简单、最古老的颜色类型，CSS颜色关键词。这些都是特定的字符串代表特定的颜色值。例如red代表红色。

这很容易理解，尽管它只能让我们指定明显的颜色基元。有大约165个不同的关键字可用于现代Web浏览器-见 [full color keyword list](https://developer.mozilla.org/zh-CN/docs/Web/CSS/color_value#Color_keywords).

你可能会在工作中经常使用诸如红色、黑色和白色这样的纯颜色，但除此之外，你还想使用另一种颜色系统。

### 十六进制值

下一个常用的颜色系统是十六进制颜色或十六进制代码。每个颜色包括一个哈希/磅符号（`#`）和其后面紧跟的六个十六进制数，其中每个十六进制数可以是0和F之间的一个值（一共16个），0123456789abcdef。每对十六进制数代表一个通道（红色、绿色或者蓝色）允许我们指定256个可用值。 (16 x 16 = 256.)  

例如，这个代码：

```html
<p>This paragraph has a red background</p>
<p>This paragraph has a blue background</p>
<p>This paragraph has a kind of pinky lilac background</p>
```

```css
/* equivalent to the red keyword */
p:nth-child(1) {
  background-color: #ff0000;
}

/* equivalent to the blue keyword */
p:nth-child(2) {
  background-color: #0000ff;
}

/* has no exact keyword equivalent */
p:nth-child(3) {
  background-color: #e0b0ff;
}
```
给出以下结果：

![css-color01]($res/css-color01.jpg)

这些值比较复杂，不太容易理解，但是它们比关键字更灵活——您可以使用十六进制值来表示您想要在颜色方案中使用的任何颜色。

### RGB

我们要讨论的第三个方案是RGB。一个RGB值是一个函数rgb() -这是给定的三个参数表示的红色，绿色和蓝色通道的颜色值，在大致相同的方式作为十六进制值。与RGB的区别在于，每个通道不是由两个十六进制数字表示的，而是由0到255之间的十进制数表示的。

让我们重写上面那个例子来使用RGB颜色：

```html
<p>This paragraph has a red background</p>
<p>This paragraph has a blue background</p>
<p>This paragraph has a kind of pinky lilac background</p>
```

```css
/* equivalent to the red keyword */
p:nth-child(1) {
  background-color: rgb(255,0,0);
}

/* equivalent to the blue keyword */
p:nth-child(2) {
  background-color: rgb(0,0,255);
}

/* has no exact keyword equivalent */
p:nth-child(3) {
  background-color: rgb(224,176,255);
}
```

这给出了和上面例子同样的结果。

RGB值的支持度与十六进制不相上下，在你的工作中你可能不会遇到任何不支持它们的浏览器。

RGB值可以说比十六进制值更直观，更容易理解。

**注意**: 为什么是255而不是256？计算机系统倾向于从0计算，而不是从1计算。所以允许256个可能的值，RGB颜色在0-255范围值，不是1-256。

### HSL

支持度比RGB稍微差一点的是HSL（不支持旧版本IE），这是开发者非常感兴趣而实施的——不只是红、绿和蓝色的值，该`hsl()`函数接受的**色相**、**饱和度**以及**明度值**，以与上述三种不同的方式用来区分167万种颜色：

1.  **色调**：颜色的底色调。这个值在0到360之间，表示色轮周围的角度。
2.  **饱和度**：饱和度是多少？这需要一个从0-100%的值，其中0是没有颜色（它将显示为灰色），100%是全彩色饱和度。
3.  **明度**：颜色有多亮或明亮？这需要一个从0-100%的值，其中0是无光（它会出现全黑的），100%是充满光的（它会出现全白）

现在我们用HSL颜色重写上面的例子:

```html
<p>This paragraph has a red background</p>
<p>This paragraph has a blue background</p>
<p>This paragraph has a kind of pinky lilac background</p>
```

```css
/* equivalent to the red keyword */
p:nth-child(1) {
  background-color: hsl(0,100%,50%);
}

/* equivalent to the blue keyword */
p:nth-child(2) {
  background-color: hsl(240,100%,50%);
}

/* has no exact keyword equivalent */
p:nth-child(3) {
  background-color: hsl(276,100%,85%);
}
```
HSL颜色模型对于常使用这样的模型的设计师来说非常直观。例如,找到一组色调以单配色方式使用是非常有用的。

```html
/* three different shades of red*/
background-color: hsl(0,100%,50%);
background-color: hsl(0,100%,60%);
background-color: hsl(0,100%,70%);
```

### RGBA 和 HSLA

RGB和HSL都有相应的模式——RGBA和HSLA——不仅允许您设置想要显示的颜色,还有此颜色的**透明度（** **transparency** ）。它们与与之相应的函数采用同样的参数,再加上第四个范围在0-1的值——设置透明度,或者说**alpha通道**。0是完全透明的,1是完全不透明的。

让我们展示另一个简单的例子,第一个HTML:

```html
<p>This paragraph has a transparent red background</p>
<p>This paragraph has a transparent blue background</p>
```

现在CSS——我们正在第一段向下定位,显示段落重叠的效果(以后你会了解更多关于定位的知识):

```css
p {
  height: 50px;
  width: 350px;
}

/* Transparent red */
p:nth-child(1) {
  background-color: rgba(255,0,0,0.5);
  position: relative;
  top: 30px;
  left: 50px;
}

/* Transparent blue */
p:nth-child(2) {
  background-color: hsla(240,100%,50%,0.5);
}
```
结果如下：

![css-color02]($res/css-color02.jpg)

透明的颜色更丰富的视觉效果非常有用——混合的背景,半透明的UI元素等等。

### 不透明度（Opacity）

还有另一种方法来指定透明度，通过CSS——[`opacity`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/opacity) 属性。
与设置某个特定颜色的透明度相比，这会设置所有选定元素以及它们的孩子节点的不透明度。

opacity属性指定了一个元素的透明度。换言之，opacity属性指定了一个元素后面的背景的被覆盖程度。

为了看出他们的区别，我们来研究下面这个例子：

```html
<p>This paragraph is using RGBA for transparency</p>
<p>This paragraph is using opacity for transparency</p>
```
现在CSS是：

```css
/* Red with RGBA */
p:nth-child(1) {
  background-color: rgba(255,0,0,0.5);
}

/* Red with opacity */
p:nth-child(2) {
  background-color: rgb(255,0,0);
  opacity: 0.5;
}
```
结果如下：

![css-color03]($res/css-color03.jpg)

注意区别——第一个盒子使用RGBA颜色，只有一个半透明的背景,而一切在第二个盒子里的都是透明的,包括文本。您要仔细思考使用两者的时机——例如,当您想创建一个图片覆盖的标题，图片能透过标题显示，且标题的文本显示不受影响，此时应该使用RGBA修改标题背景色的透明度；另一方面,当您想要创建一个动画效果,整个UI元素从完全隐藏到可见，此时应该使用不透明度（Opacity）。

###  transparent 关键字

`transparent` 关键字表示一个完全透明的颜色，即该颜色看上去将是背景色。从技术上说，它是带有阿尔法通道为最小值的黑色，是 `rgba(0,0,0,0)` 的简写。

### currentColor 关键字

`currentColor` 关键字代表原始的 `color` 属性的计算值。它允许让继承自属性或子元素的属性颜色属性以默认值不再继承。

它也能用于那些继承了元素的 `color` 属性计算值的属性，相当于在这些元素上使用 `inherit`关键字，如果这些元素有该关键字的话。

附图：大部分的色彩关键字和rgb表示
![ccs-color04]($res/ccs-color04.jpg)

（译者注：颜色译名仅供参考。）

end
2018-5-31

全文摘录自[MDN网络开发者](https://developer.mozilla.org/zh-CN)网站