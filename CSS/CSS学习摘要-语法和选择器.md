CSS学习摘要-语法和选择器
===
主要摘自[网络开发者](https://developer.mozilla.org)。

[[toc]]

从最基本的层次来看，CSS是由两块内容组合而成的：

*   **属性（Property）**：一些人类可理解的标识符，这些标识符指出你想修改哪一些样式，例如：字体，宽度，背景颜色等。
*   **属性值（Value）**：每个指定的属性都需要给定一个值，这个值表示你想把那些样式特征修改成什么样，例如，你想把字体，宽度或背景颜色改成什么。

与值配对的属性被称为[CSS声明]，CSS声明会被放置在一个[CSS声明块]中。最后，CSS声明块与选择器相结合形成一个CSS规则集（或CSS规则）。

## CSS 声明

给 CSS 属性设置特定的值是 CSS 语言的核心功能。
CSS 引擎会通过计算，将对应的 CSS 声明应用到页面的每一个元素上，从而使得元素们以适当的方式布局，并展示出适当的样式。

特别需要记住的是：
* CSS 的属性和属性值都是区分大小写的。
* 属性和属性值之间，用英文半角冒号 (`:`) 隔离。

如下图所示：

![css-declaration]($res/css-declaration.png)

CSS 有超过[300 个不同的属性](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference)以及几乎无穷无尽的属性值。属性和属性值不能任意组合：每个属性都有一个已经定义好的可用属性值范围。 

**重要**: 如果使用了未知属性，或者给属性赋予了无效值，该声明会被视为无效，浏览器的 CSS 引擎会完全忽略它。

**重要**: 在 CSS（和其他网络标准）中，使用美式拼写作为单词的标准写法。例如，颜色（见于上述代码所见）应始终拼写为 `color`。写成 `colour `会无法正常工作。

## CSS 声明块

声明按**块**分组，每一组声明都用一对大括号包裹，用 (`{`) 开始，用 (`}`) `结束`。

**声明块**里的每一个声明必须用半角分号（`;`）分隔，否则代码会不生效（至少不会按预期结果生效）。声明块里的最后一个声明结束的地方，不需要加分号，但是最后加分号是个好习惯，因为可以防止在后续增加声明时忘记加分号。

![css-declarations-block]($res/css-declarations-block.png)

**注意**：块有时候是可以嵌套的。这种情况下，每一对括号必须逻辑上嵌套，跟嵌套 HTML 元素的标签嵌套方式相同。最常见的例子是 @-rules，这是一种用 @ 标识开头的块。

例如 ：

[`@media`](@media CSS @规则 可用于根据一个或多个基于设备类型、具体特点和环境的媒体查询来应用样式。) 

[`@font-face`](@font-face CSS @规则 ，它允许网页开发者为其网页指定在线字体。 通过这种作者自备字体的方式，@font-face 可以消除对用户电脑字体的依赖。 @font-face 不仅可以放在在CSS的最顶层, 也可以放在 @规则 的 条件规则组 中。)等。

**注意**：声明块的内容允许为空 —— 这完全有效。

## CSS 选择器和规则

我们的拼图还少了一块——我们需要讨论一下如何告知我们的声明块：哪些元素是它们需要应用的。通过在每个声明块前加上**选择器（selector）**来完成这一动作，选择器是一种模式，它能在页面上匹配一些元素**。**这将使相关的声明仅被应用到被选择的元素上。选择器加上声明块被称为**规则集（ruleset**），通常简称**规则**（**rule**）。

![css-ruleset]($res/css-ruleset.png)

选择器可以很复杂 —— 你可以制作一个匹配多种元素的规则，这是通过把多个选择器囊括成用逗号分隔的选择器（一组,）来达成；选择器还可以被“链”在一起，例如我要选择所有 class 是 "blah" 的元素, 但是当且仅当它在 `<article>` 元素里、并且仅当鼠标指针悬停在这个元素上的时候。别担心：随着你在CSS上的经验变的更丰富，事情会变的更清晰。

一个元素可以被多个选择器所匹配，因此，一个给定的属性可能被多个规则设置多次。 CSS 定义了哪个规则比其它规则更具优先级，则更具优先级的规则必定被应用：这被称为**层叠算法（cascade algorithm）**，关于层叠算法的更多内容和运作原理见[层叠和继承](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Introduction_to_CSS/Cascade_and_inheritance)。

**重要**：如果链或组中的某个选择器无效，比如使用了未知的伪元素或伪类，整个组的选择器仍然是有效的，除了这个无效的将被忽略的选择器。

## CSS 语句（CSS statements）

CSS 规则是样式表的主要组成块 —— 是你在 CSS 中最常见的块。但这有一些其它类型的块，你以后偶尔会碰上 —— CSS 规则只是被称为 CSS 语句中的一种。其它类型如下：

*   @-规则(At-rules) 在CSS中被用来传递元数据、条件信息或其它描述性信息。它由（`@`）符号开始，紧跟着一个表明它是哪种规则的描述符，之后是这种规则的语法块，并最终由一个半角分号（`;`）结束。每种由描述符定义的[@-规则](https://developer.mozilla.org/zh-CN/docs/Web/CSS/At-rule)，都有其特有的内部语法和语义。一些例子如下：

    *   [`@charset`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@charset "@charset CSS @规则  指定样式表中使用的字符编码。它必须是样式表中的第一个元素，而前面不得有任何字符。因为它不是一个嵌套语句，所以不能在@规则条件组中使用。如果有多个 @charset @规则被声明，只有第一个会被使用，而且不能在HTML元素或HTML页面的字符集相关 <style> 元素内的样式属性内使用。") 和 [`@import`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@import "@import CSS@规则，用于从其他样式表导入样式规则。这些规则必须先于所有其他类型的规则，@charset 规则除外; 因为它不是一个嵌套语句，@import不能在条件组的规则中使用。") （元数据）
    *   [`@media`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media "@media CSS @规则 可用于根据一个或多个基于设备类型、具体特点和环境的媒体查询来应用样式。") 或 `@document`（条件信息，又被称为嵌套语句，见下方。)
    *   [`@font-face`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face "这是一个叫做@font-face 的CSS @规则 ，它允许网页开发者为其网页指定在线字体。 通过这种作者自备字体的方式，@font-face 可以消除对用户电脑字体的依赖。 @font-face 不仅可以放在在CSS的最顶层, 也可以放在 @规则 的 条件规则组 中。") （描述性信息）具体语法示例：

    ```
    @import 'custom.css';
    ```
    该@-规则向当前 CSS 导入其它 CSS 文件
*   **嵌套语句** 是@-规则中的一种，它的语法是 CSS 规则的嵌套块，只有在特定条件匹配时才会应用到文档上。特定条件如下：
    *   [`@media`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@media "@media CSS @规则 可用于根据一个或多个基于设备类型、具体特点和环境的媒体查询来应用样式。") 只有在运行浏览器的设备匹配其表达条件时才会应用该@-规则的内容；
    * `@supports`关联了一组嵌套的CSS语句,这些语句被放置在一个CSS区块中,该区块以大括号分割, 还有一个由多个CSS声明检测组成的条件,它是一个键值组合, 由逻辑与,逻辑或,逻辑非组合而成. 这样的条件语句称为支持条件. 只有浏览器确实支持被测功能时才会应用该@-规则的内容；
    * `@document`只有当前页面匹配一些条件时才会应用该@-规则的内容。

具体语法示例

```html
@media (min-width: 801px) {
  body {
    margin: 0 auto;
    width: 800px;
  }
}
```

上述的嵌套语句只有在页面宽度超过801像素时才会应用。

**重要**：任何不是规则集或@-规则或嵌套语句的 CSS 语句都是无效的，并会因此被忽略。

## 语法之外：使 CSS 更具可读性

正如你所见的，CSS 语法并不难写：如果你写了一些错误的规则，它将仅被忽略。但是，即使有这样的机制，这也有一些值得了解的最佳实践，使您的CSS代码更易于使用和维护。

### 空格

空格实际上意味着一些空格，一些制表符以及一些新行。你可以通过添加空格使你的样式表更具可读性。

用与处理 HTML 的方式类似，浏览器会倾向于忽略你 CSS 中的许多空格；许多空格在那的意义仅仅是增加了可读性。在下面的第一个例子中，我们的每个声明（以及规则的开始 / 结束）都在自己各自的行上 —— 这可以说是编写 CSS 的好方法，因为它很容易维护和理解：

```html
body {
  font: 1em/150% Helvetica, Arial, sans-serif;
  padding: 1em;
  margin: 0 auto;
  max-width: 33em;
}

@media (min-width: 70em) {
  body {
    font-size: 130%;
  }
}

h1 {
  font-size: 1.5em;
}

div p, #id:first-line {
  background-color: red;
  background-style: none
}

div p {
  margin: 0;
  padding: 1em;
}

div p + p {
  padding-top: 0;
}
```

你可以编写跟这完全一样的CSS，并移除大部分空格，它在功能上和第一个例子完全相同，但我确信你会同意这样有些难读：

```html
body {font: 1em/150% Helvetica, Arial, sans-serif; padding: 1em; margin: 0 auto; max-width: 33em;}
@media (min-width: 70em) { body {font-size: 130%;} }

h1 {font-size: 1.5em;}

div p, #id:first-line {background-color: red; background-style: none}
div p {margin: 0; padding: 1em;}
div p + p {padding-top: 0;}
```

你选择的代码布局通常依据个人喜好，但当你开始在团队中工作时，你可能会发现团队已有自己的风格指南，约定了代码编写规范。

您在CSS中需要注意的空格是属性和属性值边上的空格。例如，以下是有效的CSS：

```html
margin: 0 auto;
padding-left: 10px;
```

但以下是无效的：

```html
margin: 0auto;
padding- left: 10px;
```

因为 `0auto` 不被解析为 margin 属性的有效值（`0`和`auto`是两个单独的值），并且浏览器不会将 `padding-` 解析为有效属性。因此，你应该始终确保至少用一个空格分隔不同的值，且保持属性名/值为一个连续的字符串。

### 注释

与HTML一样，我们鼓励您在CSS中使用注释，以帮助您在几个月后遇上代码时了解代码如何工作，并帮助他人理解该代码。它同样对暂时注释某些部分的代码进行测试很有用，例如当您尝试查找是代码的哪一部分导致错误时。

CSS中的注释以 `/*` 开始并以 `*/` 结束。

```html
/* 基本元素样式 */
/* -------------------------------------------------------------------------------------------- */
body {font: 1em/150% Helvetica, Arial, sans-serif; padding: 1em; margin: 0 auto; max-width: 33em;}
@media (min-width: 70em) {
  /* 特别指明全局字体大小，在大屏或大窗口下加大字体大小以增加可读性 */
  body {font-size: 130%;}
}

h1 {font-size: 1.5em;}

/* 处理嵌套在DOM中的特定元素  */
/* -------------------------------------------------------------------------------------------- */
div p, #id:first-line {background-color: red; background-style: none}
div p                 {margin          :   0; padding         : 1em;}
div p + p             {padding-top     :   0;                       }
```

### 简写

一些属性比如 [`font`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/font "font 属性是设置 font-style, font-variant, font-weight, font-size, line-height 和 font-family属性的简写，或使用特定的关键字设置元素的字体为某个系统字体。")，[`background`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/background "background 是CSS简写属性，用来集中设置各种背景属性。background 可以用来设置一个或多个属性:background-color, background-image, background-position, background-repeat, background-size, background-attachment。")，[`padding`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/padding "padding属性设置一个元素的内边距，padding 区域指一个元素的内容和其边界之间的空间，该属性不能为负值。")，[`border`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/border "CSS的border属性是一个用于设置各种单独的边界属性的简写属性。border可以用于设置一个或多个以下属性的值： border-width, border-style, border-color。")，和 [`margin`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/margin "margin属性为给定元素设置所有四个（上下左右）方向的外边距属性。这是四个外边距属性设置的简写。四个外边距属性设置分别是： margin-top， margin-right， margin-bottom 和 margin-left 。指定的外边距允许为负数。") 被称为**简写属性** —— 这是由于它们允许你在一行设置多个属性，从而节省时间并使代码更整洁。

例如，像这行代码：

```html
/* 在padding和margin这样的简写属性中，值赋值的顺序是top、right、bottom、left。 
   它们还有其他简写方式，例如给padding两个值，则第一个值表示top/bottom，第二个值表示left/right */
padding: 10px 15px 15px 5px;
```

和以下的所有代码做了相同的工作：

```html
padding-top: 10px;
padding-right: 15px;
padding-bottom: 15px;
padding-left: 5px;
```

而这一行：

```html
background: red url(bg-graphic.png) 10px 10px repeat-x fixed;
```

和以下这些做了相同的事：

```html
background-color: red;
background-image: url(bg-graphic.png);
background-position: 10px 10px;
background-repeat: repeat-x;
background-scroll: fixed;
```

我们不会试图彻底全面地教导你这些东西 —— 你会在后续的课程中遇到很多例子，建议你通过简写属性的名称在我们的 [CSS参考](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Reference) 中查找以了解更多信息。

## 深入选择器
在CSS中，选择器用于定位我们想要样式化的网页HTML元素。各种各样可用的CSS选择器允许我们精确选择要样式化的元素。

### 不同种类的CSS选择器:

选择器可以被分为以下类别：

*   **简单选择器（Simple selectors)** ：通过元素类型、`class` 或 `id` 匹配一个或多个元素。
*   **属性选择器（Attribute selectors）**：通过 属性 / 属性值 匹配一个或多个元素。
*   **伪类（Pseudo-classes）** ：匹配处于确定状态的一个或多个元素，比如被鼠标指针悬停的元素，或当前被选中或未选中的复选框，或元素是DOM树中一父节点的第一个子节点。
*   **伪元素（Pseudo-elements）**:匹配处于相关的确定位置的一个或多个元素，例如每个段落的第一个字，或者某个元素之前生成的内容。 
*   **组合器（Combinators）**：这里不仅仅是选择器本身，还有以有效的方式组合两个或更多的选择器用于非常特定的选择的方法。例如，你可以只选择divs的直系子节点的段落，或者直接跟在headings后面的段落。
*   **多用选择器（Multiple selectors）**：这些也不是单独的选择器；这个思路是将以逗号分隔开的多个选择器放在一个CSS规则下面， 以将一组声明应用于由这些选择器选择的所有元素。

