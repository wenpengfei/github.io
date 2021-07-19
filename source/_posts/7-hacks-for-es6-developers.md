---
title: ES6 的 7 个小技巧
date: 2018-02-01 11:19:06
tags:
---

## 1. 交换值

使用数组的解构交换值

```js
let a = 'world', b = 'hello'
[a, b] = [b, a]
console.log(a) // -> hello
console.log(b) // -> world
// Yes, it's magic
```

## 2. 解构 promises.all

```js
const [user, account] = await Promise.all([
  fetch('/user'),
  fetch('/account')
])
```

## 3. 调试

我们都喜欢用 `console.log` 调试，这里有个小技巧（我还听说过 `console.table`）

```js
const a = 5, b = 6, c = 7
console.log({ a, b, c })
// outputs this nice object:
// {
//    a: 5,
//    b: 6,
//    c: 7
// }
```

## 4. 封装

这么写可以显得代码更加紧凑

```js
// Find max value
const max = (arr) => Math.max(...arr);
max([123, 321, 32]) // outputs: 321
// Sum array
const sum = (arr) => arr.reduce((a, b) => (a + b), 0)
sum([1, 2, 3, 4]) // output: 10
```

## 5. 合并数组

可以使用展开运算符代替 `concat`

```js
const one = ['a', 'b', 'c']
const two = ['d', 'e', 'f']
const three = ['g', 'h', 'i']
// Old way #1
const result = one.concat(two, three)
// Old way #2
const result = [].concat(one, two, three)
// New
const result = [...one, ...two, ...three]
```

## 6. 拷贝

轻松地拷贝对象和数组

```js
const obj = { ...oldObj }
const arr = [ ...oldArr ]
```

注意：这种方式是浅拷贝

## 7. 命名参数

使函数和调用函数更加可读

```js
const getStuffNotBad = (id, force, verbose) => {
  ...do stuff
}
const getStuffAwesome = ({ id, name, force, verbose }) => {
  ...do stuff
}
// Somewhere else in the codebase... WTF is true, true?
getStuffNotBad(150, true, true)
// Somewhere else in the codebase... I ❤ JS!!!
getStuffAwesome({ id: 150, force: true, verbose: true })
```





