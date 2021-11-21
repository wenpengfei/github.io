---
title: React 中的授权处理
date: 2020-01-09 15:49:27
tags:
---

`认证(Authentication)`和`授权(Authorization)`是我们在几乎所有的开发中最常见需求，`认证`需要知道“你是谁”，`授权`需要知道“你能做什么”，我们今天需要讨论的是`授权`问题

如何在使用了 `react-router` 的项目中做授权呢？首先想到的是可以拦截路由：

```js
// Route handler
class YourRoute extends React.Component {
  componentDidMount() {
    // Load user from wherever into state.
  }

  render() {
    return (
      <Authorization allowed={this.props.allowed} user={this.state.user}>
        /* the rest of your page */
      </Authorization>
    )
  }
}

export default YourRoute

// Router configuration
() => (
  <Router history={BrowserHistory}>
    <Route path="/" component={App}>
      <Route allowed={['manager', 'admin']} path="feature" component={YourRoute} />
    </Route>
  </Router>
)
```

我们使用了一个 `Authorization` 组件，它有两个 props:

- `allowed` 用来判断请求是否被允许，
- `user` 存储了一些用户信息，包括权限信息。
