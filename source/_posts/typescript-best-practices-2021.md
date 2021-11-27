---
title: Typescript 的最佳实践 2021 版
date: 2021-11-27 20:35:42
tags:
---

现在我们越来越多的项目都用上了 Typescript，也享受到了它带来的好处，为了更高效的使用它，我们可以遵循一些 `最佳实践`  ，以下的原则是我在使用并且推荐的

## 使用正确的类型声明（避免使用 any ）

类型声明是 Typescript 的一大优势，尤其是体现在代码编写阶段，因为 JavaScript 是在运行时定义类型的，Typescript 可以帮助你在运行之前就过滤掉一大部分类型引发的奇怪问题，当你知道你定义的变量是什么类型的时候不要使用 `any`，建议每次定义新变量的时候后都加上数据类型。

```typescript
name: string = "hello";
value: number = 50;
isCorrect: boolean = false;
```

## 使用严格模式

`use strict` 是 JavaScript ES5 添加的功能，它就是字面的意思：`使用严格模式`，可以在 `tsconfig` 文件中找到相关的配置。

![Untitled](/images/typescript-best-practices-2021/1.png)

这样可以防止你犯无意识或低级的错误，例如使用未声明的变量、不使用类型注释或尝试使用未来的保留关键字作为变量名等。`use strict` 通过`语法错误`的形式帮助您编写良好且安全的代码习惯。

## 使用 let 代替 var

*var* 是一位很好的老朋友，但是 *let* 和 *const* 在 *ES6* 中出现了，他们的出现是为了解决 *var* 的一些问题。

*var* 既可以作用于`全局作用域`又可以作用于`局部作用域`。

- 当 *var* 类型变量在函数/块之外定义时，它就成为全局范围的变量，该变量可用于脚本内的任何地方
- 当 *var* 在函数内部定义时变为局部作用域，它只能在该函数内部访问

```typescript
var name= "John Doe"; // 全局作用域
function getAge() {
  var age= 30; // 局部作用域
}
```

*var* 关键字有几个缺陷：可以重复声明，不声明也可以被调用，TS 也不会报错，会导致一些奇怪的问题。

```typescript
var name = "John Doe";
function getName(){
    var name = "Anne"; // 不会报错
}
```

为避免这种情况，应该改用 *let* 。 *let* 声明的是一个块级作用域变量，并且不能重新声明。但是你可以在不同的作用域中声明相同的变量名，每一个都被视为不同变量。

```typescript
let name = "John";
if (true) {
  let name = "Anne";
  console.log(name); // "Anne"
}
console.log(name); // "John"
```

## 常量使用 **const 声明**

*const* 和 *let 是一起新增的变量声明*。 *const* 也是块级作用域类型，同样的，也不能被重新声明。这些是 *let* 和 *const* 之间的相似之处。不同之处是 *const* 不能被重新赋值。所以当你声明一个常量时使用 *const*。

```typescript
const name = "John";
name = "Anne"; // error
const age = 30;
const age = 31; //error
```

> PS：声明一个 *const* 对象时，不能重新对它赋值，但是可以修改它的的属性

## 固定长度的数组使用元组类型

```typescript
let marks: number[] = [1, 2, 3];
```

你可以对 `marks` 数组添加任意数量的元素，只要都是 `number` 类型，TS 不会限制你。

但是，在数组长度为常量的情况下可能会导致严重的逻辑错误。为了避免这种错误，你可以使用元组类型来限制数组的长度和每一项的数据类型

```typescript
let marks:[number, number] = [1, 2]; // 含有 2 个 number 类型元素的数组类型
marks = [10, 20]; // 成功
marks = [1]; // 语法错误
marks = [1, 2, 3, 4, 5] // 语法错误
```

## 使用类型别名

假设有多个变量或对象拥有相同的数据结构类型

```typescript
let man: {name: string, age: number} = {name = "john", age=30};
let woman: {name: string, age: number} = {name = "Anne", age=32};
```

为了避免这种冗余的类型定义你可以使用`类型别名`

```typescript
type Details = {name: string, age: number}; // 定义类型别名
let man: Details = {name = "john", age=30}; // 使用类型别名
let woman: Details = {name = "Anne", age=32};
```

带来的额外好处就是代码的可读性更强，看起来更清晰

## any 和 unknown

表面上看来 *any*  和 *unknow* 没有什么区别，都是在我们不能确定数据类型的时候所使用的帮助类型，如果我们想快速的把 js 重构为 ts，两者均可，但是也有一些区别

任何值都可以标记为 *any*  或者 *unknown*

```typescript
let anyExample: any; // 定义一个 any 类型
let unknownExample: unknown; // 定义一个 unknown 类型
anyExample = 123; 
anyExample = "Hey"
unknownExample = false;
unknownExample = 23.22;
```

对于标记 any 的值，你可以对它做任何事

```typescript
anyExample.you.made.this.code.chain(); // success
```

unknow 则不行，它是一种更安全的类型

```typescript
unknownExample.trim(); // 语法错误
```

如果要使用 *unknow* 类型，你得把他放在一个条件判断语句中

```typescript
if (typeof exampleUnkown == "string") { // 第一步，检查类型
  exampleUnkown.trim(); // 不会报错
}
```

## 对类成员使用访问修饰符

TS 为 *class* 成员提供了`访问修饰符`，可以设置 *public*、*protected* 或者 *private* 属性，但 *class* 永远是 `public` 类型

- private：仅可以在内部访问
- protected：内部或者子类可以访问
- public：都可以访问

```typescript
class Employee {
  protected name: string;
  private salary: number;
  
  constructor(name: string, salary: number) {
    this.name = name;
    this.salary = salary
  }

  public getSalary(){
    return salary
  }
}
```

如果要访问 `salary` 属性，你必须调用 `getSalary` 方法

```typescript
class Developer extends Employee{
  viewDetails(){
    console.log(this.salary); // 错误: 属性 'salary' 是私有属性
    console.log(this.getSalary()); // success
  }
}
```

通过子类访问 `name` 属性

```typescript
class Developer extends Employee{
  viewDetails(){
    console.log(this.name);
  }
}
```

## 使用 Lint 工具

每个人都有自己的开发风格和习惯，在团队项目中，多种代码风格是灾难性的，如果不想污染代码库，还是需要选择一个 Lint 工具，首选 `ESLint`，它与 JavaScript 和 Typescript 都兼容

## 格式化代码

使用好的代码格式化程序可以使您的编码更高效、更简洁。根据我的个人经验，我更喜欢在 VS 代码中使用 `Prettier`。但是有很多代码格式化程序，选择取决于你使用的编辑器