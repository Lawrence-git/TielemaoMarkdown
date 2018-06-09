# 常见css样式

[[TOC]]

文：铁乐猫

## body标签 主体美化
```css
body{
	margin: 0;
  <!-- 设置底色为奶白色,对眼睛会更好,没那么闪 -->
	color: #5a5a5a
}
```
## a标签 超链接美化
```css
a {
  /* 设置超链接颜色为天空蓝 */
  color: #08c;
  /* 设置超链接下划线不显示，美化 */
  text-decoration: none;
}

/* 设置当鼠标移到一个超链接上面时，重新出现下划线 */
a:hover {
  text-decoration: underline;
}
```
## 伪元素清除浮动

```css
.clearfix:after{
	content: '.';
	display: block;
	clear: both;
	visibility: hidden;
	height: 0;
```
项目中推荐使用伪元素清除浮动法，`overflow：hidden`来清除浮动的方法要看情况使用，比如说导航栏中有鼠标点击后弹出悬浮栏的话，用`overflow： hidden`就会将超出部分的隐藏掉了，达不到我们想要的效果。所以一般而言，使用伪元素清除浮动比较多。