## 简单选择器

“简单”选择器，之所以这么称呼它是因为它们基于元素的类型（或其 `class`或`id`）直接匹配文档的一个或多个元素。

### 类型选择器（又名元素/标签选择器）
此选择器只是一个选择器名和指定的HTML元素名的不区分大小写的匹配。这是选择所有指定类型的最简单方式。让我们一起看看下面这个例子:

这是HTML:

```html
<p>What color do you like?</p>
<div>I like blue.</div>
<p>I prefer red!</p>
```

这是样式表:

```css
/* All p elements are red */
p {
  color: red;
}

/* All div elements are blue */
div {
  color: blue;
}
```

我们将得到这个：

![css-select01]($res/css-select01.jpg)

### 类选择器

类选择器由一个点“.”以及类后面的类名组成。类名是在HTML `class`文档元素属性中没有空格的任何值。由你自己选择一个名字。同样值得一提的是，文档中的多个元素可以具有相同的类名，而单个元素可以有多个类名(以空格分开多个类名的形式书写)。以下是一个简单的例子：

这是一些HTML：

```html
<ul>
  <li class="first done">Create an HTML document</li>
  <li class="second done">Create a CSS style sheet</li>
  <li class="third">Link them all together</li>
</ul>
```

这是一些CSS样式:

