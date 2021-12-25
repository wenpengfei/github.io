---
title: JavaScript 的各种简写和骚操作
date: 2021-12-24 21:35:08
tags:
---

![](/images/javascript-shorthand-techniques/1.png)

整理了一些 JavaScript 的算是小技巧的东西，不经常用的东西容易忘记，欢迎在留言区补充~

## 三元表达式在 if...else 中的赋值操作

```JavaScript
const x = 20
let answer

// 常规写法
if (x > 10) {
  answer = 'greater than 10'
} else {
  answer = 'less than 10'
}

//  简写
const answer = x > 10 ? "greater than 10" : "less than 10";
// 也可以嵌套
const answer = x > 10 ? "greater than 10" : x < 5 ? "less than 5" : "between 5 and 10";
```

## 科学计数法

```JavaScript
// 常规写法
for (let i = 0; i < 10000; i++) {}

// 简写
for (let i = 0; i < 1e4; i++) {}
1e0 === 1;
1e1 === 10;
1e2 === 100;
1e3 === 1000;
1e4 === 10000;
1e5 === 100000;
```

## 解构赋值简写

```JavaScript
// 常规写法
const observable = require('mobx/observable');
const action = require('mobx/action');
const runInAction = require('mobx/runInAction');

const store = this.props.store;
const form = this.props.form;
const loading = this.props.loading;
const errors = this.props.errors;
const entity = this.props.entity;

// 简写
import { observable, action, runInAction } from 'mobx';
const { store, form, loading, errors, entity } = this.props;
```

## 必填参数

```JavaScript
// 常规写法
function foo(bar) {
  if (bar === undefined) {
    throw new Error('Missing parameter!')
  }
  return bar
}

// 简写
mandatory = () => {
  throw new Error('Missing parameter!')
}

foo = (bar = mandatory()) => {
  return bar
}
```

## 合并对象

```JavaScript
let fname = { firstName: 'Black' }
let lname = { lastName: 'Panther' }


// 常规写法
let full_names = Object.assign(fname, lname)

// 展开运算符写法
let full_names = { ...fname, ...lname }
```

## 检查 null、undefined 和空值

```JavaScript
// 常规写法
if (variable1 !== null || variable1 !== undefined || variable1 !== '') {
  let variable2 = variable1
}
// 简写
let variable2 = variable1  || '';
```

## Array.find 多个条件

```JavaScript
const pets = [
  { type: 'Dog', name: 'Max' },
  { type: 'Cat', name: 'Karl' },
  { type: 'Dog', name: 'Tommy' }
]

// 常规写法
function findDog(name) {
  for (let i = 0; i < pets.length; ++i) {
    if (pets[i].type === 'Dog' && pets[i].name === name) {
      return pets[i]
    }
  }
}

// 简写
pet = pets.find((pet) => pet.type === 'Dog' && pet.name === 'Tommy')
console.log(pet) // { type: 'Dog', name: 'Tommy' }
```

## Object[key]

我们都知道 `Foo.bar` 可以写成 `Foo['bar']` 的形式，利用好这两种写法可以写出易于阅读的代码

```JavaScript
// 一般情况
function validate(values) {
  if (!values.first) return false
  if (!values.last) return false
  return true
}

console.log(validate({ first: 'Bruce', last: 'Wayne' })) // true

// 复杂情况
const schema = {
  first: {
    required: true
  },
  last: {
    required: true
  }
}

// universal validation function
const validate = (schema, values) => {
  for (field in schema) {
    if (schema[field].required) {
      if (!values[field]) {
        return false
      }
    }
  }
  return true
}

console.log(validate(schema, { first: 'Bruce' })) // false
console.log(validate(schema, { first: 'Bruce', last: 'Wayne' })) // true
```

##  


## 声明多个变量

```JavaScript
// 常规写法
let x
let y = 20

// 简写
let x,
  y = 20
```

## 多个变量赋值

可以使用数组解构来为多个变量赋值

```JavaScript
// 常规写法
let a, b, c
a = 5
b = 8
c = 12

// 简写
let [a, b, c] = [5, 8, 12]
```

## 三元运算符

三元运算符通常可以少些多行代码

```JavaScript
// 常规写法
let marks = 26
let result
if (marks >= 30) {
  result = 'Pass'
} else {
  result = 'Fail'
}
// 简写
let result = marks >= 30 ? 'Pass' : 'Fail'
```


## 设置默认值

我们可以使用 `||` 短路语法来设置默认值，防止抛异常

```JavaScript
// 常规写法
let imagePath
let path = getImagePath()
if (path !== null && path !== undefined && path !== '') {
  imagePath = path
} else {
  imagePath = 'default.jpg'
}

// 简写
let imagePath = getImagePath() || 'default.jpg'
```

## AND（&&）短路语法

当你需要判断一个表达式为 `True` 才进行下一步时可以使用 `&&` 短路语法

```JavaScript
// 常规写法
if (isLoggedIn) {
  goToHomepage()
}

// 简写
isLoggedIn && goToHomepage()
```

`&&` 短路语法在 `React` 条件渲染中用的很多，比如：

```jsx
<div> { this.state.isLoading && <Loading /> } </div>
```

## 交换两个变量的值

交换两个变量值的时候我们一般会引入第三个临时变量，但是现在我们可以用数组的解构去实现

```JavaScript
let x = 'Hello',
  y = 55
// 常规写法
const temp = x
x = y
y = temp

// 简写
const [x, y] = [y, x]
```

## 箭头函数

这个不用多说了吧

