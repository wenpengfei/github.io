---
title: 为什么CSS Grid比Bootstrap更适合布局
date: 2017-12-05 12:42:49
tags: css,grid,bootstrap
---

![img](/images/css-grid-is-better-than-bootstrap/1.png)
``CSS Grid``是一种全新的创建布局方式，这是有史以来第一个合适的布局系统，并且是浏览器原生的，给我们带来了很多好处。

当你和当今最流行的``Bootstrap``框架相比，grid的好处变的尤为清晰，您不仅可以创建在以前在不引入JavaScript的情况下无法实现的布局，而且您的代码将更易于维护和理解。

本文中我会解释一下为什么。

## 标签会更加简洁

相比``Bootstrap``，使用grid会使你的HTML更加干净，虽然这不是最重要的好处，但它可能会是你第一个注意到的。

为了举例说明，我创建了一个布局，以便我们可以比较两个版本所需要的代码。

![img](/images/css-grid-is-better-than-bootstrap/2.png)

>注意：我在给出的例子中稍微设计了一下，但是他和我们比较``Bootstrap``没有任何关系，所以我只保留布局部分的CSS

## Bootstrap
先看一下``Bootstrap``需要创建的标签。
![img](/images/css-grid-is-better-than-bootstrap/3.png)

这里有两件事需要注意一下：
- 每个row都需要一个``<div>``标签
- 使用了class name来指定布局(``col-xs-2``)

随着这种布局的复杂性增长，HTML也是如此。

如果这是个响应式网站，它会看起来更复杂：
![img](/images/css-grid-is-better-than-bootstrap/4.png)

现在我们来看一下用Grid布局：
![img](/images/css-grid-is-better-than-bootstrap/5.png)

我可以在这里使用语义化元素，但我还是使用div来和``Bootstrap``对比。

显然，grid用来布局看起来更简单，丑陋的类名和每行所需的额外的div标签一去不复返了，简简单单一个container和里面的item。

与``Bootstrap``不同的是，随着布局复杂度的增加，Grid布局标签的复杂度将不会增加太多。

``Bootstrap``示例不需要添加任何CSS，引用一下就可以了。``CSS Grid``肯定需要添加。具体来说，是这样的：
![img](/images/css-grid-is-better-than-bootstrap/6.png)

这可能是一些人赞成``Bootstrap``的一个论点：你不用关心CSS，只需要在HTML中定义布局。但是，正如你将会明白的那样，当涉及到灵活性的时候，标签和布局之间的耦合会变成一个很大的问题。

## 更灵活

假设您想要根据屏幕大小更改布局。 例如，将菜单拉到最上面一行，在移动设备上查看。

换句话说，布局从这样：

![img](/images/css-grid-is-better-than-bootstrap/2.png)

换成这样：

![img](/images/css-grid-is-better-than-bootstrap/12.png)

## CSS Grid

用``CSS Grid``的话会非常简单，我们只需要添加一个``media query``，布局就像变魔术一样变成了你想要的。

![img](/images/css-grid-is-better-than-bootstrap/7.png)

你可以这样重新排列布局，不用担心HTML标签编写的顺序，这对开发人员和设计师都是很大的一个好处！

## BootStrap

如果想在``Bootstrap``中做同样的事情，就必须得修改HTML了，需要调整标签的顺序。


![img](/images/css-grid-is-better-than-bootstrap/8.png)

这个需求仅仅使用media query是远远不够的，你还得使用JavaScript。

这个例子是我体会到的grid最大的好处

## 不再限死12列

这个不是一个很大的问题，但是这个问题也困扰过我多次，因为``Bootstrap``的grid系统分为了12列，如果你想要一个5列的布局就会纠结，或是7列、9列、任何不会合为12列的。

``CSS Grid``就没有任何限制，你可以让grid正好有你想要的数量。这是一个7列的grid：

![img](/images/css-grid-is-better-than-bootstrap/9.png)

通过设置``grid-template-columns : repeat(7, 1fr)``实现，就像这样：

![img](/images/css-grid-is-better-than-bootstrap/10.png)

## 浏览器支持

当然也必须讨论一下浏览器支持，在撰写本文的时候，全球75%的网站流量支持``CSS Grid``

![img](/images/css-grid-is-better-than-bootstrap/11.png)

> ``CSS Grid``是一个布局模块，它允许我们改变文档的布局，而不会干扰标签顺序。换句话说，CSS网格是一个纯粹的可视化工具，使用得当，对文档内容的表达应该没有影响。所以：在旧的浏览器中缺乏对``CSS Grid``的支持不影响访问者的体验，只是让体验不同。


<a href="https://hackernoon.com/how-css-grid-beats-bootstrap-85d5881cf163" target="_blank">原文链接</a>