```css
/* The element with the class "first" is bolded */
.first {
  font-weight: bold;
}

/* All the elements with the class "done" are strike through */
.done {
  text-decoration: line-through;
}
```

我们得到这样一个结果:

![css-select02]($res/css-select02.jpg)

例：处理多个类

html代码：

```html
<p class="base-box warning important">My first paragraph.</p>

<p class="base-box editor-note">My second paragraph.</p>

<p class="base-box ">My third paragraph</p>
```
css样式：

```css
.base-box {
  background-image: linear-gradient(to bottom, rgba(0,0,0,0.1), rgba(0,0,0,0.3));
  padding: 3px 3px 3px 7px;
}

.important {
  font-weight: bold;
}

.editor-note {
  background-color: #9999ff;
  border-left: 6px solid #333399;
}

.warning {
  background-color: #ff9999;
  border-left: 6px solid #993333;
}
```
页面效果：

![css-select03]($res/css-select03.jpg)

### ID 选择器

ID选择器由哈希/磅符号 (`#`)组成，后面是给定元素的ID名称。 任何元素都可以使用`id`属性设置唯一的ID名称。 由你自己选择的ID是什么。 这是选择单个元素的最有效的方式。

**重要提示**：一个ID名称必须在文件中是唯一的。关于重复ID的行为是不可预测的，比如在一些浏览器只是第一个实例计算，其余的将被忽略。

我们来看一个简单的例子 - 这是HTML：

```html
<p id="polite"> — "Good morning."</p>
<p id="rude"> — "Go away!"</p>
```

这是样式表：

```css
#polite {
  font-family: cursive;
}

#rude {
  font-family: monospace;
  text-transform: uppercase;
}
```

而我们得到这个作为输出：

