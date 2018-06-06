CSS学习摘要-引入样式
===
注：主要是摘录自[MDN 网络开发者](https://developer.mozilla.org/zh-CN)这个网站的。

## CSS 实际上如何工作？

当浏览器显示文档时，它必须将文档的内容与其样式信息结合。它分两个阶段处理文档：

1.  浏览器将 HTML和 CSS转化成 DOM（_文档对象模型_）。DOM在计算机内存中表示文档。它把文档内容和其样式结合在一起。
2.  浏览器显示 DOM 的内容。

![CSS-work]($res/CSS-work.jpg)

## 如何将你的 CSS 应用到你的 HTML 上

这有你常见的三种不同方式将 CSS 应用到 HTML 文档上，有的方式比其他方式更有用。在这里，我们将简要回顾一下每一种方式：

### 外部样式表 (推荐)

你已经在这篇文章看到了外部样式表，但是并不知道它的名字。外部样式表是指：当你将你的 CSS 保存在一个独立的扩展名为 .css 的文件中，并从HTML的 `<link>`元素中引用它。
```html
<link rel="stylesheet" href="style.css">
```
HTML 中<link>元素指定了外部资源与当前文档的关系。 
这个元素的用途包括为导航定义一个关系框架。这个元素经常用来链接样式表（如CSS文件）。

此时 HTML 文件看起来像这样：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
    <link rel="stylesheet" href="style.css">
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>This is my first CSS example</p>
  </body>
</html>
```

以及下面的 CSS 文件

```css
h1 {
  color: blue;
  background-color: yellow;
  border: 1px solid black;
}

p {
  color: red;
}
```

这种方法可以说是最好的，因为你可以使用一个样式表来设置多个文档的样式，并且需要更新 CSS 的时候只要在一个地方更新。

#### 另：外部样式表-导入式
和link有一点点 不同，使用的是`@import url`外接样式表。
```
<style type="text/css">
        @import url('./index.css');
</style>
```
### 内部样式表

内部样式表是指不使用外部 CSS 文件，而是将你的 CSS 放置在`<style>`元素中，(HTML的`<style>`元素包含了文档的样式化信息或者文档的一部分，该标签的样式信息通常是CSS的格式。) 该元素包含在 `HTML head` 内。

此时HTML看起来像这样：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
    <style>
      h1 {
        color: blue;
        background-color: yellow;
        border: 1px solid black;
      }

      p {
        color: red;
      }
    </style>
  </head>
  <body>
    <h1>Hello World!</h1>
    <p>This is my first CSS example</p>
  </body>
</html>
```

这在某些情况下很有用（也许你正在使用一个内容管理系统，不能直接修改 CSS 文件），但它不如外部样式表高效 —— 在网站中，CSS 将需要在每个页面重复，并且需要更新时要更改多个位置。

### 内联样式

内联样式是仅影响一个元素的CSS声明，被 `style`属性包括着:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>My CSS experiment</title>
  </head>
  <body>
    <h1 style="color: blue;background-color: yellow;border: 1px solid black;">Hello World!</h1>
    <p style="color:red;">This is my first CSS example</p>
  </body>
</html>
```

除非有必要，否则不要这么做！这很难维护（你可能不得不在每份文档里更新多次同样的信息），并且它还混合了 CSS 表示的样式信息和 HTML 的结构信息，使 CSS 难以阅读和理解。保持不同类型代码的分离和纯净使处理该代码的任何人工作更为容易。

您唯一可能需要使用内联样式是当您的工作环境真的非常受限（也许您的CMS只允许您编辑 HTML 的 body）。

## 总结

在HTML中引入css方式总共有三种：

1.  行内样式 ` [style]`
2.  内接样式  `<style>`
3.  外接样式  (推荐规范）
　　3.1 链接式（`link`）
　　3.2 导入式（`@import url`）

end
2018-05-30