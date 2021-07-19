---
title: 使用Parcel、Typescript、Mobx撸一个简单的React脚手架
date: 2018-01-15 09:29:20
tags:
---
项目地址：[parcel-typescript-react-boilerplate](https://github.com/wenpengfei/parcel-typescript-react-boilerplate) 欢迎批评指正，顺手点个星呗，另外赠送一个代码生成器，根据表结构生成`单表`的CURD页面，然后根据实际的业务手动修改即可：[react-admin-generator](https://github.com/wenpengfei/react-admin-generator)

## Parcel

2017年底给开发者最大的惊喜非 `Parcel` 莫属了，使用过 `Webpack` 的同学都会有配置太过复杂的感受，甚至 `Webpack` 的维护者有时候做项目也会 `copy` 一些常用的配置。如果使用 `Parcel`，仅仅定义好 index.html、index.js和index.css 它就会为我们做好一切的打包工作，就像没有使用过任何打包工具一样，这样就很舒服。

`Webpack` 虽然复杂，但是很灵活，功能很全，`Parcel` 也需要为零配置付出很多代价，我们期待它更加完善。

## Mobx

状态管理是前端开发很重要的一个话题，在开发 `React` 应用的时候默认的状态管理肯定会选择 `Redux` 了，但是我选择使用 `Mobx`，`Redux` 受到函数式编程的影响，但很多人都有面向对象语言的背景，在刚开始的时候很难适应函数式编程的原则。所以 `Mobx` 对初学者更加友好。

`Mobx` 的原则很简单：任何源自应用状态的东西都应该自动地获得。怎么理解？比如我有一个变量 `a`，我的应用订阅了这个变量，当 `a` 改变的时候我的应用会自动收到最新的值。在 `React` 中的这个变量就是 `state`，当 `state` 改变，`mobx-react` 就会重新渲染视图。

## 技术栈

### 核心库

* [typescript](https://www.typescriptlang.org/)
* [react@v16](https://reactjs.org/)
* [react-router@v4](https://reacttraining.com/react-router/)
* [mobx](https://mobx.js.org/index.html)
* [mobx-react](https://github.com/mobxjs/mobx-react)

### 工具

* [parcel](https://parceljs.org)
* [tslint](https://palantir.github.io/tslint/)
* [stylelint](https://stylelint.io/)

## 目录结构

```
├── src
│   ├── app.scss
│   ├── app.tsx                      # 入口文件
│   ├── pages.tsx                    # 模板页
│   ├── routes.ts                    # 菜单
│   ├── assets                       # 资源文件
│   │   └── logo.svg
│   ├── components                   # 组件目录
│   │   ├── content-container        # 页面container，定义了淡入动画
│   │   │   └── index.tsx   
│   │   ├── index.tsx                # components 入口文件
│   │   ├── layout                   # 布局页
│   │   │   ├── index.scss
│   │   │   └── index.tsx
│   │   ├── login-form               # 登陆组件
│   │   │   ├── index.scss
│   │   │   └── index.tsx
│   │   └── router-link
│   │       └── index.tsx
│   ├── service                      # api
│   │   ├── base.ts
│   │   ├── index.ts                 # api 入口文件
│   │   └── user
│   │       └── index.ts
│   ├── stores                       # mobx store
│   │   ├── app
│   │   │   └── index.ts
│   │   ├── base.ts
│   │   ├── index.ts                 # store 入口文件 
│   │   ├── member
│   │   │   └── index.ts
│   │   └── user
│   │       └── index.ts
│   ├── utils                        # 工具类
│   │   ├── auth.ts
│   │   ├── fetch.ts
│   │   ├── index.ts                 # 工具类入口文件
│   │   └── validator.ts
│   └── views                        # 页面
│       ├── index.ts
│       ├── login
│       │   └── index.tsx
│       ├── member
│       │   └── index.tsx
│       ├── not-found
│       │   └── index.tsx
│       └── user
│           ├── add.tsx
│           ├── edit.tsx
│           ├── form.tsx
│           ├── index.tsx
│           ├── list.tsx
│           └── table.tsx
├── index.html
├── tsconfig.json
├── tslint.json
├── yarn-error.log
└── yarn.lock
├── README.md
├── package.json
```
