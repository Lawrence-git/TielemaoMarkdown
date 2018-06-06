[[toc]]

以下从浮动到BFC的段落摘自[MDN 网络开发者](https://developer.mozilla.org/zh-CN)

# float 浮动

float CSS属性指定一个元素应沿其容器的左侧或右侧放置，允许文本和内联元素环绕它。该元素从网页的正常流动中移除，尽管仍然保持部分的流动性（与[绝对定位](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position#Absolute_positioning)相反）。

一个**浮动元素**是float的值不为none的元素。

```css
/* Keyword values */
float: left;
float: right;
float: none;
float: inline-start;
float: inline-end;

/* Global values */
float: inherit;
float: initial;
float: unset;
```
由于float意味着使用块布局，它在某些情况下修改[`display`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display "display CSS属性指定用于元素的呈现框的类型。在 HTML 中，默认的 display 属性取决于 HTML 规范所描述的行为或浏览器/用户的默认样式表。在 XML中，其默认值为 inline。") 值的计算值：

| 指定值 | 计算值 |
| --- | --- |
| `inline` | `block` |
| `inline-block` | `block` |
| `inline-table` | `table` |
| `table-row` | `block` |
| `table-row-group` | `block` |
| `table-column` | `block` |
| `table-column-group` | `block` |
| `table-cell` | `block` |
| `table-caption` | `block` |
| `table-header-group` | `block` |
| `table-footer-group` | `block` |
| `flex` | `flex`, 但是float对这样的元素不起作用 |
| `inline-flex` | `inline-flex`, 但是float对这样的元素不起作用 |
| _other_ | _unchanged_ |

**备注**：如果要在 JavaScript 中把float属性当作`element.style`对象的一个成员来操作，那么在Firefox 34 和更老的版本中，你必须拼写成cssFloat。另外还要注意到在 Internet Explorer 8 和更老的IE当中，要使用`styleFloat`属性。这是DOM驼峰命名和CSS所用的连字符分隔命名法对应关系中的一个特例（这是因为在 JavaScript 中"float"是一个保留字，因为同样的原因，"class"被改成了"className" 、"for"被改成了"htmlFor"）。

## 语法

`float` 属性的值被指定为单一的关键字，值从下面的值列表中选择。

### 值

`left`

表明元素必须浮动在其所在的块容器左侧的关键字。

`right`

表明元素必须浮动在其所在的块容器右侧的关键字。

`none`

表明元素不进行浮动的关键字。

`inline-start `

关键字，表明元素必须浮动在其所在块容器的开始一侧，在ltr脚本中是左侧，在rtl脚本中是右侧。

`inline-end `

关键字，表明元素必须浮动在其所在块容器的结束一侧，在ltr脚本中是右侧，在rtl脚本中是左侧。

### 形式化语法

left | right | none | inline-start | inline-end

例子

```html
<style type="text/css">
  div { border: solid red;  max-width: 70ex; }
  h4  { float: left;  margin: 0; }
</style>

<div>
  <h4>HELLO!</h4>
  This is some text. This is some text. This is some text.
  This is some text. This is some text. This is some text.
  This is some text. This is some text. This is some text.
  This is some text. This is some text. This is some text.
</div>
```
![css-float01]($res/css-float01.jpg)

## 浮动元素是如何定位的

正如我们前面提到的那样，当一个元素浮动之后，它会被移出正常的文档流，然后向左或者向右平移，一直平移直到碰到了所处的容器的边框，或者碰到**另外一个浮动的元素**。

在下面的图片中，有三个红色的正方形。其中有两个向左浮动，一个向右浮动。要注意到第二个向左浮动的正方形被放在第一个向左浮动的正方形的右边。如果还有更多的正方形这样浮动，它们会继续向右堆放，直到填满容器一整行，之后换行至下一行。

![css-float02]($res/css-float02.png)

## 清除浮动

在前面的例子当中，浮动的元素的高度比它们所在的容器元素（是块元素）的高度小。然而如果块元素内的文本太短，不足以把块元素的大小撑到高度大于所有浮动元素的高度，我们可能会看到意想不到的效果。例如，如果上面图片中的文字只有"Lorem ipsum dolor sit amet,"，并且接下来是另外一个和"Floats Example"这个标题一样风格的标题元素，那么第二个标题元素会出现在红色的正方形之间。然而在大多数这种情况下，我们希望这个标题元素是靠左对齐的。为了实现这个效果，我们需要清除浮动。

这个例子中，最简单的清除浮动方式就是给我们想要确保左对齐的新标题元素添加[`clear`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/clear "clear CSS 属性指定一个元素是否可以在它之前的浮动元素旁边，或者必须向下移动(清除浮动) 在它的下面。clear 属性适用于浮动和非浮动元素。")属性：

```css
h2.secondHeading { clear: both; }
```

然而这个方法只是在同一[块级格式化上下文（block formatting context）](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)中没有其他元素的时候才是有效的。如果我们的 `H2` 有兄弟元素是向左浮动和向右浮动的边栏，那么使用`clear` 会导致这个标题元素出现在边栏的下方，这显然不是我们期望的结果。

如果不能使用清除浮动，另一种做法是浮动容器的限制块级格式化上下文。再次列举上面的例子，有三个红色的正方形和一个`P`元素。我们可以设置`P`元素的[`overflow`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/overflow "overflow 属性指定当内容溢出其块级容器时,是否剪辑内容，显示滚动条或显示溢出的内容。")属性值变成`hidden`或者`auto`，因为这样可以让容器元素伸展到能够包含红色正方形，而不是让他们超出块元素的底部。

```css
p.withRedBoxes { overflow: hidden; height: auto; }
```

**Note:** 设置`overflow`为`scroll` 也可以让块元素撑大来包含所有的浮动子元素，但是这样不论内容有多少都会显示出一个滚动条。即使 `height` 默认值为 `auto`，我们还是设置了，是为了表明容器应该增大高度以便包裹住里面的内容。

# clear 清除浮动

 **`clear`** [CSS](https://developer.mozilla.org/en-US/docs/CSS "CSS") 属性指定一个元素是否可以在它之前的浮动元素旁边，或者必须向下移动(清除浮动) 在它的下面。clear 属性适用于浮动和非浮动元素。

```
clear: none;
clear: left;
clear: right;
clear: both;
clear: inline-start;
clear: inline-end;

clear: inherit;
```

当应用于非浮动块时，它将非浮动块的[边框边界](https://developer.mozilla.org/en-US/docs/CSS/box_model "CSS/box_model")移动到所有相关浮动元素[外边界](https://developer.mozilla.org/en-US/docs/CSS/box_model "CSS/box_model")的下方。这个行为作用时会导致[margin collapsing](https://developer.mozilla.org/en-US/docs/CSS/margin_collapsing "CSS/margin_collapsing")不起作用。

当应用于浮动元素时，它将元素的[外边界](https://developer.mozilla.org/en-US/docs/CSS/box_model "CSS/box_model")移动到所有相关的浮动元素[外边界](https://developer.mozilla.org/en-US/docs/CSS/box_model "CSS/box_model")的下方。这会影响后面浮动元素的布局，后面的浮动元素的位置无法高于它之前的元素。

要被清除的相关浮动元素指 在相同[块级格式化上下文](https://developer.mozilla.org/en-US/docs/CSS/block_formatting_context "CSS/block_formatting_context")中的前置浮动。

**注释**:如果你想要一个元素将所有浮动元素包含在内，你既可以将这个容器设置为浮动，又可以通过 [`::after`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::after "CSS伪元素::after用来创建一个伪元素，做为已选中元素的最后一个子元素。通常会配合content属性来为该元素添加装饰内容。这个虚拟元素默认是行内元素。") [伪元素](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)`设置clear属性`作为替代。

```html
/* best practical*/
/* old */
.clearfix:before, .clearfix:after{
    overflow: hidden;
    display: table;
    visibility: hidden;
    content: '';
    clear: both;
}

/* new */
.clearfix::before, .clearfix::after{
    overflow: hidden;
    display: table;
    visibility: hidden;
    content: '';
    clear: both;
}

/* new clearfix */
.clearfix::after {
    content: "";
    display: table;
    clear: both;
    overflow: hidden;
    visibility: hidden;
}

/* old clearfix */
.clearfix:after {
    content: "";
    display: block;
    clear: both;
}
```
## 语法

```html
clear: none
clear: left
clear: right
clear: both

clear: inherit
```

### 值

`none`

元素不会向下移动清除之前的浮动。

`left`

元素被向下移动用于清除之前的**左**浮动。

`right`

元素被向下移动用于清除之前的**右**浮动。

`both`

元素被向下移动用于清除之前的**左右**浮动。

`inline-start` 

`inline-start是一个关键字，`表示`该元素向下移动以清除其包含块的起始侧上的浮动。即在某个区域的左侧浮动或右侧浮动。`

`inline-end` 

`inline-end`是一个关键字，表示该元素向下移动以清除其包含块的末端的浮点，`即在某个区域的右侧浮动或左侧浮动。

## 示例

**注意**: 给div添加'wrapper'类，使其添加一个边框，以便更好观察到实体的此属性被清除。

HTML Content

```html
<div class="wrapper">

    <p class="black">Lorem ipsum dolor sit amet, consectetuer adipiscing elit. 
      Phasellus sit amet diam. Duis mattis varius dui. Suspendisse eget dolor.</p>

    <p class="red">Lorem ipsum dolor sit amet, consectetuer adipiscing elit.</p>

    <p class="left">This paragraph clears left.</p>

</div>
```
CSS Content

```css
.wrapper{
    border:1px solid black;
    padding:10px;
}
.left {
    border: 1px solid black;
    clear: left;
}
.black {
    float: left;
    margin: 0;
    background-color: black;
    color: #fff;
    width: 20%;
}
.red {
    float: left;
    margin: 0;
    background-color: red;
    width:20%;
}
p {
    width: 50%;
}
```
![css-clear-left]($res/css-clear-left.jpg)

如果上例中，样式中的float: left 都改为了float：right的话，则相应clear： right。
而如果左浮动和右浮动都有的样式，则clear: both。（both：两者）

# BFC 块格式上下文
**块格式化上下文（Block Formatting Context，BFC）** 是Web页面的可视化CSS渲染的一部分，是布局过程中生成块级盒子的区域，也是浮动元素与其他元素的交互限定区域。

下列方式会创建**块格式化上下文**：

*   根元素或包含根元素的元素。

*   浮动元素（元素的 `float`值不是 `none`）。

*   绝对定位元素（元素的 `position`（position属性用于指定一个元素在文档中的定位方式。top，right，bottom 和 left 属性则决定了该元素的最终位置。) 为 `absolute` （绝对定位）或 `fixed`（固定定位）。

*   行内块元素（元素的 `display` 为 `inline-block`。

*   表格单元格（元素的 `display`为 `table-cell`，HTML表格单元格默认为该值）

*   表格标题（元素的 `display`为 `table-caption`，HTML表格标题默认为该值）

*   匿名表格单元格元素（元素的 `display`为 `table、``table-row`、 `table-row-group、``table-header-group、``table-footer-group`（分别是HTML table、row、tbody、thead、tfoot的默认属性）或 `inline-table`）

*   `overflow`值不为 `visible` 的块元素。

*   `display`值为 `flow-root` 的元素。

*   `contain` 值为 `layout`、`content`或 `strict` 的元素。

*   弹性元素（`display`为 `flex` 或 `inline-flex`元素的直接子元素）

*   网格元素（`display`为 `grid` 或 `inline-grid` 元素的直接子元素）

*   多列容器（元素的 `column-count`或 `column-width` 不为 `auto，包括 ``column-count` 为 `1`）

*   `column-span` 为 `all` 的元素始终会创建一个新的BFC，即使该元素没有包裹在一个多列容器中( [标准变更](https://github.com/w3c/csswg-drafts/commit/a8634b96900279916bd6c505fda88dda71d8ec51)，[Chrome bug](https://bugs.chromium.org/p/chromium/issues/detail?id=709362)）。

创建了块格式化上下文的元素中的所有内容都会被包含到该BFC中。

`块格式化上下文`对`浮动定位`与`清除浮动`都很重要。浮动定位和清除浮动时只会应用于同一个BFC内的元素。浮动不会影响其它BFC中元素的布局，而清除浮动只能清除同一BFC中在它前面的元素的浮动。`外边距折叠`（[Margin collapsing](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing)）也只会发生在属于同一BFC的块级元素之间。

以下开始，就不是摘自MDN，而是换成摘自博客园[小马哥&](http://www.cnblogs.com/majj/)的相关博文，再加以整理和添加自己的一点思路而成。

# 行内元素和块级元素

| 级别 | 元素 |
| --- | --- |
| 行内元素 | a,b,strong,span,img,label,button,input,select,textarea|
| 块级元素|header,form,ul,ol,table,article,div,hr,aside,figure,canvas,video,audio,footer|

* 标签分为行内元素与块级元素
* 两者区别和特点：
	* 行内元素能与其它行内元素并排共处一行，而块级元素独占一行；
	* 行内元素设置width无效，height无效（可以设置line-height)，margin和padding在上下方向的设置无效；
	* 行内元素适合显示具体内容，而块级元素适合做布局；
	* 行内元素一般是内容的容器，而块级元素一般是其他容器的容器；
* 行内元素转化为块级元素，使用display：block
* 块级元素转化为行内元素，使用display：inline

# 标准文档流和脱离标准流

web网页的制作，就像是一个“流”，从上而下，像“织毛衣”。
标准文档流下的微观现象：

* 空白折叠现象
	* 多个空格会被合并成一个空格显示到浏览器页面中。
* 高矮不齐，底边对齐
	* 文字还有图片大小不一，都会让我们页面的元素出现高矮不齐的现象，但是在浏览器查看我们的页面总会发现底边对齐。
* 自动换行，一行写不满，换行写
	* 如果在一行内写文字，文字过多，那么浏览器会自动换行去显示我们的文字。

标准流里面的限制非常多，导致很多页面效果无法实现。如果我们现在就要并排、并且就要设置宽高，那该怎么办呢？办法是：移民！**脱离标准流**！

css中一共有三种手段，使一个元素脱离标准文档流：

*   （1）浮动
*   （2）绝对定位
*   （3）固定定位

# 总结

## 浮动四大特性

* 浮动的元素脱标
	* 脱离标准流，漂浮，遮盖标准流下的元素
* 浮动的元素互相贴靠
	* 没有足够空间互相贴靠时，往边靠
* 浮动的元素有“字围”（文字围绕）效果
	* 所谓字围效果，当div浮动，p不浮动，div遮盖住了p，div的层级提高，但是p中的文字不会被遮盖，此时就形成了字围效果。
* 浮动元素紧凑效果（收缩）
	* 一个浮动元素。如果没有设置width，那么就自动收缩为文字的宽度（这点跟行内元素很像）

## 布局浮动的原则

按布局来说，一般都不会是一个盒子单独浮动起来，往往是横向区域的盒子一起浮动。且为了不影响接下来的文档标准流，往往也要收尾做出清除浮动。

## 为什么要清除浮动？

在页面布局的时候，每个结构中的父元素的高度，我们一般不会设置。（为什么？）

试想想，如果我第一版的页面的写完了，感觉非常爽，突然隔了一个月，产品经理说页面某一块的区域，需要加点内容，或者觉得图片要缩小一下。这样的需求在工作中非常常见的。真想打他啊。那么此时作为一个前端小白，肯定是去每个地方加内容，改图片，然后修改父盒子的高度。那问题来了，这样不影响开发效率吗？答案是肯定的。

所以父盒子我们一般不设置高度，而由子元素的内容去撑起父盒子的高度。那么当这个子元素是浮动的时候，父盒子没有高度，浮动子元素是不会填充父盒子的高度的，这个时候就会有下一栏的盒子跑过来到浮动子元素的下面被覆盖摭挡了，非常影响页面布局。

浮动元素确实能实现我们页面元素并排的效果，这是它的好处，但同时它还带来了页面布局极大的错乱！！！所以我们要在浮动完之后再做一步清除浮动的操作（并不影响浮动子元素之前的布局，只是为了下排的布局不乱。）

## 清除浮动的方法

* 给父盒子设置高度（不推荐，除非万年不变的导航栏之类）
* clear:both（左右浮动均可清除）
	* 给浮动元素的后面加一个空的div，并且该元素不浮动，然后设置clear：both
	* 又被称为内墙法，无缘无故添加了div元素，结构冗余
* 伪元素清除法（常用）
	* 给浮动子元素的父盒子，也就是不浮动的元素，添加一个clearfix的类，然后设置。
```css
.clearfix:after{
    /*必须要写这三句话*/ 
	content: '.'; 
	clear: both; 
	display: block;
}
```
* overflow：hidden（常用加推荐）
	* overflow：auto也可以清除浮动，但auto会出现滚动条，所以还是使用不影响布局的hidden，推荐使用这种方法，原理大致是借助了BFC（块格式上下文）实现的。

| 值 | 描述 |
|---|---|
| visible | 默认值。内容不会被修剪，会呈现在元素框之外。 |
| hidden | 内容会被修剪，并且其余内容是不可见的。 |
| scroll | 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容。 |
| auto | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |
| inherit | 规定应该从父元素继承 overflow 属性的值。 |

逐渐演变成overflow：hidden清除法。其实它是一个BFC区域。

## BFC常见作用
* 包含浮动元素，解决高度塌陷和清除浮动；
* 不被浮动元素覆盖（比如文字围绕效果）；
*  阻止外边距折叠（margin塌陷）
	* 在标准文档流中，块级标签之间竖直方向的margin会以大的为准，这就是margin的塌陷现象。可以用overflow：hidden产生bfc来解决。

end
2018-06-01