---
title: 2022 年了，看几个 ES2021 的新特性
date: 2022-01-02 20:53:18
tags:
---

## 逻辑赋值运算符

```javascript
x || (x = y); // 旧写法
x ||= y; // 新写法

x && (x = y); // 旧写法
x &&= y; // 新写法

x ?? (x = y); // 旧写法
x ??= y; // 新写法
```

```javascript
const updateID = user => {
  // 我们想给 user.id 赋值，并且需要一些逻辑判断

  // 旧写法是这样的
  if (!user.id) user.id = 1

  // 或者这样
  user.id = user.id || 1

  // 新的逻辑赋值运算符这样写
  user.id ||= 1
}
```

```javascript
function setOpts(opts) {
  opts.cat ??= 'meow'
  opts.dog ??= 'bow';
}

setOpts({ cat: 'meow' })
```

## 数字分隔符
数字太长的话可读性就会变差，有了分隔符会好些

```javascript
1_000_000_000           
101_475_938.38 

let fee = 123_00;       // $123
let fee = 12_300;       // $12,300
let amount = 12345_00;  // 12,345
let amount = 123_4500;  // 123.45
let amount = 1_234_500; // 1,234,500
```

```javascript
0.000_001 // 1 millionth
1e10_000  // 10^10000 -- granted, far less useful / in-range...
0xA0_B0_C0;
```

## Promise.any 和 AggregateError

`Promise.any()` 接收一个 Promise 可迭代对象，只要其中的一个 Promise 成功，就返回那个已经成功的 Promise 。如果可迭代对象中没有一个 Promise 成功（即所有的 Promise 都失败/拒绝），就返回一个失败的 Promise 和 `AggregateError` 类型的实例，它是 Error 的一个子类，用于把单一的错误集合在一起。本质上，这个方法和 Promise.all() 是相反的。

```javascript
Promise.any([
  fetch('https://v8.dev/').then(() => 'home'),
  fetch('https://v8.dev/blog').then(() => 'blog'),
  fetch('https://v8.dev/docs').then(() => 'docs')
]).then((first) => {
  // 有任何一个成功的 Promise
  console.log(first);
  // → 'home'
}).catch((error) => {
  // 所有的 Promise 都失败
  console.log(error);
});
```

## String.prototype.replaceAll

```javascript
// String.prototype.replaceAll(searchValue, replaceValue)

'x'.replace('', '_');
// → '_x'

'xxx'.replace(/(?:)/g, '_');
// → '_x_x_x_'

'xxx'.replaceAll('', '_');
// → '_x_x_x_'
```

## WeakRefs 和 FinalizationRegistry 对象
使用 `Wea​​kRef` 类可以创建对对象的弱引用，在对象被垃圾收集后可以运行使用 `FinalizationRegistry` 创建的自定义终结器（finalizers），两者可以独立使用，也可以一起使用，具体的规范可以参考：[tc39/proposal-weakrefs](https://github.com/tc39/proposal-weakrefs)

```javascript
let target = {};
let wr = new WeakRef(target);

// wr 和 target 不相同


// 创建一个 FinalizationRegistry
const registry = new FinalizationRegistry(heldValue => {
  // ....
});

registry.register(myObject, "some value", myObject);
// 当你不再关心 myObject 对象
registry.unregister(myObject);
```