```JavaScript
// 常规写法
function add(num1, num2) {
  return num1 + num2
}

// 简写
const add = (num1, num2) => num1 + num2
```

## 模板字符串

这个也不用多说，ES6 的常规操作

```JavaScript
// 常规写法
console.log('You got a missed call from ' + number + ' at ' + time)

// 简写
console.log(`You got a missed call from ${number} at ${time}`)

```

## 多行字符串

同上

```JavaScript
// 常规写法
console.log(
  'JavaScript, often abbreviated as JS, is a\n' +
    'programming language that conforms to the \n' +
    'ECMAScript specification. JavaScript is high-level,\n' +
    'often just-in-time compiled, and multi-paradigm.'
)

// 简写
console.log(
  `JavaScript, often abbreviated as JS, 
  is a programming language that conforms to the ECMAScript specification. 
  JavaScript is high-level, often just-in-time compiled, and multi-paradigm.`
)
```

## 多个条件判断

当有多个值需要判断时，可以考虑使用 `indexOf()` 和 `includes()` 方法

```JavaScript
// 常规写法
if (value === 1 || value === 'one' || value === 2 || value === 'two') {
  // 其他代码...
}

// 简写 1
if ([1, 'one', 2, 'two'].indexOf(value) >= 0) {
  // 其他代码...
}
// 简写 2
if ([1, 'one', 2, 'two'].includes(value)) {
  // 其他代码...
}
```

## 对象属性赋值

如果变量名和对象的属性名相同，就不用写`键：值`的形式，直接写变量名就行

```JavaScript
let firstName = 'Amitav'
let lastName = 'Mishra'

// 常规写法
let obj = { firstName: firstName, lastName: lastName }

// 简写
let obj = { firstName, lastName }
```

## 字符串转数字

虽然 JavaScript 内置了 `parseInt` 和 `parseFloat` 方法，但是我们也可以使用 `+` 操作符

```JavaScript
// 常规写法 
let total = parseInt('453'); 
let average = parseFloat('42.6'); 

// 简写 
let total = +'453'; 
let average = +'42.6';
```

## 重复字符串

要把字符串重复指定的次数，可以使用 `for` 循环，现在可以使用 `repeat()` 方法一行搞定

```JavaScript
// 常规写法
let str = ''
for (let i = 0; i < 5; i++) {
  str += 'Hello '
}
console.log(str) // Hello Hello Hello Hello Hello

// 简写
'Hello '.repeat(5)
```

> 惹恼了女朋友需要说一百遍 sorry ？每次还得换行？结尾加个 `\n` 换行符就行

```JavaScript
'sorry\n'.repeat(100);
```

## 指数运算

一般我们使用 `Math.pow()` 进行指数运算，现在我们可以使用双星号 `(**)`

```JavaScript
// 常规写法
const power = Math.pow(4, 3) // 64

// 简写
const power = 4 ** 3 // 64
```

## 双非位运算符（~~）

双非位运算符是 `Math.floor()` 的替代品

```JavaScript
// 常规写法
const floor = Math.floor(6.8) // 6

// 简写
const floor = ~~6.8 // 6
```

## 找到数组中的最大/最小值

```JavaScript
// 简写
const arr = [2, 8, 15, 4]
Math.max(...arr) // 15
Math.min(...arr) // 2
```

## for 循环

我们一般都用 `for` 循环去遍历一个数组，现在我们可以用 `for...of` 遍历值，使用 `for...in` 遍历索引

```JavaScript
const array1 = ['a', 'b', 'c'];

for (const element of array1) {
  console.log(element);
}

// expected output: "a"
// expected output: "b"
// expected output: "c"
```

```JavaScript
const obj = {a:1, b:2, c:3};

for (var prop in obj) {
  console.log("obj." + prop + " = " + obj[prop]);
}

// Output:
// "obj.a = 1"
// "obj.b = 2"
// "obj.c = 3"
```

## 合并数组

```JavaScript
let arr1 = [20, 30]

// 常规写法
let arr2 = arr1.concat([60, 80])
// [20, 30, 60, 80]

// 简写
let arr2 = [...arr1, 60, 80]
// [20, 30, 60, 80]

```

## 深拷贝

```JavaScript
let obj = { x: 20, y: { z: 30 } }

// 常规写法
const makeDeepClone = (obj) => {
  let newObject = {}
  Object.keys(obj).map((key) => {
    if (typeof obj[key] === 'object') {
      newObject[key] = makeDeepClone(obj[key])
    } else {
      newObject[key] = obj[key]
    }
  })
  return newObject
}
const cloneObj = makeDeepClone(obj)

// 属性值不含 function, undefined 或者 NaN 时
const cloneObj = JSON.parse(JSON.stringify(obj))

// 也可以用展开运算符
let obj = { x: 20, y: 'hello' }
const cloneObj = { ...obj }
```

## 获取字符

```JavaScript
let str = 'jscurious.com'
// 常规写法
str.charAt(2) // c

// 简写
str[2] // c
```

# 参考

[[1] 20 JavaScript Shorthand Techniques that will save your time](https://JavaScript.plainenglish.io/20-JavaScript-shorthand-techniques-that-will-save-your-time-f1671aab405f)
[[2] 12 Good JavaScript Shorthand Techniques](https://medium.com/hackernoon/12-amazing-JavaScript-shorthand-techniques-fef16cdbc7fe)
[[3] 25+ JavaScript Shorthand Coding Techniques](https://www.sitepoint.com/shorthand-JavaScript-techniques/)