![css-select04]($res/css-select04.jpg)

例：冠亚季军对应金银铜三色

基于相关ID的CSS选择器规则，使冠军、亚军、季军显示对应的金、银、铜三种颜色。

html代码：

```html
<p id="first"><strong>Winner</strong>: Velma Victory</p>
<p id="second"><strong>2nd</strong>: Colin Contender</p>
<p id="third"><strong>3rd</strong>: Phoebe Player</p>
```

css样式：

```css
p {
  text-transform: uppercase;
  padding: 5px;
}

#first {
  background-color: goldenrod;
}

#second {
  background-color: silver;
}

#third {
  background-color: #CD7F32;
}

```
页面效果如图：

![css-select05]($res/css-select05.jpg)

### 通用选择器
通用选择（`*`）是最终的王牌。它允许选择在一个页面中的所有元素。由于给每个元素应用同样的规则几乎没有什么实际价值，更常见的做法是与其他选择器结合使用(参考下面 组合）

**重要提示**：使用通用选择时小心。因为它适用于所有的元素，在大型网页利用它可以对性能有明显的影响：网页可以显示比预期要慢。大多数情况下，你都不会使用这个选择器。

例如，这是HTML：

```html
<div>
  <p>I think the containing box just needed
  a <strong>border</strong> or <em>something</em>,
  but this is getting <strong>out of hand</strong>!</p>
</div>
```

这是样式表：

```css
* {
  padding: 5px;
  border: 1px solid black;
  background: rgba(255,0,0,0.25)
}
```
组合在一起，会得到这样的结果：

![css-select06]($res/css-select06.jpg)

另：通用（通配符`*`）选择器还常见于用来消除默认样式，一 般是消除默认的外边距和内边距。因为默认样式比较丑且有可能影响到css后的一些布局。

如以下css代码也不算少见:

```css
* {
  margin: 0;
  padding: 0:
}
```


### 简单（基本）选择器总结

简单（基本）选择器包含：

* 1.元素（标签）选择器（共性）
	* 元素选择器可以选中所有的标签元素，比如div，ul，li ，p等等，不管标签藏的多深，都能选中，而且选中的是页面上所有的，而不是单独的某一个，所以说它具有的是 "共性" 而不是 ”特性“。

* 2.ID选择器（#）
	* 同一个页面中id不要有重复（规范）；
	* 任何的标签元素都可以额外设置ID；
	* ID命名要规范（字母，数字，下划线），大小写敏感（严格区分大小写）；

* 3.类选择器（class）
	* 所谓类就是class，class与id非常相似；
	* 同样是任何的标签元素都可以添加上类名，不同于ID是不同标识，class是可以重复相同的命名；
	* 相同的类名，用于替元素，对象归类；
	* 同一个标签中可以携带多个类，分别用空格隔开。
	* 每个类尽可能的小，有公共（共有属性）的概念，能够让更多的标签使用上，减少重复代码（css样式）；
	*  玩好了类，等于玩好了2分之1css。
	*  相比使用id，要尽可能的使用class，因为id一般是用于Js当选择的，而class用于css当选择居多。

## 属性选择器

属性选择器是一种特殊类型的选择器，它根据元素的 `属性`和`属性值`来匹配元素。它们的通用语法由方括号(`[]`) 组成，其中包含属性名称，后跟可选条件以匹配属性的值。 属性选择器可以根据其匹配属性值的方式分为两类： 存在和值属性选择器和子串值属性选择器。


### 存在和值（Presence and value）属性选择器

这些属性选择器尝试匹配精确的属性值：

*   `[attr]`：该选择器选择包含 attr 属性的所有元素，不论 attr 的值为何。
*   `[attr=val]`：该选择器仅选择 attr 属性被赋值为 val 的所有元素。
*   `[attr~=val]`：该选择器仅选择 attr 属性的值（以空格间隔出多个值）中有包含 val 值的所有元素，比如位于被空格分隔的多个类（class）中的一个类。

让我们看一个以下面的HTML代码片段为例：

```html
我的食谱配料: <i lang="fr-FR">Poulet basquaise</i>
<ul>
  <li data-quantity="1kg" data-vegetable>Tomatoes</li>
  <li data-quantity="3" data-vegetable>Onions</li>
  <li data-quantity="3" data-vegetable>Garlic</li>
  <li data-quantity="700g" data-vegetable="not spicy like chili">Red pepper</li>
  <li data-quantity="2kg" data-meat>Chicken</li>
  <li data-quantity="optional 150g" data-meat>Bacon bits</li>
  <li data-quantity="optional 10ml" data-vegetable="liquid">Olive oil</li>
  <li data-quantity="25cl" data-vegetable="liquid">White wine</li>
</ul>
```
和一个简单的样式表：

```css
/* 
    具有"data-vegetable"属性的所有元素,
    将被给予绿色的文本颜色
*/
[data-vegetable] {
  color: green
}

/* 
    具有"data-vegetable"属性且属性值刚好是"liquid"的所有元素，
    将被给予金色背景颜色 
*/
[data-vegetable="liquid"] {
  background-color: goldenrod;
}

/* 
    具有"data-vegetable"属性且属性值包含"spicy"的所有元素，
    即使某元素的该属性还包含其他属性值，
    都会被给予红色的文本颜色 
*/
[data-vegetable~="spicy"] {
  color: red;
}
```
结果如下：

![css-select07]($res/css-select07.jpg)


**笔记**：本例中的 `data-*` 属性被称为 [数据属性](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Global_attributes/data-*)。它们提供了一种在HTML属性中存储自定义数据的方法，由此，这些数据可以轻松地被提取和使用。有关详细信息，请参阅 [如何使用数据属性](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/Using_data_attributes)。


### 子串值（Substring value）属性选择器

这种情况的属性选择器也被称为“伪正则选择器”，因为它们提供类似 [regular expression](https://developer.mozilla.org/en-US/docs/Glossary/regular_expression "regular expression: Regular expressions (or regex) are rules that govern which sequences of characters come up in a search.") 的灵活匹配方式（但请注意，这些选择器并不是真正的正则表达式）：

*   `[attr|=val]` : 选择attr属性的值是 val 或值以 val- 开头的元素（-用来处理语言编码）。
*   `[attr^=val]` : 选择attr属性的值以val开头（包括 val）的元素。
*   `[attr$=val]` : 选择attr属性的值以val结尾（包括 val）的元素。
*   `[attr*=val]` : 选择attr属性的值中包含字符串 val 的元素。

让我们继续我们前面的例子，并添加以下CSS规则：

```css
/* 语言选择的经典用法 */
[lang|="fr"] {
  font-weight: bold;
}

/* 
    具有"data-vegetable"属性含有值"not spicy"的所有元素,都变回绿色
*/
[data-vegetable*="not spicy"] {
  color: green;
}

/* 
   具有"data-quantity"属性其值以"kg"结尾的所有元素*/
[data-quantity$="kg"] {
  font-weight: bold;
}

/* 
   具有属性"data-quantity"其值以"optional"开头的所有元素 
*/
[data-quantity^="optional"] {
  opacity: 0.5;
}
```
有了这些新的规则，我们会得到这个：

![css-select08]($res/css-select08.jpg)

例： 给足球比赛结果添加样式

```html
<ol>
  <li lang="en-GB" data-perf="inc-pro">Manchester United</li>
  <li lang="es" data-perf="same-pro">Barcelona</li>
  <li lang="de" data-perf="dec">Bayern Munich</li>
  <li lang="es" data-perf="same">Real Madrid</li>
  <li lang="de" data-perf="inc-rel">Borussia Dortmund</li>
  <li lang="en-GB" data-perf="dec-rel">Southampton FC</li>
</ol>

```
添加属性选择器到一些规则来创建一个简单的足球结果列表。 这里有三件事要做：

*   前三个规则分别在列表项的左侧添加英国，德国和西班牙国旗图标。 您需要填写适当的属性选择器，以便团队获得正确的国家标志，并配合语言。
*   规则4-6在列表项中添加背景颜色，以指示球队是否已经在联盟中升格（绿色，rgba（0,255,0,0.7）），向下（红色，rgba（255,0,0,0.5）） ），或者留在同一个地方（蓝色，rgba（0,0,255,0.5））。填写适当的属性选择器，以将正确的颜色与正确的队列匹配，并与inc，相同和dec字符串匹配 data-perf属性值。
*   规则7-8使得晋级的球队变得加粗，有降级危险的球队为斜体和灰色。 填写适当的属性选择器，将这些样式与正确的队列进行匹配，并与data-perf属性值中显示的pro和rel字符串进行匹配。

```css
li[lang="en-GB"] {
   background: url("http://mdn.github.io/learning-area/css/introduction-to-css/css-selectors/en-GB.png") 5px center no-repeat;
}

li[lang="de"] {
   background: url("http://mdn.github.io/learning-area/css/introduction-to-css/css-selectors/de.png") 5px center no-repeat;
}

li[lang="es"] {
   background: url("http://mdn.github.io/learning-area/css/introduction-to-css/css-selectors/es.png") 5px center no-repeat;
}

li[data-perf*="inc"] {
   background-color: rgba(0,255,0,0.7);
}

li[data-perf*="same"] {
   background-color: rgba(0,0,255,0.5);
}

li[data-perf*="dec"] {
   background-color: rgba(255,0,0,0.7);
}

li[data-perf*="pro"] {
   font-weight: bold;
}

li[data-perf*="rel"] {
   font-style: italic;
   color: #666;
}

ol {
   padding: 0;
}

li {
   padding: 3px 3px 3px 34px;
   margin-bottom: 5px;
   list-style-position: inside;
}
```
效果如图：

![css-select09]($res/css-select09.jpg)

## 伪类和伪元素选择器

伪选择器不是选择元素，而是元素的某些部分，或仅在某些特定上下文中存在的元素。它们有两种主要类型 ： 伪类和伪元素。

### 伪类（Pseudo-class）

一个 CSS  `伪类（pseudo-class）`是一个以冒号(`:`)作为前缀的关键字，当你希望样式在特定状态下才被呈现到指定的元素时，你可以往元素的选择器后面加上对应的伪类（pseudo-class）。你可能希望某个元素在处于某种状态下呈现另一种样式，例如当鼠标悬停在元素上面时，或者当一个复选框被禁用或被勾选时，又或者当一个元素是它在 DOM 树中父元素的第一个孩子元素时。

*   [`:active`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:active ":active CSS伪类匹配被用户激活的元素。它让页面能在浏览器监测到激活时给出反馈。当用鼠标交互时，它代表的是用户按下按键和松开按键之间的时间。 :active 伪类通常用来匹配tab键交互。通常用于但并不限于 <a> 和 <button> HTML元素。")
*   [`:any`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:any "The :any() pseudo-class lets you quickly construct sets of similar selectors by establishing groups from which any of the included items will match. This is an alternative to having to repeat the entire selector for the one item that varies.")
*   [`:checked`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:checked )
*   [`:default`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:default ":default CSS pseudo-class 表示一组相关元素中的默认表单元素。")
*   [`:dir()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:dir "此页面仍未被本地化, 期待您的翻译!")
*   [`:disabled`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:disabled ":disabled  CSS 伪类表示任何被禁用的元素。如果一个元素不能被激活（如选择、点击或接受文本输入）或获取焦点，则该元素处于被禁用状态。元素还有一个启用状态（enabled state），在启用状态下，元素可以被激活或获取焦点。")
*   [`:empty`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:empty ":empty CSS 伪类 代表没有子元素的元素。子元素只可以是元素节点或文本（包括空格），无论一个元素是否为 (empty 或 not), 注释或处理指令都不会产生影响。")
*   [`:enabled`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:enabled "CSS 伪类 :enabled 表示任何启用的（enabled）元素。如果一个元素能够被激活（如选择、点击或接受文本输入）或获取焦点，则该元素是启用的。元素还有一个禁用的状态（disabled state），在被禁用时，元素不能被激活或获取焦点。")
*   [`:first`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first ":first @page CSS 伪类选择器 描述的是：打印文档的时候，第一页的样式。")
*   [`:first-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first-child ":first-child CSS伪类 代表了一组兄弟元素中的第一个元素。在level3实现中，被匹配的元素需要具有一个父级元素，而在level4实现中则不需要。")
*   [`:first-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first-of-type "此页面仍未被本地化, 期待您的翻译!")
*   [`:fullscreen`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:fullscreen "css伪类:fullscreen应用于当前处于全屏显示模式的元素。 它不仅仅选择顶级元素，还包括所有已显示的栈内元素。")
*   [`:focus`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:focus "CSS伪类 :focus表示获得焦点的元素（如表单输入）。当用户点击或触摸元素或通过键盘的 “tab” 键选择它时会被触发。")
*   [`:hover`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:hover ":hover CSS伪类适用于用户使用指示设备虚指一个元素（没有激活它）的情况。这个样式会被任何与链接相关的伪类重写，像:link, :visited, 和 :active等。为了确保生效，:hover规则需要放在:link和:visited规则之后，但是在:active规则之前，按照LVHA的循顺序声明:link－:visited－:hover－:active。")
*   [`:indeterminate`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:indeterminate ":indeterminate CSS 伪类 表示状态不确定的表单元素:")
*   [`:in-range`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:in-range "该伪类用于给用户一个可视化的提示，表示输入域的当前值处于允许范围内。")
*   [`:invalid`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:invalid )
*   [`:lang()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:lang "此页面仍未被本地化, 期待您的翻译!")
*   [`:last-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:last-child ":last-child CSS 伪类 代表父元素的最后一个子元素。")
*   [`:last-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:last-of-type ":last-of-type CSS 伪类 表示了在（它父元素的）子元素列表中，最后一个给定类型的元素。当代码类似Parent tagName:last-of-type的作用区域包含父元素的所有子元素中的最后一个选定元素，也包括子元素的最后一个子元素并以此类推。")
*   [`:left`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:left ":left CSS 伪类, 需要和@规则  @page 配套使用, 对打印文档的左侧页设置CSS样式.")
*   [`:link`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:link ":link伪类选择器是用来选中元素当中的链接。它将会选中所有尚未访问的链接，包括那些已经给定了其他伪类选择器的链接（例如:hover选择器，:active选择器，:visited选择器）。为了可以正确地渲染链接元素的样式，:link伪类选择器应当放在其他伪类选择器的前面，并且遵循LVHA的先后顺序，即：:link — :visited — :hover — :active。:focus伪类选择器常伴随在:hover伪类选择器左右，需要根据你想要实现的效果确定它们的顺序。")
*   [`:not()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:not "此页面仍未被本地化, 期待您的翻译!")
*   [`:nth-child()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-child "此页面仍未被本地化, 期待您的翻译!")
*   [`:nth-last-child()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-last-child "此页面仍未被本地化, 期待您的翻译!")
*   [`:nth-last-of-type()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-last-of-type "此页面仍未被本地化, 期待您的翻译!")
*   [`:nth-of-type()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-of-type "此页面仍未被本地化, 期待您的翻译!")
*   [`:only-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:only-child "CSS伪类:only-child代表了属于某个父元素的唯一一个子元素.等效的选择器还可以写成 :first-child:last-child或者:nth-child(1):nth-last-child(1),当然,前者的权重会低一点.")
*   [`:only-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:only-of-type "CSS 伪类 :only-of-type 代表了任意一个元素，这个元素没有其他相同类型的兄弟元素。")
*   [`:optional`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:optional ":optional CSS 伪类 表示任意没有required属性的 <input>，<select> 或  <textarea> 元素使用它。")
*   [`:out-of-range`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:out-of-range "该伪类用于给用户一个可视化的提示，表示输入域的当前值处于允许范围外。")
*   [`:read-only`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:read-only ":read-only CSS 伪类 表示元素不可被用户编辑的状态（如锁定的文本输入框）。")
*   [`:read-write`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:read-write ":read-write CSS 伪类 代表一个元素（例如可输入文本的 input元素）可以被用户编辑。")
*   [`:required`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:required ":required CSS 伪类 表示 任意 <input> 元素表示任意拥有required属性的 <input> 或 <textarea> 元素使用它. 它允许表单在提交之前容易的展示必填字段并且渲染其外观.")
*   [`:right`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:right "此页面仍未被本地化, 期待您的翻译!")
*   [`:root`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:root ":root 这个 CSS 伪类匹配文档树的根元素。对于 HTML 来说，:root 表示 <html> 元素，除了优先级更高之外，与 html 选择器相同。")
*   [`:scope`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:scope ":scope 属于CSS伪类，它将会匹配作为选择符匹配元素的参考点(css的作用域或作用点)。在HTML中，可以使用<style>的scoped属性来重新定义新的参考点。如果HTML中没有使用这个属性，那么默认的参考点(css的作用域或作用点)是<html>。")
*   [`:target`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:target ":target CSS 伪类 代表一个唯一的页面元素(目标元素)，其ID与当前URL片段匹配 .")
*   `:valid`
*   `:visited`

我们不会立马深入讲解每一个伪类（pseudo-class），在学习领域中，详尽无遗地教给你每一件事不是目的。在适当的时候，你会更加详尽的了解一些内容。

一个伪类（Pseudo-class）的例子

现在，让我们来看一个简单的使用例子。首先是一个 HTML 片段：

```html
<a href="https://developer.mozilla.org/" target="_blank">Mozilla Developer Network</a>
```

然后，一些 CSS 样式：

```css
/* 这些样式将在任何情况下应用于我们
的链接 */

a {
  color: blue;
  font-weight: bold;
}

/* 我们想让被访问过的链接和未被访问
的链接看起来一样 */

a:visited {
  color: blue;
}

/* 当光标悬停于链接，键盘激活或锁定
链接时，我们让链接呈现高亮 */

a:hover,
a:active,
a:focus {
  color: darkred;
  text-decoration: none;
}
```
现在，我们可以跟修改了样式的链接愉快地玩耍了！

![css-select10]($res/css-select10.gif)

例：一个条纹状的信息列表

为信息列表增加条纹状效果，然后增加合适的伪类使得里面的链接在鼠标经过时显得不同。

1.  使用伪类 a：hover 实现悬停样式。
2.  对于条纹状效果，你需要用到一个伪类，如`:nth-of-type()`，通过给那两个颜色规则添加稍微不同的伪类，使得列表的奇数行和偶数行有着不同的样式效果。

```html
<ul>
  <li><a href="#">United Kingdom</a></li>
  <li><a href="#">Germany</a></li>
  <li><a href="#">Finland</a></li>
  <li><a href="#">Russia</a></li>
  <li><a href="#">Spain</a></li>
  <li><a href="#">Poland</a></li>
</ul>
```
```css
ul {
  padding: 0;
}

li {
  padding: 3px;
  margin-bottom: 5px;
  list-style-type: none;
}

a {
  text-decoration: none;
  color: black;
}

a:hover {
  text-decoration: underline;
  color: red;
}

li:nth-of-type(2n) {
  background-color: #ccc;
}

li:nth-of-type(2n+1) {
  background-color: #eee;
}

```
效果如图：

![css-select11]($res/css-select11.gif)

### 伪元素

[伪元素（Pseudo-element）](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)跟伪类很像，但它们又有不同的地方。它们都是关键字，但这次伪元素前缀是两个冒号 (`::`) ， 同样是添加到选择器后面去选择某个元素的某个部分。

*   [`::after`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::after "CSS伪元素::after用来创建一个伪元素，做为已选中元素的最后一个子元素。通常会配合content属性来为该元素添加装饰内容。这个虚拟元素默认是行内元素。")
*   [`::before`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::before "常通过 content 属性来为一个元素添加修饰性的内容。")
*   [`::first-letter`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::first-letter "CSS 伪元素 ::first-letter会选中某 block-level element（块级元素）第一行的第一个字母，并且文字所处的行之前没有其他内容（如图片和内联的表格） 。")
*   [`::first-line`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::first-line "::first-line CSS pseudo-element （CSS伪元素）在某 block-level element （块级元素）的第一行应用样式。第一行的长度取决于很多因素，包括元素宽度，文档宽度和文本的文字大小。")
*   [`::selection`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::selection "::selection CSS伪元素应用于文档中被用户高亮的部分（比如使用鼠标或其他选择设备选中的部分）。")
*   [`::backdrop`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::backdrop "::backdrop CSS 伪元素 是在任何处于全屏模式的元素下的即刻渲染的盒子（并且在所有其他在堆中的层级更低的元素之上）。")

它们都拥有特别的行为和有趣的特性，但我们不会在这里对它们都进行深入探究。

一个伪元素（pseudo-element）例子

我们在这里仅展示一个简单的 CSS 例子，就是如何在所有超链接元素后面的增加一个箭头：

```html
<ul>
  <li><a href="https://developer.mozilla.org/en-US/docs/Glossary/CSS">CSS</a> defined in the MDN glossary.</li>
  <li><a href="https://developer.mozilla.org/en-US/docs/Glossary/HTML">HTML</a> defined in the MDN glossary.</li>
</ul>
```
```

让我们加上 CSS 规则：

```css
/* 所有含有"href"属性并且值以"http"开始的元素，
将会在其内容后增加一个箭头（去表明它是外部链接）
*/

[href^=http]::after {
  content: '⤴';
}
```

我们可以得到这样的效果：

![css-select12]($res/css-select12.jpg)

例：一个很棒棒的段落

我们有一个很棒棒的段落需要装饰！你需要做的事是给出两个规则集分别指定段落的第一行和第一个单词，可以使用伪元素 `::first-line`和 `::first-letter` 得到需要的。元素需要的效果，是段落的第一行使用粗体字，它的第一个单词首字母大写并给它一种老式的感觉。

```html
<p>This is my very important paragraph.
 I am a distinguished gentleman of such renown that my paragraph
 needs to be styled in a manner befitting my majesty. Bow before 
my splendour, dear students, and go forth and learn CSS!</p>
```
```css
p::first-line {
  font-weight: bold;
}

p::first-letter {
  font-size: 3em;
  border: 1px solid black;
  background: red;
  display: block;
  float: left;
  padding: 2px;
  margin-right: 4px;
}
```
效果如下图：

![css-select13]($res/css-select13.jpg)

### 伪类选择器总结

伪类选择器一般会用在超链接a标签中，使用a标签的伪类选择器，我们一定要遵循"爱恨准则"  LoVe HAte。

例：
```css
	/*没有被访问的a标签的样式*/
        .box ul li.item1 a:link{         
            color: #666;
        }

        /*访问过后的a标签的样式*/
        .box ul li.item2 a:visited{
            color: yellow;
        }

        /*鼠标悬停时a标签的样式*/
        .box ul li.item3 a:hover{
            color: green;
        }

        /*鼠标摁住的时候a标签的样式*/
        .box ul li.item4 a:active{
            color: yellowgreen;
        }
```
再介绍一种css3的选择器nth-child()

```css
	/*选中第一个元素*/
        div ul li:first-child{
            font-size: 20px;
            color: red;
        }
        /*选中最后一个元素*/
        div ul li:last-child{
            font-size: 20px;
            color: yellow;
        }
        
        /*选中当前指定的元素  数值从1开始*/
        div ul li:nth-child(3){
            font-size: 30px;
            color: purple;
        }
        
        /*n表示选中所有,这里面必须是n, 从0开始的  0的时候表示没有选中*/
        div ul li:nth-child(n){
            font-size: 40px;
            color: red;
        }
        
        /*偶数*/
        div ul li:nth-child(2n){
            font-size: 50px;
            color: gold;
        }
        /*奇数*/
        div ul li:nth-child(2n+1){
            font-size: 50px;
            color: yellow;
        }
        /*隔几换色  隔行换色
             隔4换色 就是5n+1,隔3换色就是4n+1
            */
        
        div ul li:nth-child(5n+1){
            font-size: 50px;
            color: red;
        }
```
## 组合器和多个选择器

组合器和选择器组 — 将多个选择器组合在一起以进一步利用其选择能力。

虽然一次使用一个选择器就很有用，但在某些情形中却可能效率低下。 CSS选择器在你开始组合他们进行细粒度选择的时候变得更具使用价值。基于元素之间的相互关联关系，CSS提供了几种方法来对元素进行选择。下表使用连接符展示了这些关联关系(A和B代表前文所述的任意选择器):

| Combinators | Select |
| --- | --- |
| A,B | 匹配满足A（和/或）B的任意元素（参见下方 `同一规则集上的多个选择器`）. |
| A B | 匹配任意元素，满足条件：B是A的后代结点（B是A的子节点，或者A的子节点的子节点） |
| A > B | 匹配任意元素，满足条件：B是A的直接子节点 |
| A + B | 匹配任意元素，满足条件：B是A的下一个兄弟节点（AB有相同的父结点，并且B紧跟在A的后面） |
| A ~ B | 匹配任意元素，满足条件：B是A之后的兄弟节点中的任意一个（AB有相同的父节点，B在A之后，但不一定是紧挨着A）      |


### 组合器示例

接下来看一个使用了组合器的例子:

```html
<table lang="en-US" class="with-currency">
  <thead>
    <tr>
      <th scope="col">Product</th>
      <th scope="col">Qty.</th>
      <th scope="col">Price</th>
    </tr>
  </thead>
  <tfoot>
    <tr>
      <th colspan="2" scope="row">Total:</th>
      <td>148.55</td>
    </tr>
  </tfoot>
  <tbody>
    <tr>
      <td>Lawnchair</td>
      <td>1</td>
      <td>137.00</td>
    </tr>
    <tr>
      <td>Marshmallow rice bar</td>
      <td>2</td>
      <td>1.10</td>
    </tr>
    <tr>
      <td>Book</td>
      <td>1</td>
      <td>10.45</td>
    </tr>
  </tbody>
</table>
```

然后对该HTML文档应用下面的样式表:

```css
/* 基本的table样式 */
table {
  font: 1em sans-serif;
  border-collapse: collapse;
  border-spacing: 0;
}

/* 所有在table里的td以及th，这里的逗号不是一个组合器，
    它只是允许你把几个选择器对应到相同的CSS规则上.*/
table td, table th {
  border : 1px solid black;
  padding: 0.5em 0.5em 0.4em;
}

/* 所有table里的thead里的所有th */
table thead th {
  color: white;
  background: black;
}

/* 所有table里的tbody里的所有td，每个td都是由它上边的td选择 */
table tbody td + td {
  text-align: center;
}

/*table里所有的tbody里的td当中的最后一个 */
table tbody td:last-child {
  text-align: right
}

/* 所有table里的tfoot里的th */
table tfoot th {
  text-align: right;
  border-top-width: 5px;
  border-left: none;
  border-bottom: none;
}

/* 在table当中，所有的th之后的td */
table th + td {
  text-align: right;
  border-top-width: 5px;
  color: white;
  background: black;
}

/* 拥有属性lang并且这个属性值为en-US的类名为“with-currency”里边的td当中
的最后一个元素添加一个伪类::before*/
.with-currency[lang="en-US"] td:last-child::before {
  content: '$';
}

/* 拥有属性lang并且这个属性值为fr的类名为“with-currency”里面的td当中的
最后一个元素添加一个伪类::after */
.with-currency[lang="fr"] td:last-child::after {
  content: ' €';
}
```
效果如下：

![css-select14]($res/css-select14.jpg)

### 应用同一规则的选择器组

你已经遇见过这种做法的许多例子，但还是让我们来把它进一步阐释清楚。通过相互间用逗号分隔的多个选择器所形成的组，可以一次性将同一规则同时应用到多组选定元素。例如：

```css
p, li {
  font-size: 1.6em;
}
```

或者这样:

```css
h1, h2, h3, h4, h5, h6 {
  font-family: helvetica, 'sans serif';
}
```
### 组合器和多个选择器（高级选择器）总结

可以形像分为

* 后代选择器 （空格）
	* 使用空格表示后代选择器。顾名思义，父元素的后代（包括儿子，孙子，重孙子）
* 子代选择器 （>）
	* 使用>表示子代选择器。比如div>p,仅仅表示的是当前div元素选中的子代(不包含孙子....)元素p。
* 兄弟选择器 （`+` `~`）
	* `A+B`匹配任意元素，满足条件：B是A的下一个兄弟节点（AB有相同的父结点，并且B紧跟在A的后面）
	* `A~B`匹配任意元素，满足条件：B是A之后的兄弟节点中的任意一个（AB有相同的父节点，B在A之后，但不一定是紧挨着A）
* 并集选择器 （逗号）
	* 多个选择器之间使用逗号隔开。表示选中的页面中的多个标签。一些共性的元素，可以使用并集选择器
* 交集选择器 （`.`）
	* 使用`.`表示交集选择器。第一个标签必须是标签选择器，第二个标签必须是类选择器 语法：div.active

关于选择器就浅析到这了。
end
2018-5-31

