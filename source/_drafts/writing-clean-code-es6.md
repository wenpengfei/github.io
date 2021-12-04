---
title: 写出整洁的 JavaScript 代码 ES6 版
tags:
---
好的代码不仅仅是可以跑起来的代码，更是可以被其他人轻松阅读、重用和重构的代码，因为代码除了实现功能外，大部分的时间都是要被你或是团队其他成员维护的。

<img src="/images/writing-clean-code-es6/1.png" style="width:80%" />

虽然本文主要专注于编写干净整洁的 [JavaScript ES6](https://262.ecma-international.org/6.0/) 代码，并且不与任何框架相关，但是下面将要提到的绝大多数示例也适用于其他语言，另外，下面的示例也主要是从 Robert C. Martin 的书 [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) 中所采纳的建议，也不意味着要严格遵守。

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

> 注意，布尔类型的变量名通常像是在回答问题，例如：

```
isActive
didSubscribe
hasLinkedAccount
```

## 避免添加不必要的上下文

在特定的对象或类中不需要加冗余的上下文

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
// 错误 ❌
function printAllFilesInDirectory(dir) {
  const directory = dir || "./";
  //   ...
}

// 正确 ✅
function printAllFilesInDirectory(dir = "./") {
  // ...
}
```

## 限制参数个数

这一点有争议。函数的参数应该`不多于2个`，意思是函数的参数为 0 个 1 个或者 2 个，如果需要第三个参数的话说明：
- 函数需要拆分
- 可以把相关参数聚合成对象传递

```javascript
// 错误 ❌
function sendPushNotification(title, message, image, isSilent, delayMs) {
  // ...
}

sendPushNotification("New Message", "...", "http://...", false, 1000);

// 正确 ✅
function sendPushNotification({ title, message, image, isSilent, delayMs }) {
  // ...
}

const notificationConfig = {
  title: "New Message",
  message: "...",
  image: "http://...",
  isSilent: false,
  delayMs: 1000,
};

sendPushNotification(notificationConfig);
```

## 不要在一个函数中做太多事情

原则上一个函数只做一件事，这一原则可以很好地帮助我们降低函数的体积和复杂度，也能更好的测试、调试和重构，一个函数的代码行数是判断这个函数是否做太多事情的一个指标，一般建议一个函数长度小于 20~30 行。

```javascript
// 错误 ❌
function pingUsers(users) {
  users.forEach((user) => {
    const userRecord = database.lookup(user);
    if (!userRecord.isActive()) {
      ping(user);
    }
  });
}

// 正确 ✅
function pingInactiveUsers(users) {
  users.filter(!isUserActive).forEach(ping);
}

function isUserActive(user) {
  const userRecord = database.lookup(user);
  return userRecord.isActive();
}
```

## 避免使用 flag 变量

flag 变量意味着函数可以被进一步简化

```javascript
// 错误 ❌
function createFile(name, isPublic) {
  if (isPublic) {
    fs.create(`./public/${name}`);
  } else {
    fs.create(name);
  }
}

// 正确 ✅
function createFile(name) {
  fs.create(name);
}

function createPublicFile(name) {
  createFile(`./public/${name}`);
}
```

## 不要重复自己（DRY）

重复的代码不是一个好的信号，你复制粘贴了多少次，下次这部分代码修改的时候你就要就要同时修改几个地方。

```javascript
// 错误 ❌
function renderCarsList(cars) {
  cars.forEach((car) => {
    const price = car.getPrice();
    const make = car.getMake();
    const brand = car.getBrand();
    const nbOfDoors = car.getNbOfDoors();

    render({ price, make, brand, nbOfDoors });
  });
}

function renderMotorcyclesList(motorcycles) {
  motorcycles.forEach((motorcycle) => {
    const price = motorcycle.getPrice();
    const make = motorcycle.getMake();
    const brand = motorcycle.getBrand();
    const seatHeight = motorcycle.getSeatHeight();

    render({ price, make, brand, seatHeight });
  });
}

// 正确 ✅
function renderVehiclesList(vehicles) {
  vehicles.forEach((vehicle) => {
    const price = vehicle.getPrice();
    const make = vehicle.getMake();
    const brand = vehicle.getBrand();

    const data = { price, make, brand };

    switch (vehicle.type) {
      case "car":
        data.nbOfDoors = vehicle.getNbOfDoors();
        break;
      case "motorcycle":
        data.seatHeight = vehicle.getSeatHeight();
        break;
    }

    render(data);
  });
}
```

## 避免副作用

在 JavaScript 中，首选的模式应该是函数式而不是命令式，换句话说，要保证函数的纯粹性，副作用可以修改共享的状态和资源，会导致代码的不稳定和难以测试，排查问题也会特别棘手，所有的副作用应该集中管理。如果需要修改全局状态可以定义一个统一的服务去修改。

```javascript
// 错误 ❌
let date = "21-8-2021";

function splitIntoDayMonthYear() {
  date = date.split("-");
}

splitIntoDayMonthYear();

// Another function could be expecting date as a string
console.log(date); // ['21', '8', '2021'];

// 正确 ✅
function splitIntoDayMonthYear(date) {
  return date.split("-");
}

const date = "21-8-2021";
const newDate = splitIntoDayMonthYear(date);

// Original vlaue is intact
console.log(date); // '21-8-2021';
console.log(newDate); // ['21', '8', '2021'];
```

另外，如果一个可变的对象作为一个函数的参数传递进去，返回这个参数的时候应该是这个参数的克隆对象而不是直接把这个对象修改后返回。

```javascript

// 错误 ❌
function enrollStudentInCourse(course, student) {
  course.push({ student, enrollmentDate: Date.now() });
}

// 正确 ✅
function enrollStudentInCourse(course, student) {
  return [...course, { student, enrollmentDate: Date.now() }];
}
```

# 并发

## 避免使用回调
[回调函数](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function) 太乱了，所以 ES6 给我们提供了 [Promise](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function) 允许我们使用链式的回调，当然 [Async/Await](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function) 提供了更简洁的方案，让我们写出更加线性的代码

```javascript
// 错误 ❌
getUser(function (err, user) {
  getProfile(user, function (err, profile) {
    getAccount(profile, function (err, account) {
      getReports(account, function (err, reports) {
        sendStatistics(reports, function (err) {
          console.error(err);
        });
      });
    });
  });
});

// 正确 ✅
getUser()
  .then(getProfile)
  .then(getAccount)
  .then(getReports)
  .then(sendStatistics)
  .catch((err) => console.error(err));

// 正确 ✅✅
async function sendUserStatistics() {
  try {
    const user = await getUser();
    const profile = await getProfile(user);
    const account = await getAccount(profile);
    const reports = await getReports(account);
    return sendStatistics(reports);
  } catch (e) {
    console.error(err);
  }
}
```

# 错误处理

## 处理抛出的异常和 rejected 的 promise

正确的处理异常可以使我们的代码更加的简装，也会更方便的排查问题。

```javascript
// 错误 ❌
try {
  // Possible erronous code
} catch (e) {
  console.log(e);
}

// 正确 ✅
try {
  // Possible erronous code
} catch (e) {
  // Follow the most applicable (or all):
  // 1- More suitable than console.log
  console.error(e);

  // 2- Notify user if applicable
  alertUserOfError(e);

  // 3- Report to server
  reportErrorToServer(e);

  // 4- Use a custom error handler
  throw new CustomError(e);
}
```

# 注释

## 只为复杂的逻辑添加注释

不要过度的添加注释，只需要为复杂的逻辑添加即可。

```javascript
// 错误 ❌
function generateHash(str) {
  // Hash variable
  let hash = 0;

  // Get the length of the string
  let length = str.length;

  // If the string is empty return
  if (!length) {
    return hash;
  }

  // Loop through every character in the string
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = str.charCodeAt(i);

    // Make the hash
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}

// 正确 ✅
function generateHash(str) {
  let hash = 0;
  let length = str.length;
  if (!length) {
    return hash;
  }

  for (let i = 0; i < length; i++) {
    const char = str.charCodeAt(i);
    hash = (hash << 5) - hash + char;
    hash = hash & hash; // Convert to 32bit integer
  }
  return hash;
}
```

## 版本控制

完全没有必要写代码的修改历史，版本管理（比如 git ）已经帮我们做了这些事情。

```javascript

// 错误 ❌
/**
 * 2021-7-21: Fixed corner case
 * 2021-7-15: Improved performance
 * 2021-7-10: Handled mutliple user types
 */
function generateCanonicalLink(user) {
  // const session = getUserSession(user)
  const session = user.getSession();
  // ...
}

// 正确 ✅
function generateCanonicalLink(user) {
  const session = user.getSession();
  // ...
}
```

本文简短的讨论了一些可以提高 [ES6](https://262.ecma-international.org/6.0/) 代码可读性的原则和方法，绝大多数原则可以应用到其他编程语言上，使用这些原则可能会比较花时间，但是长远来看它可以保证你代码的可读性可扩展性。
