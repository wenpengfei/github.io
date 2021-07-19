---
title: CSS终极之战：Grid vs Flexbox
date: 2017-12-12 02:19:06
tags:
---
在过去几年里，`Flexbox`已经成了前端最流行的布局框架，这并不奇怪，因为我们可以很方便的用它去对齐元素。

然而，前端村儿里面还有个叫`Grid`的小孩儿，他和`Flexbox`有很多功能相似的地方，有些地方他比`Flexbox`要好，但有也有不足。

这也会变成让开发者们纠结的地方，应该用哪个？本文将会在宏观和微观上面对比两个模块。

## 一维 vs 二维

>`Flexbox`用来做一维布局，`Grid`用来做二维布局

意思是如果你只在一个方向上布局（比如在header里面放三个button），你需要使用`Flexbox`

![img](/images/the-ultimate-css-battle-grid-vs-flexbox/2.png)

他将会比`Grid`更加灵活，可以用更少的代码去实现并且更加容易维护。

但是，如果你打算在两个维度上创建一个完整的布局，同时使用行和列，那么你应该使用`Grid`

## 二维

![img](/images/the-ultimate-css-battle-grid-vs-flexbox/3.png)

在这种情况下，`Grid`会更加灵活，并且会使你的标签更简单，代码更容易维护。

你可以结合两者一起使用，在上面的例子中最完美的做法是使用`Grid`来布局页面，使用`Flexbox`去对齐header里面的内容。

## 内容优先 vs 布局优先

两者的另一个核心区别是`Flexbox`以内容为基础，`Grid`以布局为基础，听起来有些抽象，我们来用一个实际的例子去解释一下。

我们会用上一段提到的header，他的HTML是这样的：

```html
<header>
  <div>Home</div>
  <div>Search</div>
  <div>Logout</div>
</header>
```

在使用`Flexbox`之前，里面的div会堆叠在彼此之上：

![img](/images/the-ultimate-css-battle-grid-vs-flexbox/4.png)

## Flexbox header

当我们加了`display:flex`之后，他们会漂亮的在一条线上。

```css
header {
  display:flex;
}
```

![img](/images/the-ultimate-css-battle-grid-vs-flexbox/5.png)

为了让logout button 在最右边，我们简单的给他指定一个margin：

```css
header > div:nth-child(3) {
  margin-left: auto;
}
```

效果如下：

![img](/images/the-ultimate-css-battle-grid-vs-flexbox/6.png)

值得注意的是：让元素本身决定他放在哪里，我们除了`display: flex`之外不添加任何东西。

这是`Flexbox`和`Grid`的核心差别，当我们用`Grid`来创建这个header时，这个差别会更加明显。

>尽管用`Grid`创建一维的header不太合适，但在本文中去实现它却是一个很好的练习，因为它教会了我们关于`Flexbox`和`Grid`的核心区别

## Grid header

如果使用`Grid`会有多种方法。我准备用非常简单的一种去做，我们的`Grid`有十列，每一列都是一个单位宽度 。

```css
header {
  display: grid;
  grid-template-columns: repeat(10, 1fr);
}
```

他看起来和`Flexbox`的解决方案一样

![img](/images/the-ultimate-css-battle-grid-vs-flexbox/7.png)

但是，我们可以在上帝视角去看两者有什么不同，我们来使用chrome的审查元素去看一下：

![img](/images/the-ultimate-css-battle-grid-vs-flexbox/8.png)

最关键的区别就是，这种方式必须先定义布局的列。从定义列的宽度开始，然后我们才能将元素放在可用的单元格中。

>这种方式强迫我们去分割我们的header有多少列

除非我们改变`Grid`，否则我们会被困死在10列中，但是在`Flexbox`中我们不会被这个麻烦困扰。

为了把logout放在最右边，我们会把他放在第十列：

```css
header > div:nth-child(3) {
  grid-column: 10;
}
```

审查元素时，看起来是这样的：

![img](/images/the-ultimate-css-battle-grid-vs-flexbox/9.png)

我们不能简单的添加一个`margin-left: auto;`因为它已经被放在了第三个单元格中，想要移动它，我们得再找一个单元格把它放进去。

## 结合两者

现在我们看下如如何同时使用`Grid`和`Flexbox`来把header合并进我们的布局，我们先来创建布局。

![img](/images/the-ultimate-css-battle-grid-vs-flexbox/10.png)

标签如下：

```html
<div class="container">
  <header>HEADER</header>
  <aside>MENU</aside>
  <main>CONTENT</main>
  <footer>FOOTER</footer>
</div>
```

然后是CSS：

```css
.container {
  display: grid;
  grid-template-columns: repeat(12, 1fr);
  grid-template-rows: 50px 350px 50px;
}
```

把元素放进`Grid`：

```css
header {
  grid-column: span 12;
}
aside {
  grid-column: span 2;
}
main {
  grid-column: span 10;
}
footer {
  grid-column: span 12;
}
```

下面仅需要来布置好header：我们把它从`Grid`中的元素转换为`Flexbox`容器。

```css
header {
  display: flex;
}
```

现在我们把logout放在右边：

```css
header > div:nth-child(3) {
  margin-left: auto;
}
```

现在我们有了同时使用`Grid`和`Flexbox`的完美布局。下面是这两个容器：

![img](/images/the-ultimate-css-battle-grid-vs-flexbox/11.png)

所以现在你应该理解了`Flexbox` 和`Grid`的差别，也知道了如何结合两者使用。

## 浏览器支持

本文截止前，全球77%流量的网站支持`Grid`，这个数字还在不断增长。

>我相信2018年将会是`Grid`的一年，他将获得突破，并将成为前端开发人员的必备技能，就像过去几年的`Flexbox`一样。