---
title: 编写 React 组件的 10 个建议
date: 2019-07-05 18:25:04
tags:
---

![img](/images/the-10-component-commandments/1.jpg)

写一个公用的组件很难，你必须细心地考虑很多问题，比如应该暴露出哪些 `props`。

本文将简要的介绍 API 设计中的一些最佳实践，以及编写 React 组件的 10 条参考规则。

## 什么是 API ？

API （Application Programming Interface）是两段代码或者两个应用如何交互的一个定义或者接口。

- 后端和前端交互使用的是就是 API，可以通过此 API 获取或者操作一组数据
- 类和调用该类的代码之间的接口也是 API，你可以调用类里面的方法

同理，组件定义的 `props` 也是 API，这是用户与组件交互的方式。

## API 设计中的一些最佳实践

所以，设计一个 API 的时候应该有哪些规则和注意事项？我们做了很多研究并结合实践给出了 4 条 API 设计的最佳实践：

#### 稳定的版本

最重要的规则之一就是要保持稳定，意思就是要最大限度的减少破坏性的升级，如果你做了破坏性的升级，一定要写一个完整的升级指南，如果可能的话，再提供一些 API 让用户消化升级的过程，使用户升级版本的成本降低。

如果你要发布 API，请使用 [语义化版本](https://semver.org/lang/zh-CN/)，以便用户可以自由的选择他们需要的版本。

#### 提供错误描述信息

调用 API 出错的时候，返回给客户端一个详细的错误说明，并且告诉客户端应该如何解决。在没有任何上下文的情况下返回一个 “调用出错” 的错误信息给客户端并不是一个友好的体验。

#### 不要让开发人员迷惑

开发人员都是很傲娇的，并不想在使用你 API 的时候感到迷惑，换句话说，使你的 API 尽可能的直观，规范。可以通过遵循一些原则和命名规范去实现。

举个例子，你的 API 提供了 boolean 类型的参数，参数命名的时候你在一个地方用 is 做前缀，另外一个地方又用了 has 做前缀，然后其他地方又用了另外一个前缀，这会让开发人员比较迷惑。

#### 暴露出来的 API 尽可能的少

当然不是说功能多了不好，只是要善用外观模式或者命令模式等去封装一些操作，做到高内聚，API 过多会增加学习成本，一个高内聚的 API 会被认做是一个易于使用的 API。

## 组件设计的 10 个建议

上面 4 条规则在 REST APIs 中应用的很好，之前提过的，我们的组件也有它的 API，就是 `props`，我们该如何定义 `props` 使它不违反上面的规则呢？下面列出 10 条建议：

#### 写文档

如果你没有写个组件的使用文档，好吧，使用者可以看你的代码，但这并不是一个好的体验。

有很多写文档的工具，这里推荐三个：

- [Storybook](https://storybook.js.org/)
- [Styleguidist](https://react-styleguidist.js.org/)
- [Docz](https://www.docz.site/)

不管选用哪个，一定要把所有的 API 使用说明都写出来。

#### 允许上下文语义

HTML是一种以语义方式结构化信息的语言。然而，我们的大多数组件都是由 `<div />` 标签组成的。它在某种程度上是没问题的，因为通用组件不知道我们想要的是 `<article />` 或 `<section />` 还是 `<aside />`，但是这样并不优雅。所以，我们建议允许组件接受一个 `prop`，它将覆盖正在呈现的 DOM 元素。以下是如何实现它的示例：

```jsx
function Grid({ as: Element, ...props }) {
  return <Element className="grid" {...props} />
}

Grid.defaultProps = {
  as: 'div',
};
```

在 jsx 中把 `as` prop 重命名文本地变量 `Element`，并且把默认值设为 `div`。

使用 `<Grid />` 的时候传递你想要的标签名就好了。

```jsx
function App() {
  return (
    <Grid as="main">
      <MoreContent />
    </Grid>
  );
}
```

这样在 React 中使用是没有问题的，另外一个经典的例子就是你有一个 `<Button />` 组件，想把它渲染成 React Router 的 `<Link />`

```jsx
<Button as={Link} to="/profile">
  Go to Profile
</Button>
```

#### 避免使用 boolean props

Boolean props 听起来是个不错的选择，因为你可以不用指定值，看起来非常优雅。

```jsx
<Button large>BUY NOW!</Button>
```

尽管 Boolean props 看起来非常优雅，但是它只能支持两个值 `true` or `false`

如果你的组件设计了很多种 boolean 类型的 props 看起来就比较累赘了：

```jsx
<Button large small primary disabled secondary>
  WHAT AM I??
</Button>
```

换句话说，boolean 通常很难根据需求去扩展。换成字符串枚举是一个更好的选择，它可以扩展为二元值之外的任意值。

```jsx
<Button variant="primary" size="large">
  I am primarily a large button
</Button>
```

不是说 boolean props 一无是处，一些不可能扩展并且只有两个值的 prop 比如 disabled 还是用 boolean 类型。

#### 使用 props.children

React 有几个特殊的 prop，比如 `key`，他们有特殊的处理机制，还有一个就是 `children`。

在开始标签和结束标签中间的内容都会被塞进 `props.children` props，应该尽可能多的使用它，因为它比一个 `content` prop，或者一些文本内容需要传递的时候更易使用。

```jsx
<TableCell content="Some text" />
// vs
<TableCell>Some text</TableCell>
```

使用 `props.children` 有几个好处。首先，它类似于常规 HTML 的使用方式。其次，你可以自由地传递任何你想要的东西，而不是将 `leftIcon` 和 `rightIcon` prop 添加到组件中，只需将它们作为 `props.children` prop 的一部分传递：

```jsx
<TableCell>
  <ImportantIcon /> Some text
</TableCell>
```

你可能会说你的组件只会渲染纯文本，不需要其他东西，现在看来可能没问题，但是以后需求不断变化的时候你就会发现 `props.children` 的好处。

#### 父组件钩子函数

有时候我们会写一些内部逻辑很复杂的组件，比如 `AutoComplete` 或者一些图表。

这些类型的组件要渲染的内容通常依赖外部的 API，随着时间的推移，可能还要实现一些特殊的需求

我们如何提供一个单一又标准化的 prop 让调用者去控制或者覆盖组件内部的默认逻辑呢？

解决方案是 `state reducers` 模式，这里有一篇文章 [post about the concept itself](https://kentcdodds.com/blog/the-state-reducer-pattern)

总结下来，`state reducer` 模式就是让消费者可以访问到组件内部发生的一切事件、状态，从而进行更高级的自定义配置

```jsx
function MyCustomDropdown(props) {
  const stateReducer = (state, action) => {
    if (action.type === Dropdown.actions.CLOSE) {
      buttonRef.current.focus();
    }
  };
  return (
    <>
      <Dropdown stateReducer={stateReducer} {...props} />
      <Button ref={buttonRef}>Open</Button>
    </>
}
```

#### 扩散剩余的 props

当你创建一个新组件的时候，确保剩余的 props 被传递到可以生效的 element 上，不必仅仅是为了向底层组件传递 props 就额外添加一个。可以用 `...rest` 操作符：

```jsx
function ToolTip({ isVisible, ...rest }) {
  return isVisible ? <span role="tooltip" {...rest} /> : null;
}
```

#### 给出足够的默认值

尽量为 props 提供默认值，这样也可以大大减少需要传递 props 的数量，以 `onClick` 为例，可以提供一个空函数作为默认值，另外可以为字符类型的 prop 提供一个空字符串作为默认值，这样当没有传递 prop 的时候就可以确保处理的是空字符串而不是 undefined 或者 null 等不确定的值 

#### 不要重命名 HTML 的属性

HTML 标签本身就含有自己的一些属性，就是他自己的 API，为什么不用呢？

就像前面提到过的，减少暴露的 API，为什么要添加一个 screenReaderLabel prop 而不用自身原有的 aria-label API呢？

所以，请不要为了“易用性”而去重复定义，比如添加了 screenReaderLabel prop，然后又传递了一个 aria-label 属性，那么最终显示应该是什么呢？

另外，请不要覆盖 HTML 标签本身的属性，比如 `<button />` 元素的 type 属性，可以是 submit（默认）button 或者 reset，然而许多开发者都将它重新定义为其他的含义的props（primary warning info 等），这样就令使用者比较迷惑。

#### 编写 prop types

代码既文档，现在已经有 `prop-types` 包可用，去使用它。

如果没有传递一些必须的 props，控制台会报错，如果用的是 TypeScript 或者 Flow，那开发体验就更好了。

#### 为开发者设计

最后，遵循最重要的规则。确保您的 API 和 “组件体验” 针对将使用它的人员或是开发同事进行了优化。

提升开发体验的一个方法就是提供详细的报错信息，或者是在开发模式下在控制台发出警告。

在控制台报错或者报警的时候，如果开发人员也看到了对应错误或者警告的文档链接，会大大提升组件的使用体验。

不必担心过于冗长的错误信息会占用太大空间，况且构建生产环境的时候也不会把这些信息打包进去。

React 本身就是一个非常优秀的类库，当你忘记使用 key 或者拼错了生命周期的名字等等，都会在控制台收到大量详细的错误警告信息。





