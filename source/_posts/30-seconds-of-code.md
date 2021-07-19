---
title: Javascript代码段合集
date: 2017-12-19 11:06:23
tags:
---

![img](/images/30-seconds-of-code/1.png)

> 花30秒或者更短的时间就能理解的Javascript代码段

- 可以使用`Ctrl + F` 或者 `command + F`搜索
- 代码段使用ES6编写，使用 [Babel transpiler](https://babeljs.io/) 保证兼容.
- 作者在持续更新[传送门](https://github.com/Chalarangelo/30-seconds-of-code)

## 目录

### Array
* [`arrayMax`](#arrayMax)
* [`arrayMin`](#arrayMin)
* [`chunk`](#chunk)
* [`compact`](#compact)
* [`countOccurrences`](#countOccurrences)
* [`deepFlatten`](#deepFlatten)
* [`difference`](#difference)
* [`distinctValuesOfArray`](#distinctValuesOfArray)
* [`dropElements`](#dropElements)
* [`everyNth`](#everyNth)
* [`filterNonUnique`](#filterNonUnique)
* [`flatten`](#flatten)
* [`flattenDepth`](#flattenDepth)
* [`groupBy`](#groupBy)
* [`head`](#head)
* [`initial`](#initial)
* [`initializeArrayWithRange`](#initializeArrayWithRange)
* [`initializeArrayWithValues`](#initializeArrayWithValues)
* [`intersection`](#intersection)
* [`last`](#last)
* [`mapObject`](#mapObject)
* [`nthElement`](#nthElement)
* [`pick`](#pick)
* [`pull`](#pull)
* [`remove`](#remove)
* [`sample`](#sample)
* [`shuffle`](#shuffle)
* [`similarity`](#similarity)
* [`symmetricDifference`](#symmetricDifference)
* [`tail`](#tail)
* [`take`](#take)
* [`takeRight`](#takeRight)
* [`union`](#union)
* [`without`](#without)
* [`zip`](#zip)

### Browser
* [`bottomVisible`](#bottomVisible)
* [`currentURL`](#currentURL)
* [`elementIsVisibleInViewport`](#elementIsVisibleInViewport)
* [`getScrollPosition`](#getScrollPosition)
* [`getURLParameters`](#getURLParameters)
* [`redirect`](#redirect)
* [`scrollToTop`](#scrollToTop)

### Date
* [`getDaysDiffBetweenDates`](#getDaysDiffBetweenDates)
* [`JSONToDate`](#JSONToDate)
* [`toEnglishDate`](#toEnglishDate)

### Function
* [`chainAsync`](#chainAsync)
* [`compose`](#compose)
* [`curry`](#curry)
* [`functionName`](#functionName)
* [`pipe`](#pipe)
* [`promisify`](#promisify)
* [`runPromisesInSeries`](#runPromisesInSeries)
* [`sleep`](#sleep)

### Math
* [`arrayAverage`](#arrayAverage)
* [`arraySum`](#arraySum)
* [`collatz`](#collatz)
* [`digitize`](#digitize)
* [`distance`](#distance)
* [`factorial`](#factorial)
* [`fibonacci`](#fibonacci)
* [`gcd`](#gcd)
* [`hammingDistance`](#hammingDistance)
* [`isDivisible`](#isDivisible)
* [`isEven`](#isEven)
* [`lcm`](#lcm)
* [`median`](#median)
* [`palindrome`](#palindrome)
* [`percentile`](#percentile)
* [`powerset`](#powerset)
* [`randomIntegerInRange`](#randomIntegerInRange)
* [`randomNumberInRange`](#randomNumberInRange)
* [`round`](#round)
* [`standardDeviation`](#standardDeviation)

### Media
* [`speechSynthesis`](#speechSynthesis)

### Node
* [`JSONToFile`](#JSONToFile)
* [`readFileLines`](#readFileLines)

### Object
* [`cleanObj`](#cleanObj)
* [`objectFromPairs`](#objectFromPairs)
* [`objectToPairs`](#objectToPairs)
* [`shallowClone`](#shallowClone)
* [`truthCheckCollection`](#truthCheckCollection)

### String
* [`anagrams`](#anagrams)
* [`capitalize`](#capitalize)
* [`capitalizeEveryWord`](#capitalizeEveryWord)
* [`escapeRegExp`](#escapeRegExp)
* [`fromCamelCase`](#fromCamelCase)
* [`reverseString`](#reverseString)
* [`sortCharactersInString`](#sortCharactersInString)
* [`toCamelCase`](#toCamelCase)
* [`truncateString`](#truncateString)

### Utility
* [`coalesce`](#coalesce)
* [`coalesceFactory`](#coalesceFactory)
* [`extendHex`](#extendHex)
* [`getType`](#getType)
* [`hexToRGB`](#hexToRGB)
* [`isArray`](#isArray)
* [`isBoolean`](#isBoolean)
* [`isFunction`](#isFunction)
* [`isNumber`](#isNumber)
* [`isString`](#isString)
* [`isSymbol`](#isSymbol)
* [`RGBToHex`](#RGBToHex)
* [`timeTaken`](#timeTaken)
* [`toOrdinalSuffix`](#toOrdinalSuffix)
* [`UUIDGenerator`](#UUIDGenerator)
* [`validateEmail`](#validateEmail)
* [`validateNumber`](#validateNumber)

## Array

### arrayMax

返回数组中的最大值.

使用 `Math.max()` 配合展开操作符 (`...`) 得到数组中的最大值.

```js
const arrayMax = arr => Math.max(...arr);
// arrayMax([10, 1, 5]) -> 10
```

[⬆ back to top](#目录)

### arrayMin

返回数组中的最小值.

使用 `Math.min()` 配合展开操作符 (`...`) 得到数组中的最小值.

```js
const arrayMin = arr => Math.min(...arr);
// arrayMin([10, 1, 5]) -> 1
```

[⬆ back to top](#目录)

### chunk

将一个数组分割成几个数组段.

使用 `Array.from()` 创建一个适合它长度的新的数组
使用 `Array.slice()` 分割为指定 `size` 长度的数组段
如果指定的数组不能被平均分割，最后的块将包含剩余的元素。

```js
const chunk = (arr, size) =>
  Array.from({length: Math.ceil(arr.length / size)}, (v, i) => arr.slice(i * size, i * size + size));
// chunk([1,2,3,4,5], 2) -> [[1,2],[3,4],[5]]
```

[⬆ back to top](#目录)

### compact

移除数组中的非真值

使用 `Array.filter()` 过滤非真值 (`false`, `null`, `0`, `""`, `undefined`, 和 `NaN`).

```js
const compact = (arr) => arr.filter(Boolean);
// compact([0, 1, false, 2, '', 3, 'a', 'e'*23, NaN, 's', 34]) -> [ 1, 2, 3, 'a', 's', 34 ]
```

[⬆ back to top](#目录)

### countOccurrences

计算元素出现的次数.

使用 `Array.reduce()` 计算指定元素在数组中出现的次数

```js
const countOccurrences = (arr, value) => arr.reduce((a, v) => v === value ? a + 1 : a + 0, 0);
// countOccurrences([1,1,2,1,2,3], 1) -> 3
```

[⬆ back to top](#目录)

### deepFlatten

深度降维

使用递归.
使用 `Array.concat()` 和一个空数组 (`[]`) 还有展开运算符 (`...`) 降维一个多维数组.

```js
const deepFlatten = arr => [].concat(...arr.map(v => Array.isArray(v) ? deepFlatten(v) : v));
// deepFlatten([1,[2],[[3],4],5]) -> [1,2,3,4,5]
```

[⬆ back to top](#目录)

### difference

返回两个数组的差集

创建一个 `b` 的 `Set`, 然后使用 `Array.filter()` 查找 `a` 中不包含 `b`的元素.

```js
const difference = (a, b) => { const s = new Set(b); return a.filter(x => !s.has(x)); };
// difference([1,2,3], [1,2,4]) -> [3]
```

[⬆ back to top](#目录)

### distinctValuesOfArray

返回数组中不重复的元素

使用 ES6的 `Set` 和展开运算符 `...rest` 过滤重复的元素.

```js
const distinctValuesOfArray = arr => [...new Set(arr)];
// distinctValuesOfArray([1,2,2,3,4,4,5]) -> [1,2,3,4,5]
```

[⬆ back to top](#目录)

### dropElements

给函数传递一个表达式和数组，只保留表达式为true的元素

```js
const dropElements = (arr, func) => {
  while (arr.length > 0 && !func(arr[0])) arr.shift();
  return arr;
};
// dropElements([1, 2, 3, 4], n => n >= 3) -> [3,4]
```

[⬆ back to top](#目录)

### everyNth

返回数组中每一个第n的元素.

使用 `Array.filter()` 返回每一个第n的元素.

```js
const everyNth = (arr, nth) => arr.filter((e, i) => i % nth === 0);
// everyNth([1,2,3,4,5,6], 2) -> [ 1, 3, 5 ]
```

[⬆ back to top](#目录)

### filterNonUnique

过滤不唯一的元素.

使用 `Array.filter()` 只保留唯一的元素.

```js
const filterNonUnique = arr => arr.filter(i => arr.indexOf(i) === arr.lastIndexOf(i));
// filterNonUnique([1,2,2,3,4,4,5]) -> [1,3,5]
```

[⬆ back to top](#目录)

### flatten

降维数组.

使用 `Array.reduce()` 获取到每一个元素然后使用 `concat()` 降维.

```js
const flatten = arr => arr.reduce((a, v) => a.concat(v), []);
// flatten([1,[2],3,4]) -> [1,2,3,4]
```

[⬆ back to top](#目录)

### flattenDepth

根据指定的深度降维数组.

使用递归，为所有维度的数组降低一维.
使用 `Array.reduce()` 和 `Array.concat()` 合并降维后的数组或元素.
此时如果 `depth` 为 `1` 停止递归.

```js
const flattenDepth = (arr, depth = 1) =>
  depth != 1 ? arr.reduce((a, v) => a.concat(Array.isArray(v) ? flattenDepth(v, depth - 1) : v), [])
  : arr.reduce((a, v) => a.concat(v), []);
// flattenDepth([1,[2],[[[3],4],5]], 2) -> [1,2,[3],4,5]
```

[⬆ back to top](#目录)

### groupBy

根据指定的表达式为数组分组

使用 `Array.map()` 映射为根据表达式或属性名值计算后的数组
使用 `Array.reduce()` 创建一个键值是上一步map出来的结果，值是相对应的数组的对象

```js
const groupBy = (arr, func) =>
  arr.map(typeof func === 'function' ? func : val => val[func])
    .reduce((acc, val, i) => { acc[val] = (acc[val] || []).concat(arr[i]); return acc; }, {});
// groupBy([6.1, 4.2, 6.3], Math.floor) -> {4: [4.2], 6: [6.1, 6.3]}
// groupBy(['one', 'two', 'three'], 'length') -> {3: ['one', 'two'], 5: ['three']}
```

[⬆ back to top](#目录)

### head

返回集合的第一个元素

使用 `arr[0]` 返回给定数组的第一个元素.

```js
const head = arr => arr[0];
// head([1,2,3]) -> 1
```

[⬆ back to top](#目录)

### initial

返回一个数组中除去最后一个元素的其他元素.

使用 `arr.slice(0,-1)` 返回除去最后一个元素的其他元素.

```js
const initial = arr => arr.slice(0, -1);
// initial([1,2,3]) -> [1,2]
```

[⬆ back to top](#目录)

### initializeArrayWithRange

初始化一个指定范围的数组

使用 `Array(end-start)` 创建一个期望长度的数组, 根据给定的范围使用`Array.map()`填充数组.
参数`start` 默认值为 `0`.

```js
const initializeArrayWithRange = (end, start = 0) =>
  Array.from({ length: end - start }).map((v, i) => i + start);
// initializeArrayWithRange(5) -> [0,1,2,3,4]
```

[⬆ back to top](#目录)

### initializeArrayWithValues

初始化并且根据给定的值填充数组.

使用 `Array(n)` 创建一个期望长度的数组, 根据给定的值使用 `fill(v)` 填充数组.
参数 `value` 默认值为 `0`.

```js
const initializeArrayWithValues = (n, value = 0) => Array(n).fill(value);
// initializeArrayWithValues(5, 2) -> [2,2,2,2,2]
```

[⬆ back to top](#目录)

### intersection

返回两个数组的交集.

创建一个 `b` 的 `Set`, 然后使用 `a` 的 `Array.filter()` 查找含 `b` 元素.

```js
const intersection = (a, b) => { const s = new Set(b); return a.filter(x => s.has(x)); };
// intersection([1,2,3], [4,3,2]) -> [2,3]
```

[⬆ back to top](#目录)

### last

返回数组中的最后一个元素.

使用 `arr.length - 1` 计算出最后一个元素的索引，然后返回它的值.

```js
const last = arr => arr[arr.length - 1];
// last([1,2,3]) -> 3
```

[⬆ back to top](#目录)

### mapObject

映射一个数组，结果是键值为他的每一个元素的值，值为给定表达式结果的对象

```js
const mapObject = (arr, fn) => 
  (a => (a = [arr, arr.map(fn)], a[0].reduce( (acc,val,ind) => (acc[val] = a[1][ind], acc), {}) )) ( );
/*
const squareIt = arr => mapObject(arr, a => a*a)
squareIt([1,2,3]) // { 1: 1, 2: 4, 3: 9 }
*/
```

[⬆ back to top](#目录)

### nthElement

返回数组的第n个对象.

使用 `Array.slice()` 获取满足给定条件的数组的第一个元素
如果给定的索引超出范围，返回 `[]`.
参数 `n` 默认为第一个元素

```js
const nthElement = (arr, n=0) => (n>0? arr.slice(n,n+1) : arr.slice(n))[0];
// nthElement(['a','b','c'],1) -> 'b'
// nthElement(['a','b','b'],-3) -> 'a'
```

[⬆ back to top](#目录)

### pick

返回对象的一个拷贝，返回的对象只含有给定的键的键值对

```js
const pick = (obj, arr) =>
  arr.reduce((acc, curr) => (curr in obj && (acc[curr] = obj[curr]), acc), {});
// pick({ 'a': 1, 'b': '2', 'c': 3 }, ['a', 'c']) -> { 'a': 1, 'c': 3 }
```

[⬆ back to top](#目录)

### pull

抽取数组中指定的元素

使用 `Array.filter()` 和 `Array.includes()` 抽出不需要的元素.
使用 `Array.length = 0` 重置数组并且使用 `Array.push()` 重新填充抽取后的数组.

```js
const pull = (arr, ...args) => {
  let pulled = arr.filter((v, i) => !args.includes(v));
  arr.length = 0; pulled.forEach(v => arr.push(v));
};
// let myArray = ['a', 'b', 'c', 'a', 'b', 'c'];
// pull(myArray, 'a', 'c');
// console.log(myArray) -> [ 'b', 'b' ]
```

[⬆ back to top](#目录)

### remove

移除数组中给定表达式为 `false`. 的值

使用 `Array.filter()` 找到表达式为 `true` 的值，然后通过 `Array.reduce()` 使用 `Array.splice()` 移除.

```js
const remove = (arr, func) =>
  Array.isArray(arr) ? arr.filter(func).reduce((acc, val) => {
    arr.splice(arr.indexOf(val), 1); return acc.concat(val);
    }, [])
  : [];
// remove([1, 2, 3, 4], n => n % 2 == 0) -> [2, 4]
```

[⬆ back to top](#目录)

### sample

返回数组的一个随机元素

使用 `Math.random()` 创建一个随机数，然后和 `length` 相乘之后通过 `Math.floor()` 找到一个最接近的数.

```js
const sample = arr => arr[Math.floor(Math.random() * arr.length)];
// sample([3, 7, 9, 11]) -> 9
```

[⬆ back to top](#目录)

### shuffle

打乱数组中值的顺序

使用 `Array.sort()` 重新排序, 使用 `Math.random() - 0.5` 作为`compareFunction`.

```js
const shuffle = arr => arr.sort(() => Math.random() - 0.5);
// shuffle([1,2,3]) -> [2,3,1]
```

[⬆ back to top](#目录)

### similarity

返回一个数组，它的值两个数组里面都存在.

使用 `includes()` 找出`values`不含有的元素，使用`filter()`移除.

```js
const similarity = (arr, values) => arr.filter(v => values.includes(v));
// similarity([1,2,3], [1,2,4]) -> [1,2]
```

[⬆ back to top](#目录)

### symmetricDifference

返回两个数组的对称差异.

通过两个数组分别创建 `Set`, 然后使用 `Array.filter()` 找出不在另外一个集合中的元素.

```js
const symmetricDifference = (a, b) => {
  const sA = new Set(a), sB = new Set(b);
  return [...a.filter(x => !sB.has(x)), ...b.filter(x => !sA.has(x))];
}
// symmetricDifference([1,2,3], [1,2,4]) -> [3,4]
```

[⬆ back to top](#目录)

### tail

返回数组中除去第一个元素的集合

如果数组`length` 大于 `1`, 返回 `arr.slice(1)` 否则就返回整个数组.

```js
const tail = arr => arr.length > 1 ? arr.slice(1) : arr;
// tail([1,2,3]) -> [2,3]
// tail([1]) -> [1]
```

[⬆ back to top](#目录)

### take

返回前n个元素.

```js
const take = (arr, n = 1) => arr.slice(0, n);
// take([1, 2, 3], 5) -> [1, 2, 3]
// take([1, 2, 3], 0) -> []
```

[⬆ back to top](#目录)

### takeRight

返回后n个元素.

```js
const takeRight = (arr, n = 1) => arr.slice(arr.length - n, arr.length);
// takeRight([1, 2, 3], 2) -> [ 2, 3 ]
// takeRight([1, 2, 3]) -> [3]
```

[⬆ back to top](#目录)

### union

合并两个集合（结果不含重复元素）

```js
const union = (a, b) => Array.from(new Set([...a, ...b]));
// union([1,2,3], [4,3,2]) -> [1,2,3,4]
```

[⬆ back to top](#目录)

### without

根据指定的值过滤数组

```js
const without = (arr, ...args) => arr.filter(v => !args.includes(v));
// without([2, 1, 2, 3], 1, 2) -> [3]
```

[⬆ back to top](#目录)

### zip

根据原始数组的位置把多个数组压缩

使用 `Math.max.apply()` 获取输入数组中最大的长度，根据这个长度使用`Array.from()`创建一个新的数组，之后把输入数组的映射压缩到里面，如果某个数组缺少元素使用`undefined`代替

```js
const zip = (...arrays) => {
  const maxLength = Math.max(...arrays.map(x => x.length));
  return Array.from({length: maxLength}).map((_, i) => {
   return Array.from({length: arrays.length}, (_, k) => arrays[k][i]);
  })
}
//zip(['a', 'b'], [1, 2], [true, false]); -> [['a', 1, true], ['b', 2, false]]
//zip(['a'], [1, 2], [true, false]); -> [['a', 1, true], [undefined, 2, false]]
```

[⬆ back to top](#目录)
## Browser

### bottomVisible

如果到达页面底部，返回`true`否则返回`false`

使用 `scrollY`, `scrollHeight` 和 `clientHeight` 判断是否到达页面底部

```js
const bottomVisible = () =>
  document.documentElement.clientHeight + window.scrollY >= document.documentElement.scrollHeight || document.documentElement.clientHeight;
// bottomVisible() -> true
```

[⬆ back to top](#目录)

### currentURL

返回当前页面的URL.

使用 `window.location.href` 获取当前页面URL.

```js
const currentURL = () => window.location.href;
// currentUrl() -> 'https://google.com'
```

[⬆ back to top](#目录)

### elementIsVisibleInViewport

如果一个元素在视口可见，返回`true`否则返回`false`

使用 `Element.getBoundingClientRect()` 和 `window.inner(Width|Height)` 判断元素是否在视口可见，第二个参数设置为`true`表示是否部分可见，默认值为`false`

```js
const elementIsVisibleInViewport = (el, partiallyVisible = false) => {
  const { top, left, bottom, right } = el.getBoundingClientRect();
  return partiallyVisible
    ? ((top > 0 && top < innerHeight) || (bottom > 0 && bottom < innerHeight)) &&
      ((left > 0 && left < innerWidth) || (right > 0 && right < innerWidth))
    : top >= 0 && left >= 0 && bottom <= innerHeight && right <= innerWidth;
};
// e.g. 100x100 viewport and a 10x10px element at position {top: -1, left: 0, bottom: 9, right: 10}
// elementIsVisibleInViewport(el) -> false (not fully visible)
// elementIsVisibleInViewport(el, true) -> true (partially visible)
```

[⬆ back to top](#目录)

### getScrollPosition

返回滚动条在当前页面的位置.

如果 `pageXOffset` 和 `pageYOffset` 未定义，使用 `scrollLeft` and `scrollTop`.

```js
const getScrollPosition = (el = window) =>
  ({x: (el.pageXOffset !== undefined) ? el.pageXOffset : el.scrollLeft,
    y: (el.pageYOffset !== undefined) ? el.pageYOffset : el.scrollTop});
// getScrollPosition() -> {x: 0, y: 200}
```

[⬆ back to top](#目录)

### getURLParameters

返回URL查询字符串对象.

```js
const getURLParameters = url =>
  url.match(/([^?=&]+)(=([^&]*))/g).reduce(
    (a, v) => (a[v.slice(0, v.indexOf('='))] = v.slice(v.indexOf('=') + 1), a), {}
  );
// getURLParameters('http://url.com/page?name=Adam&surname=Smith') -> {name: 'Adam', surname: 'Smith'}
```

[⬆ back to top](#目录)

### redirect

重定向到指定的URL.

```js
const redirect = (url, asLink = true) =>
  asLink ? window.location.href = url : window.location.replace(url);
// redirect('https://google.com')
```

[⬆ back to top](#目录)

### scrollToTop

平滑滚动到页面顶部.

```js
const scrollToTop = () => {
  const c = document.documentElement.scrollTop || document.body.scrollTop;
  if (c > 0) {
    window.requestAnimationFrame(scrollToTop);
    window.scrollTo(0, c - c / 8);
  }
};
// scrollToTop()
```

[⬆ back to top](#目录)
## Date

### getDaysDiffBetweenDates

返回两个Date对象的天数差

```js
const getDaysDiffBetweenDates = (dateInitial, dateFinal) => (dateFinal - dateInitial) / (1000 * 3600 * 24);
// getDaysDiffBetweenDates(new Date("2017-12-13"), new Date("2017-12-22")) -> 9
```

[⬆ back to top](#目录)

### JSONToDate

转换一个JSON对象为时间.

```js
const JSONToDate = arr => {
  const dt = new Date(parseInt(arr.toString().substr(6)));
  return `${ dt.getDate() }/${ dt.getMonth() + 1 }/${ dt.getFullYear() }`
};
// JSONToDate(/Date(1489525200000)/) -> "14/3/2017"
```

[⬆ back to top](#目录)

### toEnglishDate

把美国时间转换为英国时间.

```js
const toEnglishDate  = (time) =>
  {try{return new Date(time).toISOString().split('T')[0].replace(/-/g, '/')}catch(e){return}};
// toEnglishDate('09/21/2010') -> '21/09/2010'
```

[⬆ back to top](#目录)
## Function

### chainAsync

串联异步方法.

```js
const chainAsync = fns => { let curr = 0; const next = () => fns[curr++](next); next(); };
/*
chainAsync([
  next => { console.log('0 seconds'); setTimeout(next, 1000); },
  next => { console.log('1 second');  setTimeout(next, 1000); },
  next => { console.log('2 seconds'); }
])
*/
```

[⬆ back to top](#目录)

### compose

从右往左执行函数组合

```js
const compose = (...fns) => fns.reduce((f, g) => (...args) => f(g(...args)));
/*
const add5 = x => x + 5
const multiply = (x, y) => x * y
const multiplyAndAdd5 = compose(add5, multiply)
multiplyAndAdd5(5, 2) -> 15
*/
```

[⬆ back to top](#目录)

### curry

对函数进行柯里化

```js
const curry = (fn, arity = fn.length, ...args) =>
  arity <= args.length
    ? fn(...args)
    : curry.bind(null, fn, arity, ...args);
// curry(Math.pow)(2)(10) -> 1024
// curry(Math.min, 3)(10)(50)(2) -> 2
```

[⬆ back to top](#目录)

### functionName

打印函数名称

```js
const functionName = fn => (console.debug(fn.name), fn);
// functionName(Math.max) -> max (logged in debug channel of console)
```

[⬆ back to top](#目录)

### pipe

从左往右执行函数组合


```js
const pipeFunctions = (...fns) => fns.reduce((f, g) => (...args) => g(f(...args)));
/*
const add5 = x => x + 5
const multiply = (x, y) => x * y
const multiplyAndAdd5 = pipeFunctions(multiply, add5)
multiplyAndAdd5(5, 2) -> 15
*/
```

[⬆ back to top](#目录)

### promisify

把异步函数转化为promise

*In Node 8+, you can use [`util.promisify`](https://nodejs.org/api/util.html#util_util_promisify_original)*

```js
const promisify = func =>
  (...args) =>
    new Promise((resolve, reject) =>
      func(...args, (err, result) =>
        err ? reject(err) : resolve(result))
    );
// const delay = promisify((d, cb) => setTimeout(cb, d))
// delay(2000).then(() => console.log('Hi!')) -> Promise resolves after 2s
```

[⬆ back to top](#目录)

### runPromisesInSeries

执行一系列promise函数

```js
const runPromisesInSeries = ps => ps.reduce((p, next) => p.then(next), Promise.resolve());
// const delay = (d) => new Promise(r => setTimeout(r, d))
// runPromisesInSeries([() => delay(1000), () => delay(2000)]) -> executes each promise sequentially, taking a total of 3 seconds to complete
```

[⬆ back to top](#目录)

### sleep

延迟执行异步函数

Delay executing part of an `async` function, by putting it to sleep, returning a `Promise`.

```js
const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));
/*
async function sleepyWork() {
  console.log('I\'m going to sleep for 1 second.');
  await sleep(1000);
  console.log('I woke up after 1 second.');
}
*/
```

[⬆ back to top](#目录)
## Math

### arrayAverage

返回数组的平均值

```js
const arrayAverage = arr => arr.reduce((acc, val) => acc + val, 0) / arr.length;
// arrayAverage([1,2,3]) -> 2
```

[⬆ back to top](#目录)

### arraySum

返回数组的和

```js
const arraySum = arr => arr.reduce((acc, val) => acc + val, 0);
// arraySum([1,2,3,4]) -> 10
```

[⬆ back to top](#目录)

### collatz

实现Collatz算法.

如果 `n` 是偶数, 返回 `n/2`. 否则返回 `3n+1`.

```js
const collatz = n => (n % 2 == 0) ? (n / 2) : (3 * n + 1);
// collatz(8) --> 4
// collatz(5) --> 16
```

[⬆ back to top](#目录)

### digitize

把数字转为数组

```js
const digitize = n => [...''+n].map(i => parseInt(i));
// digitize(2334) -> [2, 3, 3, 4]
```

[⬆ back to top](#目录)

### distance

返回两点距离.

```js
const distance = (x0, y0, x1, y1) => Math.hypot(x1 - x0, y1 - y0);
// distance(1,1, 2,3) -> 2.23606797749979
```

[⬆ back to top](#目录)

### factorial

计算一个数字的阶乘.

```js
const factorial = n =>
  n < 0 ? (() => { throw new TypeError('Negative numbers are not allowed!') })()
  : n <= 1 ? 1 : n * factorial(n - 1);
// factorial(6) -> 720
```

[⬆ back to top](#目录)

### fibonacci

指定一个长度，输出斐波那契数列

```js
const fibonacci = n =>
  Array(n).fill(0).reduce((acc, val, i) => acc.concat(i > 1 ? acc[i - 1] + acc[i - 2] : i), []);
// fibonacci(5) -> [0,1,1,2,3]
```

[⬆ back to top](#目录)

### gcd

计算两个数字之间的最大公约数。

```js
const gcd = (x, y) => !y ? x : gcd(y, x % y);
// gcd (8, 36) -> 4
```

[⬆ back to top](#目录)

### hammingDistance

计算两个值的Hamming距离.

```js
const hammingDistance = (num1, num2) =>
  ((num1 ^ num2).toString(2).match(/1/g) || '').length;
// hammingDistance(2,3) -> 1
```

[⬆ back to top](#目录)

### isDivisible

检查第一个数字是否可被第二个数字整除.

```js
const isDivisible = (dividend, divisor) => dividend % divisor === 0;
// isDivisible(6,3) -> true
```

[⬆ back to top](#目录)

### isEven

检查数字是否为偶数

```js
const isEven = num => num % 2 === 0;
// isEven(3) -> false
```

[⬆ back to top](#目录)

### lcm

计算两个数字的最小公倍数.

```js
const lcm = (x,y) => {
  const gcd = (x, y) => !y ? x : gcd(y, x % y);
  return Math.abs(x*y)/(gcd(x,y));
};
// lcm(12,7) -> 84
```

[⬆ back to top](#目录)

### median

返回数组的中位数

```js
const median = arr => {
  const mid = Math.floor(arr.length / 2), nums = arr.sort((a, b) => a - b);
  return arr.length % 2 !== 0 ? nums[mid] : (nums[mid - 1] + nums[mid]) / 2;
};
// median([5,6,50,1,-5]) -> 5
// median([0,10,-2,7]) -> 3.5
```

[⬆ back to top](#目录)

### palindrome

判断给定字符串是否是回文字符串（回文字符串是正读和反读都一样的字符串，比如“level”或者“noon”）

```js
const palindrome = str => {
  const s = str.toLowerCase().replace(/[\W_]/g,'');
  return s === s.split('').reverse().join('');
}
// palindrome('taco cat') -> true
 ```

[⬆ back to top](#目录)

### percentile

使用百分位数公式来计算给定数组中有多少数字小于或等于给定值。

```js
const percentile = (arr, val) =>
  100 * arr.reduce((acc,v) => acc + (v < val ? 1 : 0) + (v === val ? 0.5 : 0), 0) / arr.length;
// percentile([1,2,3,4,5,6,7,8,9,10], 6) -> 55
 ```

[⬆ back to top](#目录)

### powerset

输出给定数组的所有子集

```js
const powerset = arr =>
  arr.reduce((a, v) => a.concat(a.map(r => [v].concat(r))), [[]]);
// powerset([1,2]) -> [[], [1], [2], [2,1]]
```

[⬆ back to top](#目录)

### randomIntegerInRange

返回指定范围内的随机整数

```js
const randomIntegerInRange = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;
// randomIntegerInRange(0, 5) -> 2
```

[⬆ back to top](#目录)

### randomNumberInRange

返回指定范围内的随机数

```js
const randomNumberInRange = (min, max) => Math.random() * (max - min) + min;
// randomNumberInRange(2,10) -> 6.0211363285087005
```

[⬆ back to top](#目录)

### round

将数字四舍五入到指定的数字位数.

```js
const round = (n, decimals=0) => Number(`${Math.round(`${n}e${decimals}`)}e-${decimals}`);
// round(1.005, 2) -> 1.01
```

[⬆ back to top](#目录)

### standardDeviation

返回数组的标准差

```js
const standardDeviation = (arr, usePopulation = false) => {
  const mean = arr.reduce((acc, val) => acc + val, 0) / arr.length;
  return Math.sqrt(
    arr.reduce((acc, val) => acc.concat(Math.pow(val - mean, 2)), [])
       .reduce((acc, val) => acc + val, 0) / (arr.length - (usePopulation ? 0 : 1))
  );
};
// standardDeviation([10,2,38,23,38,23,21]) -> 13.284434142114991 (sample)
// standardDeviation([10,2,38,23,38,23,21], true) -> 12.29899614287479 (population)
```

[⬆ back to top](#目录)
## Media

### speechSynthesis

语音合成 (实验特性).

详情查看 [SpeechSynthesisUtterance interface of the Web Speech API](https://developer.mozilla.org/en-US/docs/Web/API/SpeechSynthesisUtterance).

```js
const speechSynthesis = message => {
  const msg = new SpeechSynthesisUtterance(message);
  msg.voice = window.speechSynthesis.getVoices()[0];
  window.speechSynthesis.speak(msg);
};
// speechSynthesis('Hello, World') -> plays the message
```

[⬆ back to top](#目录)
## Node

### JSONToFile
将一个JSON对象转换为文件.

```js
const fs = require('fs');
const JSONToFile = (obj, filename) => fs.writeFile(`${filename}.json`, JSON.stringify(obj, null, 2))
// JSONToFile({test: "is passed"}, 'testJsonFile') -> writes the object to 'testJsonFile.json'
```

[⬆ back to top](#目录)

### readFileLines

读取指定的文件并且根据行生成数组

  ```js
const fs = require('fs');
const readFileLines = filename => fs.readFileSync(filename).toString('UTF8').split('\n');
/*
  contents of test.txt :
    line1
    line2
    line3
    ___________________________
  let arr = readFileLines('test.txt')
  console.log(arr) // -> ['line1', 'line2', 'line3']
 */
```

[⬆ back to top](#目录)
## Object

### cleanObj

移除对象中除去给定的属性名之外的属性

```js
const cleanObj = (obj, keysToKeep = [], childIndicator) => {
  Object.keys(obj).forEach(key => {
    if (key === childIndicator) {
      cleanObj(obj[key], keysToKeep, childIndicator);
    } else if (!keysToKeep.includes(key)) {
      delete obj[key];
    }
  })
}
/*
  const testObj = {a: 1, b: 2, children: {a: 1, b: 2}}
  cleanObj(testObj, ["a"],"children")
  console.log(testObj)// { a: 1, children : { a: 1}}
*/
```

[⬆ back to top](#目录)

### objectFromPairs

根据给定的键值对生成对象

```js
const objectFromPairs = arr => arr.reduce((a, v) => (a[v[0]] = v[1], a), {});
// objectFromPairs([['a',1],['b',2]]) -> {a: 1, b: 2}
```

[⬆ back to top](#目录)

### objectToPairs

转换一个对象为数组

```js
const objectToPairs = obj => Object.keys(obj).map(k => [k, obj[k]]);
// objectToPairs({a: 1, b: 2}) -> [['a',1],['b',2]])
```

[⬆ back to top](#目录)

### shallowClone

创建一个对象的浅拷贝.

```js
const shallowClone = obj => Object.assign({}, obj);
/*
const a = { x: true, y: 1 };
const b = shallowClone(a);
a === b -> false
*/
```

[⬆ back to top](#目录)

### truthCheckCollection

检查某个属性名是否在一个数组中都存在
 
 ```js
truthCheckCollection = (collection, pre) => (collection.every(obj => obj[pre]));
// truthCheckCollection([{"user": "Tinky-Winky", "sex": "male"}, {"user": "Dipsy", "sex": "male"}], "sex") -> true
```

[⬆ back to top](#目录)
## String

### anagrams

生成一个字符串的所有字符排列组合（包含重复）

```js
const anagrams = str => {
  if (str.length <= 2) return str.length === 2 ? [str, str[1] + str[0]] : [str];
  return str.split('').reduce((acc, letter, i) =>
    acc.concat(anagrams(str.slice(0, i) + str.slice(i + 1)).map(val => letter + val)), []);
};
// anagrams('abc') -> ['abc','acb','bac','bca','cab','cba']
```

[⬆ back to top](#目录)

### Capitalize

将给定字符串首字母大写.

```js
const capitalize = ([first,...rest], lowerRest = false) =>
  first.toUpperCase() + (lowerRest ? rest.join('').toLowerCase() : rest.join(''));
// capitalize('myName') -> 'MyName'
// capitalize('myName', true) -> 'Myname'
```

[⬆ back to top](#目录)

### capitalizeEveryWord

将给定字符串的每个单词首字母大写.

```js
const capitalizeEveryWord = str => str.replace(/\b[a-z]/g, char => char.toUpperCase());
// capitalizeEveryWord('hello world!') -> 'Hello World!'
```

[⬆ back to top](#目录)

### escapeRegExp

转义字符串以便在正则表达式中使用

```js
const escapeRegExp = str => str.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
// escapeRegExp('(test)') -> \\(test\\)
```

[⬆ back to top](#目录)

### fromCamelCase

把camelcase字符串转换成其他格式

```js
const fromCamelCase = (str, separator = '_') =>
  str.replace(/([a-z\d])([A-Z])/g, '$1' + separator + '$2')
    .replace(/([A-Z]+)([A-Z][a-z\d]+)/g, '$1' + separator + '$2').toLowerCase();
// fromCamelCase('someDatabaseFieldName', ' ') -> 'some database field name'
// fromCamelCase('someLabelThatNeedsToBeCamelized', '-') -> 'some-label-that-needs-to-be-camelized'
// fromCamelCase('someJavascriptProperty', '_') -> 'some_javascript_property'
```

[⬆ back to top](#目录)

### reverseString

反转字符串

```js
const reverseString = str => [...str].reverse().join('');
// reverseString('foobar') -> 'raboof'
```

[⬆ back to top](#目录)

### sortCharactersInString

按照字母顺序重新排列字符串

```js
const sortCharactersInString = str =>
  str.split('').sort((a, b) => a.localeCompare(b)).join('');
// sortCharactersInString('cabbage') -> 'aabbceg'
```

[⬆ back to top](#目录)

### toCamelCase

把一个字符串转换为camelcase.

```js
const toCamelCase = str =>
  str.replace(/^([A-Z])|[\s-_]+(\w)/g, (match, p1, p2, offset) =>  p2 ? p2.toUpperCase() : p1.toLowerCase());
// toCamelCase("some_database_field_name") -> 'someDatabaseFieldName'
// toCamelCase("Some label that needs to be camelized") -> 'someLabelThatNeedsToBeCamelized'
// toCamelCase("some-javascript-property") -> 'someJavascriptProperty'
// toCamelCase("some-mixed_string with spaces_underscores-and-hyphens") -> 'someMixedStringWithSpacesUnderscoresAndHyphens'
```

[⬆ back to top](#目录)

### truncateString

根据指定长度截取字符串

```js
const truncateString = (str, num) =>
  str.length > num ? str.slice(0, num > 3 ? num - 3 : num) + '...' : str;
// truncateString('boomerang', 7) -> 'boom...'
```

[⬆ back to top](#目录)
## Utility

### coalesce

返回第一个不为null/undefined的参数

```js
const coalesce = (...args) => args.find(_ => ![undefined, null].includes(_))
// coalesce(null,undefined,"",NaN, "Waldo") -> ""
```

[⬆ back to top](#目录)

### coalesceFactory

实现自定义coalesce函数

```js
const coalesceFactory = valid => (...args) => args.find(valid);
// const customCoalesce = coalesceFactory(_ => ![null, undefined, "", NaN].includes(_))
// customCoalesce(undefined, null, NaN, "", "Waldo") //-> "Waldo"
```

[⬆ back to top](#目录)

### extendHex

将3位数的颜色代码扩展为6位数的颜色代码

```js
const extendHex = shortHex =>
  '#' + shortHex.slice(shortHex.startsWith('#') ? 1 : 0).split('').map(x => x+x).join('')
// extendHex('#03f') -> '#0033ff'
// extendHex('05a') -> '#0055aa'
```

[⬆ back to top](#目录)

### getType

获取一个值的原生类型.

```js
const getType = v =>
  v === undefined ? 'undefined' : v === null ? 'null' : v.constructor.name.toLowerCase();
// getType(new Set([1,2,3])) -> "set"
```

[⬆ back to top](#目录)

### hexToRGB

将hex色值转换为RGB

```js
const hexToRgb = hex => {
  const extendHex = shortHex =>
    '#' + shortHex.slice(shortHex.startsWith('#') ? 1 : 0).split('').map(x => x+x).join('');
  const extendedHex = hex.slice(hex.startsWith('#') ? 1 : 0).length === 3 ? extendHex(hex) : hex;
  return `rgb(${parseInt(extendedHex.slice(1), 16) >> 16}, ${(parseInt(extendedHex.slice(1), 16) & 0x00ff00) >> 8}, ${parseInt(extendedHex.slice(1), 16) & 0x0000ff})`;
}
// hexToRgb('#27ae60') -> 'rgb(39, 174, 96)'
// hexToRgb('#acd') -> 'rgb(170, 204, 221)'
```

[⬆ back to top](#目录)

### isArray

检查给定对象是否是数组

```js
const isArray = val => !!val && Array.isArray(val);
// isArray(null) -> false
// isArray([1]) -> true
```

[⬆ back to top](#目录)

### isBoolean

检查给定对象是否是布尔值

```js
const isBoolean = val => typeof val === 'boolean';
// isBoolean(null) -> false
// isBoolean(false) -> true
```

[⬆ back to top](#目录)

### isFunction

检查给定对象是否是方法

```js
const isFunction = val => val && typeof val === 'function';
// isFunction('x') -> false
// isFunction(x => x) -> true
```

[⬆ back to top](#目录)

### isNumber

检查给定对象是否是数字

```js
const isNumber = val => typeof val === 'number';
// isNumber('1') -> false
// isNumber(1) -> true
```

[⬆ back to top](#目录)

### isString

检查给定对象是否是字符串

```js
const isString = val => typeof val === 'string';
// isString(10) -> false
// isString('10') -> true
```

[⬆ back to top](#目录)

### isSymbol

检查给定对象是否是symbol.

```js
const isSymbol = val => typeof val === 'symbol';
// isSymbol('x') -> false
// isSymbol(Symbol('x')) -> true
```

[⬆ back to top](#目录)

### RGBToHex

将RGB色值转换为Hex

```js
const RGBToHex = (r, g, b) => ((r << 16) + (g << 8) + b).toString(16).padStart(6, '0');
// RGBToHex(255, 165, 1) -> 'ffa501'
```

[⬆ back to top](#目录)

### timeTaken

计算函数执行时间

```js
const timeTaken = callback => {
  console.time('timeTaken');  const r = callback();
  console.timeEnd('timeTaken');  return r;
};
// timeTaken(() => Math.pow(2, 10)) -> 1024
// (logged): timeTaken: 0.02099609375ms
```

[⬆ back to top](#目录)

### toOrdinalSuffix

Adds an ordinal suffix to a number.

Use the modulo operator (`%`) to find values of single and tens digits.
Find which ordinal pattern digits match.
If digit is found in teens pattern, use teens ordinal.

```js
const toOrdinalSuffix = num => {
  const int = parseInt(num), digits = [(int % 10), (int % 100)],
    ordinals = ['st', 'nd', 'rd', 'th'], oPattern = [1, 2, 3, 4],
    tPattern = [11, 12, 13, 14, 15, 16, 17, 18, 19];
  return oPattern.includes(digits[0]) && !tPattern.includes(digits[1]) ? int + ordinals[digits[0] - 1] : int + ordinals[3];
};
// toOrdinalSuffix("123") -> "123rd"
```

[⬆ back to top](#目录)

### UUIDGenerator

生成UUID.

```js
const UUIDGenerator = () =>
  ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, c =>
    (c ^ crypto.getRandomValues(new Uint8Array(1))[0] & 15 >> c / 4).toString(16)
  );
// UUIDGenerator() -> '7982fcfe-5721-4632-bede-6000885be57d'
```

[⬆ back to top](#目录)

### validateEmail

验证是否为邮箱

```js
const validateEmail = str =>
  /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/.test(str);
// validateEmail(mymail@gmail.com) -> true
```

[⬆ back to top](#目录)

### validateNumber

验证是否为数字

```js
const validateNumber = n => !isNaN(parseFloat(n)) && isFinite(n) && Number(n) == n;
// validateNumber('10') -> true
```

[⬆ back to top](#目录)

## Credits

*Icons made by [Smashicons](https://www.flaticon.com/authors/smashicons) from [www.flaticon.com](https://www.flaticon.com/) is licensed by [CC 3.0 BY](http://creativecommons.org/licenses/by/3.0/).*


