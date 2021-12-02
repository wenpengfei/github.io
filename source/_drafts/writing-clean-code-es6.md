---
title: 写出整洁的 JavaScript 代码 ES6 版
tags:
---
整洁的代码不仅仅是可以跑起来的代码，而是可以被其他人轻松阅读、重用和重构的代码，因为代码除了实现功能外，大部分的时间都是要被你或是团队其他成员维护的。试想一下，在维护一个老项目时，花时间最长的是什么？写代码？调试？答案是 **「阅读其他人写的代码」**。

<img src="/images/writing-clean-code-es6/1.png" style="width:80%" />

虽然本文主要专注于编写干净整洁的 [JavaScript ES6](https://262.ecma-international.org/6.0/) 代码，并且不与任何框架相关，但是下面将要提到的绝大多数实例也适用于其他语言，另外，下面的示例也主要是从 Robert C. Martin 的书 [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) 中所采纳的建议，也不意味着要严格遵守。

# 变量

## 使用有意义的名字

变量的命名应该是描述性并且应该是有意义的，经验法则是大多数 JavaScript 变量应该使用`驼峰命名法`（camelCase）。

```javascript
// 错误 ❌
const foo = "JDoe@example.com";
const bar = "John";
const age = 23;
const qux = true;

// 正确 ✅
const email = "John@example.com";
const firstName = "John";
const age = 23;
const isActive = true
```

> 注意，布尔类型的变量名通常需要回答特定问题，例如：

```
isActive
didSubscribe
hasLinkedAccount
```

## 避免添加不必要的上下文（前后缀）

在特定的对象或类中不需要加冗余的上下文（前后缀）

```javascript
// 错误 ❌
const user = {
  userId: "296e2589-7b33-400a-b762-007b730c8e6d",
  userEmail: "JDoe@example.com",
  userFirstName: "John",
  userLastName: "Doe",
  userAge: 23,
};

user.userId;

// 正确 ✅
const user = {
  id: "296e2589-7b33-400a-b762-007b730c8e6d",
  email: "JDoe@example.com",
  firstName: "John",
  lastName: "Doe",
  age: 23,
};

user.id;
```

## 避免硬编码

确保声明有意义且可搜索的常量，而不是直接使用一个常量值，全局变量建议使用`蛇形命名法`（SCREAMING_SNAKE_CASE）

```javascript
// 错误 ❌
setTimeout(clearSessionData, 900000);

// 正确 ✅
const SESSION_DURATION_MS = 15 * 60 * 1000;

setTimeout(clearSessionData, SESSION_DURATION_MS);
```

# 函数

## 使用描述性的命名

函数名可以很长，长到足以描述它的作用，一般函数中都含有`动词`来描述它所做的事情，但是返回布尔值的函数是个例外，一般是一个回答“是”或者“否”的问题形式，另外函数命名也应该是驼峰命名法。

```javascript
// 错误 ❌
function toggle() {
  // ...
}

function agreed(user) {
  // ...
}

// 正确 ✅
function toggleThemeSwitcher() {
  // ...
}

function didAgreeToAllTerms(user) {
  // ...
}
```

## 使用默认参数

直接使用默认值比短路语法或者在函数中加入判断语句更加简洁，值得注意的是，短路适用于所有被认为是“假”的值，例如 `false`、`null`、`undefined`、`''`、`""`、`0` 和 `NaN`，而默认参数仅替换 `undefined`。

```javascript
// Don't ❌
function printAllFilesInDirectory(dir) {
  const directory = dir || "./";
  //   ...
}

// Do ✅
function printAllFilesInDirectory(dir = "./") {
  // ...
}
```

