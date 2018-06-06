[[toc]]

注：全文摘要自[网络开发者网站](https://developer.mozilla.org)，当然间隔也会整理一些思路和格式排版添加进去。

CSS框模型（译者注：也被称为“盒模型”）是网页布局的基础 ——每个元素被表示为一个矩形的方框，框的内容、内边距、边界和外边距像洋葱的膜那样，一层包着一层构建起来。浏览器渲染网页布局时，它会算出每个框的内容要用什么样式，周围的洋葱层有多大，以及框相对于其它框放在哪里。在理解如何创建 CSS 布局之前，你需要理解框模型。

# 框属性

文档的每个元素被构造成文档布局内的一个矩形框，框每层的大小都可以使用一些特定的CSS属性调整。相关属性如下:

![](https://mdn.mozillademos.org/files/13647/box-model-standard-small.png)

## 【`width` 和 `height`】宽度和高度

`width` 和 `height`设置**内容框（content box）**的宽度和高度。内容框是框内容显示的区域——包括框内的文本内容，以及表示嵌套子元素的其它框。

**注意**: 还有其他属性可以更巧妙地处理内容的大小——设置大小约束而不是绝对的大小。这些属性包括`min-width`、`max-width`、`min-height` 和 `max-height`。

## 【`padding`】内边距

**padding **表示一个 CSS 框的**内边距** ——这一层位于内容框的外边缘与边界的内边缘之间。

该层的大小可以通过简写属性`padding`一次设置所有四个边，或用 `padding-top`、`padding-right`、`padding-bottom`和`padding-left`  属性一次设置一个边。

* padding-top 是指一个元素在内边距区域（padding area）中上方的高度。

* padding-bottom 是指一个元素在内边距区域（padding area）中下方的高度。

* padding-left 是指一个元素在内边距区域（padding area）中左边的宽度。

* padding-right 是指一个元素在内边距区域（padding area）中右边的宽度。

* 内边距（padding）是指一个元素的内容和边框之间的区域。和外边距（margin）不同，内边距（padding）是不允许有负值的。内边距（padding）可以用四个值声明一个元素的四个方向的内边距（paddings），这是一种CSS缩写属性。

## 【`border`】边界边框

CSS的border属性是一个用于设置各种单独的边界属性的简写属性。border可以用于设置一个或多个以下属性的值： border-width, border-style, border-color。

CSS 框的**边界**（border）是一个分隔层，位于内边距的外边缘以及外边距的内边缘之间。边界的默认大小为0——从而让它不可见——不过我们可以设置边界的厚度、风格和颜色让它出现。

 `border`简写属性可以让我们一次设置所有四个边，例如  `border: 1px solid black; `但这个简写可以被各种普通书写的更详细的属性所覆盖：

*   `border-top`，`border-right`, `border-bottom`, `border-left` : 分别设置某一边的边界厚度／风格／颜色。

*   `border-width`, `border-style`, `border-color`: 分别仅设置边界的厚度／风格／颜色，并应用到全部四边边界。

*   你也可以单独设置某一个边的三个不同属性，如 `border-top-width`, `border-top-style`, `border-top-color`，等。 

附：
* border-top是属性 border-top-color, border-top-style, 和border-top-width 的三者的缩写。这些属性都是在描述一个元素的上方的边框border。

* border-right 是属性border-right-color, border-right-style, 和border-right-width的三者的缩写。这些属性都是在描述一个元素的右边的边框border。

* border-bottom 简写属性把下边框的所有属性： border-bottom-color，border-bottom-style 与 border-bottom-width设置到了一个声明中。这些属性描述了元素的下边框样式。

* border-left 是属性border-left-color, border-left-style, 和border-left-width的三者的缩写。这些属性都是在描述一个元素的左边的边框border。

* border-style 是一个 CSS 简写属性，用来设定元素所有边框的样式。

* border-color 是一个用于设置元素四个边框颜色的快捷属性： border-top-color, border-right-color, border-bottom-color, border-left-color

## 【`margin`】外边距

外边距（margin）代表 CSS 框周围的外部区域，称为**外边距**，它在布局中推开其它 CSS 框。其表现与 padding 很相似；简写属性为 `margin`，单个属性分别为 `margin-top`、`margin-right`、`margin-bottom` 和`margin-left`。

**注意**: 外边距有一个特别的行为被称作`外边距塌陷（margin collapsing）`：当两个框彼此接触时，它们的间距将取两个相邻外边界的最大值，而非两者的总和。

* margin属性为给定元素设置所有四个（上下左右）方向的外边距属性。这是四个外边距属性设置的简写。四个外边距属性设置分别是： margin-top， margin-right， margin-bottom 和 margin-left 。指定的外边距允许为负数。

* margin-top 属性用来设置元素的顶部外边距，你也可以使用负值。

* margin-bottom 属性用于设置元素的底部外边距，允许设置负数值。一个正数值将让它相对于正常流与邻近块更远，而负数值将使得更近。

* margin-left  属性 设置与元素相关联的盒子模型的左外边距。这个值可以为负值。

* 竖直排列相邻的两个盒子模型的外边距会重叠，称为 margin collapsing.

## 一些提示及想法

*   默认情况下`background-color`/`background-image` 延伸到了border的内边缘。`background-clip`( background-clip  设置元素的背景（背景图片或颜色）是否延伸到边框下面。) 属性来改变。

*   如果content框变得比示例输出窗口大，它将从窗口溢出，此时会出现滚动条，你可以滚动窗口来查看盒子剩余的内容 。你可以使用`overflow`（overflow 属性指定当内容溢出其块级容器时,是否剪辑内容，显示滚动条或显示溢出的内容。)属性来控制溢出。

*   框的高度不遵守百分比的长度；框的高度总是采用框内容的高度，除非指定一个绝对的高度（如：px 或者em），它会比在页面上默认是100%高度更实用。

*   border也忽略百分比宽度设置。

*   你应该注意的是框的总宽度是`width`, `padding-right`, `padding-left`, `border-right`, 以及 `border-left` 属性之和。在一些情况下比较恼人（例如，假使你想要一个框占窗口宽度的50%，但边界和内边距是用像素来表示时该怎么办？），为了避免这种问题，有可能使用属性`box-sizing`来调整框模型。使用值`border-box`，它将框模型更改成这个新的模型：

![box-model-alt-small]($res/box-model-alt-small.png)

box-sizing 属性用于更改用于计算元素宽度和高度的默认的 CSS 盒子模型。可以使用此属性来模拟不正确支持CSS盒子模型规范的浏览器的行为。

# 高级的框操作

在设置框的width, height, border, padding 及margin之外， 还有一些其他的属性可以改变它们的行为。本节讨论这些其他的属性。

## 溢流

当你使用绝对的值设置了一个框的大小（如，固定像素的宽/高），允许的大小可能不适合放置内容，这种情况下内容会从盒子溢流。我们使用[`overflow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow "overflow 属性指定当内容溢出其块级容器时,是否剪辑内容，显示滚动条或显示溢出的内容。")属性来控制这种情况的发生。它有一些可能的值，但是最常用的是：

*   `auto`: 当内容过多，溢流的内容被隐藏，然后出现滚动条来让我们滚动查看所有的内容。
*   `hidden`: 当内容过多，溢流的内容被隐藏。
*   `visible`: 当内容过多，溢流的内容被显示在盒子的外边（这个是默认的行为）

该示例展示了这些设置是如何工作的：

首先是HTML代码：
```html
<p class="autoscroll">
   Lorem ipsum dolor sit amet, consectetur adipiscing elit.
   Mauris tempus turpis id ante mollis dignissim. Nam sed
   dolor non tortor lacinia lobortis id dapibus nunc. Praesent
   iaculis tincidunt augue. Integer efficitur sem eget risus
   cursus, ornare venenatis augue hendrerit. Praesent non elit
   metus. Morbi vel sodales ligula.
</p>

<p class="clipped">
   Lorem ipsum dolor sit amet, consectetur adipiscing elit.
   Mauris tempus turpis id ante mollis dignissim. Nam sed
   dolor non tortor lacinia lobortis id dapibus nunc. Praesent
   iaculis tincidunt augue. Integer efficitur sem eget risus
   cursus, ornare venenatis augue hendrerit. Praesent non elit
   metus. Morbi vel sodales ligula.
</p>

<p class="default">
   Lorem ipsum dolor sit amet, consectetur adipiscing elit.
   Mauris tempus turpis id ante mollis dignissim. Nam sed
   dolor non tortor lacinia lobortis id dapibus nunc. Praesent
   iaculis tincidunt augue. Integer efficitur sem eget risus
   cursus, ornare venenatis augue hendrerit. Praesent non elit
   metus. Morbi vel sodales ligula.
</p>
```
然后是应用到HTML的CSS代码：

```css
p {
  width  : 400px;
  height : 2.5em;
  padding: 1em 1em 1em 1em;
  border : 1px solid black;
}

.autoscroll { overflow: auto;    }
.clipped    { overflow: hidden;  }
.default    { overflow: visible; }
```
上边的代码给出了以下的结果：

![css-overflow]($res/css-overflow.jpg)

## 背景裁剪（Background clip）

框的背景是由颜色和图片组成的，它们堆叠在一起(`background-color`, `background-image`）。 它们被应用到一个盒子里，然后被画在盒子的下面。默认情况下，背景延伸到了边界外沿。这通常是OK的，但是在一些情况下比较讨厌（假使你有一个平铺的背景图，你只想要它延伸到内容的边沿会怎么做？），该行为可以通过设置盒子的`background-clip`属性来调整。

让我们通过一个示例来看看这个是怎么工作的。首先是我们的HTML代码：

```html
<div class="default"></div>
<div class="padding-box"></div>
<div class="content-box"></div>
```

然后是CSS代码:

```css
div {
  width  : 60px;
  height : 60px;
  border : 20px solid rgba(0, 0, 0, 0.5);
  padding: 20px;
  margin : 20px 0;

  background-size    : 140px;
  background-position: center;
  background-image   : url('https://mdn.mozillademos.org/files/11947/ff-logo.png');
  background-color   : gold;
}

.default     { background-clip: border-box;  }
.padding-box { background-clip: padding-box; }
.content-box { background-clip: content-box; }
```
上述代码产生以下结果：

![ccs-background-clip]($res/ccs-background-clip.jpg)

## 轮廓(Outline)

最后，还有重要的一点， 一个框的 [`outline`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/outline "CSS的outline属性是用来设置一个或多个单独的轮廓属性的简写属性 ， 例如 outline-style, outline-width 和 outline-color。 多数情况下，简写属性更加可取和便捷。") 是一个看起来像是边界但又不属于框模型的东西。它的行为和边界差不多，但是并不改变框的尺寸（更准确的说，轮廓被勾画于在框边界之外，外边距区域之内）

**Note**: 使用 outline 属性的时候要注意，它一般只在需要可用性的一些情况才被使用，例如在一些用户点击它的时候使用 outline 来表示高亮、可用，如果你要使用 outline，请确保不要因为它看起来像链接的高亮让用户迷惑。

## CSS 框类型

之前我们说的所有对于框的应用都表示的是块级元素的，然而，CSS还有一些表现不同的其他框类型。我们可以通过`display`属性来设定元素的框类型。`display`属性有很多的属性值。在本篇文章，我们将关注三个最常见的类型：`block`, `inline`, and `inline-block。`

*   块框（ `block` box）是定义为堆放在其他框上的框（例如：其内容会独占一行），而且可以设置它的宽高，之前所有对于框模型的应用适用于块框 （ `block` box）

*   行内框（ `inline` box）与块框是相反的，它随着文档的文字（例如：它将会和周围的文字和其他行内元素出现在同一行，而且它的内容会像一段中的文字一样随着文字部分的流动而打乱），对行内盒设置宽高无效，设置padding, margin 和 border都会更新周围文字的位置，但是对于周围的的块框（ `block` box）不会有影响。

*   行内块状框（`inline-block` box） 像是上述两种的集合：它不会重新另起一行但会像行内框（ `inline` box）一样随着周围文字而流动，而且他能够设置宽高，并且像块框一样保持了其块特性的完整性，它不会在段落行中断开。（在下面的示例中，行内块状框会放在第二行文本上，因为第一行没有足够的空间，并且不会突破两行。然而，如果没有足够的空间，行内框会在多条线上断裂，而它会失去一个框的形状。）

**注意：**默认状态下display属性值，块级元素`display: block` ，行内元素`display: inline`

这些东西在短时间听起来可能让你很混乱，现在让我们来看一些简单的小例子。

首先，HTML代码：

```html
<p>
   Lorem ipsum dolor sit amet, consectetur adipiscing elit.
   <span class="inline">Mauris tempus turpis id ante mollis dignissim.</span>
   Nam sed dolor non tortor lacinia lobortis id dapibus nunc.
</p>

<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit.
  <span class="block">Mauris tempus turpis id ante mollis dignissim.</span>
  Nam sed dolor non tortor lacinia lobortis id dapibus nunc.
</p>

<p>
  Lorem ipsum dolor sit amet, consectetur adipiscing elit.
  <span class="inline-block">Mauris tempus turpis id ante mollis dignissim.</span>
  Nam sed dolor non tortor lacinia lobortis id dapibus nunc.
</p>
```
现在我们加一些CSS:

```css
p {
  padding : 1em;
  border  : 1px solid black;
}

span {
  padding : 0.5em;
  border  : 1px solid green;

  /* That makes the box visible, regardless of its type */
  background-color: yellow;
}

.inline       { display: inline;       }
.block        { display: block;        }
.inline-block { display: inline-block; }
```
上面的代码给出了这个结果，说明了显示类型的不同效果：

![ccs-display-block]($res/ccs-display-block.jpg)

# 例：制作三角形盒子模型

HTML代码如下：
```html
<!--制作三角形-->
<div class="triangle"></div>
```
CSS代码：
```CSS
.triangle{
			width: 0;
			height: 0;
	
/*只定义盒子的下方和左右两边的border且让它们重合就生成了三角形*/
			border-bottom: 30px solid red;
			/*transparent 完全透明*/
			border-left: 30px solid transparent;
			border-right: 30px solid transparent;
		}

```
效果如图，当然你还可以更改color及三条边的长度来做出等腰三角形等：

![css-triangle]($res/css-triangle.jpg)

end
2018-06-01