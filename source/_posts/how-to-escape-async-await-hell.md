---
title: 如何避免 await/async 地狱
date: 2018-05-10 08:33:38
tags:
---

![img](/images/how-to-escape-async-await-hell/1.png)

> 原文地址：[How to escape async/await hell](https://medium.freecodecamp.org/avoiding-the-async-await-hell-c77a0fb71c4c)
> 译文出自：{% post_link how-to-escape-async-await-hell 夜色镇歌的个人博客 %}

async/await 把我们从回调地狱中解救了出来，但是如果滥用就会掉进 async/await 地狱。

本文中我会解释一下什么是 async/await 地狱，并会分享几个技巧去避免。

## 啥是 await/async 地狱

异步 Javascript 编程中，我们通常会写许多 async 方法，并且使用 `await` 关键字去等待它，有很多时候下一行的执行并不依赖于上一行，但是我们仍然使用了 `await` 去等待，所以可能会导致一些性能问题。

## 一个 await/async 地狱的例子

如何编写一个订购披萨和饮料的代码？它可能会像这样：

```javascript
(async () => {
  const pizzaData = await getPizzaData()    // async call
  const drinkData = await getDrinkData()    // async call
  const chosenPizza = choosePizza()    // sync call
  const chosenDrink = chooseDrink()    // sync call
  await addPizzaToCart(chosenPizza)    // async call
  await addDrinkToCart(chosenDrink)    // async call
  orderItems()    // async call
})()
```

看起来没什么问题，也能正常工作，但这并不是一个好的实现。先来看下这段代码都做了什么，以便定位问题。

## 解释下

我们把代码用 `async IIFE` 包裹了起来，然后下面这些会依次执行。

1. 获取披萨菜单
2. 获取饮料菜单
3. 从披萨菜单中选择披萨
4. 从饮料菜单中选择饮料
5. 把选好的披萨加到购物车
6. 把选好的饮料加到购物车
7. 下单

## 哪里错了？

正如我刚强调的，这些语句会**依次执行，没有并发**。仔细想一下，为啥我获取饮料菜单之前得先获取披萨菜单？这两份菜单我应该同时去获取。当然，选择披萨之前得先获取披萨菜单，这个规则同样适用于饮料。

所以我们可以得出结论，披萨相关的工作和饮料相关的工作可以并行进行，但涉及披萨相关工作的各个步骤需要按顺序进行（一步接着一步）。

## 另一个糟糕的例子

这段代码会获取购物车中的购物项并且发出订购请求。

```JavaScript
async function orderItems() {
  const items = await getCartItems()    // async call
  const noOfItems = items.length
  for(var i = 0; i < noOfItems; i++) {
    await sendRequest(items[i])    // async call
  }
}
```

这个例子中 for 循环在下一次迭代之前必须等待上一个 `sendRequest()` 执行完毕，可我们根本不需要等待，只想尽快的把请求都发送出去然后等待他们都完成。

想必现在你已经了解了什么是 async/await 地狱，以及它对性能的影响是多么的严重。现在我想问你个问题。

## 如果忘记了 await 关键字呢？

如果忘记使用 await，async 函数会执行并且返回一个 Promise，你可以稍后再去resolve。

```Javascript

(async () => {
  const value = doSomeAsyncTask()
  console.log(value) // an unresolved promise
})()
```

另一个后果是编译器不知道你想把函数完全执行，所以编译器会退出程序而不完成异步函数，所以还是需要使用 await 关键字

promises 一个有趣的特性就是你可以在一行代码中去得到 Promise ，而在另外一行中去等待并 resolve，这是避免 async/await 地狱的关键之处。

```Javascript
(async () => {
  const promise = doSomeAsyncTask()
  const value = await promise
  console.log(value) // the actual value
})()
```

正如你看到的，`doSomeAsyncTask()` 方法返回一个 Promise，调用的时候它已经开始执行了，为了得到他的解析值，我们使用了 await 关键字，告诉编译器等待解析完毕再执行下一行。

## 如何避免 async/await 地狱

你应该按照这些步骤来避免 async/await 地狱：

#### 找到语句的依赖关系

第一个例子中，我们选择了一个披萨和一杯饮料。总结一下，选择披萨之前得先获取披萨菜单，加到购物车之前得先选好，这三个步骤都是相互依赖的，必须等待上一个步骤完成后才能进行下一步。

我们选择饮料的时候并不依赖于选择披萨，所以选择披萨和饮料是可以并行执行的。这也是机器能比我们做的更好的一件事。

#### 封装相互依赖的异步方法

正如你看到的，选择披萨的依赖有获取披萨菜单、选择、添加到购物车。所以我们把这些依赖放在一个异步方法里，饮料同理，这也是为什么我们会有 `selectPizza()` 和 `selectDrink()` 两个异步方法。

#### 并行执行

我们利用事件循环去非阻塞并行地执行这些异步方法，**通常会用的两个方法就是尽早的返回 `Promise` 和使用 `Promise.all()`**

我们修复一下代码，把这三个方法应用到我们的例子中去。

## 修改下代码

```JavaScript
async function selectPizza() {
  const pizzaData = await getPizzaData()    // async call
  const chosenPizza = choosePizza()    // sync call
  await addPizzaToCart(chosenPizza)    // async call
}

async function selectDrink() {
  const drinkData = await getDrinkData()    // async call
  const chosenDrink = chooseDrink()    // sync call
  await addDrinkToCart(chosenDrink)    // async call
}

(async () => {
  const pizzaPromise = selectPizza()
  const drinkPromise = selectDrink()
  await pizzaPromise
  await drinkPromise
  orderItems()    // async call
})()

// Although I prefer it this way 

(async () => {
  Promise.all([selectPizza(), selectDrink()]).then(orderItems)   // async call
})()
```

我们把相互依赖的语句封装在各自的函数里，现在同时去执行 `selectPizza()` 和 `selectDrink()`

第二个例子中，我们需要处理未知数量的 `Promise` 。处理这种情况很简单，我们先把 Promises 放进数组，然后使用 `Promise.all()` 让他们并行执行，之后等待他们全都执行完毕。

```JavaScript
async function orderItems() {
  const items = await getCartItems()    // async call
  const noOfItems = items.length
  const promises = []
  for(var i = 0; i < noOfItems; i++) {
    const orderPromise = sendRequest(items[i])    // async call
    promises.push(orderPromise)    // sync call
  }
  await Promise.all(promises)    // async call
}

// Although I prefer it this way

async function orderItems() {
  const items = await getCartItems()    // async call
  const promises = items.map((item) => sendRequest(item))
  await Promise.all(promises)    // async call
}
```

希望本文可以引发你对 async/await 使用的思考，也希望能帮助你提升程序的性能。


