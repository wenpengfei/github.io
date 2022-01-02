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
1_000_000_000           // 10 亿
101_475_938.38          // And this is hundreds of millions

let fee = 123_00;       // $123 (12300 cents, apparently)
let fee = 12_300;       // $12,300 (woah, that fee!)
let amount = 12345_00;  // 12,345 (1234500 cents, apparently)
let amount = 123_4500;  // 123.45 (4-fixed financial)
let amount = 1_234_500; // 1,234,500
```

```javascript
0.000_001 // 1 millionth
1e10_000  // 10^10000 -- granted, far less useful / in-range...
0xA0_B0_C0;
```

## Promise.any 和聚合异常

```javascript
Promise.any([
  fetch('https://v8.dev/').then(() => 'home'),
  fetch('https://v8.dev/blog').then(() => 'blog'),
  fetch('https://v8.dev/docs').then(() => 'docs')
]).then((first) => {
  // Any of the promises was fulfilled.
  console.log(first);
  // → 'home'
}).catch((error) => {
  // All of the promises were rejected.
  console.log(error);
});
```

上面的 `error` 对象就是一个 `聚合异常`

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
使用 `Wea​​kRef` 类可以创建对对象的弱引用，在对象被垃圾收集后可以运行使用 `FinalizationRegistry` 创建的自定义终结器（finalizers），两者可以独立使用，也可以一起使用，具体的规范参考： [tc39/proposal-weakrefs](https://github.com/tc39/proposal-weakrefs)

```javascript
let target = {};
let wr = new WeakRef(target);

//wr and target aren't the same


// Creating a new registry
const registry = new FinalizationRegistry(heldValue => {
  // ....
});

registry.register(myObject, "some value", myObject);
// ...some time later, if you don't care about `myObject` anymore...
registry.unregister(myObject);
```

