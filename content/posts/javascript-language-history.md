---
title: "JavaScript 发展史：从 10 天脚本到全栈霸主"
date: 2024-05-03T12:30:00+08:00
draft: false
tags: ["JavaScript", "编程语言", "发展史", "前端开发", "Node.js"]
categories: ["技术文章"]
author: "Admin"
description: "全面回顾 JavaScript 从 1995 年诞生至今的传奇历程，探索它如何从被嘲笑的玩具语言成长为统治 Web 和全栈开发的霸主。"
---

# JavaScript 发展史：从 10 天脚本到全栈霸主

> "JavaScript 是世界上最被误解的编程语言。" — Douglas Crockford

## 引言

JavaScript 可能是编程史上最成功的"意外"。1995 年，网景公司（Netscape）的布兰登·艾克（Brendan Eich）仅用 **10 天时间**就设计出了这门语言的雏形。当时它只是一个用于网页表单验证的小脚本工具，甚至被许多专业开发者嘲笑为"玩具语言"。

然而，28 年后的今天，JavaScript 已经：

- 🌐 成为 **Web 前端的事实标准**（98% 的网站使用）
- 🚀 通过 Node.js 进军 **后端开发**
- 📱 通过 React Native、Electron 等框架实现 **跨平台移动和桌面应用开发**
- 🤖 在 **物联网、机器学习、区块链** 等领域占有一席之地
- 📦 拥有全球最大的包管理器 **npm**（超过 200 万个包）

本文将带您穿越时空，全面了解 JavaScript 从诞生到统治的完整历程。

---

## 第一章：诞生前夕（1993-1995）

### 互联网黎明时代

1993 年，万维网（World Wide Web）刚刚起步。当时的网页非常静态：
- 只有简单的 HTML 标签
- 没有交互性
- 每次用户操作都需要重新加载整个页面
- 表单验证必须提交到服务器

网景公司的工程师们意识到：**需要一种轻量级的脚本语言，让网页具备基本的交互能力**。

### 关键人物登场

**布兰登·艾克（Brendan Eich）**
- 1995 年加入网景公司
- 之前在微系统公司（Microsystem Solutions）工作
- 精通 Scheme、Self 等函数式和原型语言
- 原本计划将 Scheme 嵌入网景浏览器

**马克·安德森（Marc Andreessen）**
- 网景公司联合创始人
- 提出了"让非专业程序员也能编写简单脚本"的愿景
- 要求新语言看起来像 Java（当时最火的语言）

### 设计目标

网景公司为这门新语言设定了明确的目标：

1. **轻量级**：语法简单，学习曲线低
2. **嵌入式**：可以直接写在 HTML 中
3. **动态类型**：不需要声明变量类型
4. **函数式**：支持一等公民函数
5. **原型继承**：而非传统的类继承
6. **与 Java 协作**：可以调用 Java applet

---

## 第二章：创世纪的 10 天（1995 年 5 月）

### 快速开发历程

1995 年 5 月，布兰登·艾克接到任务：**为网景 Navigator 浏览器设计一门脚本语言**。

**时间线：**
- **第 1-2 天**：设计基本语法结构
- **第 3-5 天**：实现词法分析器和解析器
- **第 6-8 天**：构建运行时环境和内置对象
- **第 9-10 天**：测试和调试

### 命名风波

这门语言的命名经历了一系列变化：

1. **Mocha**（摩卡）：艾克最初的命名，灵感来自咖啡
2. **LiveScript**：网景市场团队改名，强调"动态"特性
3. **JavaScript**：与 Sun 公司合作后最终定名（蹭 Java 热度）

> 💡 **有趣的事实**：JavaScript 和 Java 的关系，就像"汽车"和"地毯"的关系——除了名字相似，几乎没有共同点。

### 首个版本：Mocha v1.0

1995 年 9 月，网景 Navigator 2.0 Beta 发布，首次内置了 JavaScript（当时叫 LiveScript）。

**核心特性：**
```javascript
// 变量声明（当时还没有 let/const）
var name = "World";

// 基本数据类型
var number = 42;
var text = "Hello";
var flag = true;

// 函数定义
function greet(person) {
    return "Hello, " + person + "!";
}

// 简单的 DOM 操作（当时还不叫 DOM）
document.write(greet(name));

// 原型对象
var person = {
    name: "Alice",
    age: 25,
    sayHello: function() {
        return "Hi, I'm " + this.name;
    }
};
```

**初始内置对象：**
- `Object` - 基础对象
- `Function` - 函数构造器
- `Array` - 数组
- `String` - 字符串
- `Number` - 数字
- `Boolean` - 布尔值
- `Date` - 日期时间
- `Math` - 数学函数

---

## 第三章：浏览器大战与标准化（1996-1999）

### 微软入局：JScript 诞生

1996 年，微软推出 Internet Explorer 3.0，为了对抗网景，微软：

- **逆向工程**了 JavaScript
- 改名为 **JScript**（避免商标问题）
- 添加了一些专有扩展
- 实现了部分不同的行为

**后果：** 开发者不得不编写两套代码，或者检测浏览器类型。

### 其他竞争者

- **VBScript**：微软基于 Visual Basic 的脚本语言（主要在 IE 中使用）
- **PerlScript**：ActiveState 推出的 Perl 版本
- **PythonScript**：短暂的 Python 浏览器脚本尝试

### ECMAScript 标准化

1996 年 11 月，网景将 JavaScript 提交给 **欧洲计算机制造商协会（Ecma International）** 进行标准化。

**标准化过程：**
- **TC39 委员会**成立（Technical Committee 39）
- 成员包括：网景、微软、太阳微系统、诺基亚等
- 目标：创建中立的语言规范，避免厂商锁定

#### ECMAScript 1.0（1997 年 6 月）

第一个官方标准发布，主要特性：

```javascript
// 正则表达式支持
var pattern = /hello/i;
var result = pattern.test("Hello World");

// try-catch 异常处理
try {
    riskyOperation();
} catch (e) {
    console.log("Error: " + e.message);
}

// switch 语句
switch (day) {
    case "Monday":
        console.log("Start of week");
        break;
    default:
        console.log("Other day");
}

// do-while 循环
var i = 0;
do {
    console.log(i);
    i++;
} while (i < 5);
```

#### ECMAScript 2.0（1998 年 6 月）

小幅更新，主要是编辑性修改，与 ISO/IEC 16262 国际标准对齐。

#### ECMAScript 3.0（1999 年 12 月）

**重大更新**，引入了许多现代特性：

```javascript
// 正则表达式改进
var email = /\S+@\S+\.\S+/;

// 新的字符串方法
var text = "hello world";
text.toUpperCase();      // "HELLO WORLD"
text.substring(0, 5);    // "hello"
text.split(" ");         // ["hello", "world"]

// 数组新方法
var nums = [1, 2, 3];
nums.join(",");          // "1,2,3"
nums.slice(1, 2);        // [2]
nums.concat([4, 5]);     // [1, 2, 3, 4, 5]

// try-catch-finally
try {
    doSomething();
} catch (e) {
    handleError(e);
} finally {
    cleanup();
}

// 保留字增加（enum, instanceof, typeof 等）
```

**影响：**
- IE 5.5+、Firefox、Opera 9+ 都实现了 ES3
- 成为事实上的标准长达 **10 年**
- 许多老代码至今仍基于 ES3

---

## 第四章：黑暗时代（2000-2007）

### AJAX 革命（2005）

2005 年，Google 推出了 Google Maps 和 Gmail，展示了 **AJAX（Asynchronous JavaScript and XML）** 的强大能力。

**核心概念：**
```javascript
// 原始的 XMLHttpRequest
function loadData() {
    var xhr = new XMLHttpRequest();
    xhr.open("GET", "/api/data", true);
    xhr.onreadystatechange = function() {
        if (xhr.readyState === 4 && xhr.status === 200) {
            var data = JSON.parse(xhr.responseText);
            updatePage(data);
        }
    };
    xhr.send();
}
```

**影响：**
- 网页可以异步加载数据，无需刷新
- 用户体验大幅提升
- 催生了 Web 2.0 时代
- JavaScript 从"玩具"变成"必需"

### jQuery 诞生（2006）

约翰·雷西格（John Resig）发布了 **jQuery**，解决了浏览器兼容性问题。

```javascript
// 原生 JavaScript（2006 年）
var items = document.getElementsByTagName("li");
for (var i = 0; i < items.length; i++) {
    items[i].addEventListener("click", function() {
        this.style.color = "red";
    });
}

// jQuery（简洁优雅）
$("li").click(function() {
    $(this).css("color", "red");
});
```

**jQuery 的贡献：**
- 统一的跨浏览器 API
- CSS 选择器引擎
- 链式调用
- 简化 AJAX
- 插件生态系统

> 💡 **巅峰时期**：2011 年，前 10000 个网站中 **75%** 使用 jQuery。

### 框架萌芽期

- **Prototype**（2005）：最早的前端框架之一
- **MooTools**（2006）：注重面向对象
- **Dojo Toolkit**（2005）：企业级组件库
- **YUI**（2006）：雅虎出品

---

## 第五章：标准化的停滞与复兴（2008-2014）

### ECMAScript 4 的失败

2000 年代初，TC39 开始规划 ES4，提议包括：
- 类（class）
- 模块（module）
- 类型注解（type annotations）
- 私有成员
- 命名空间

**分歧：**
- **激进派**（雅虎、谷歌）：想要大规模革新
- **保守派**（微软）：担心破坏向后兼容性

**结果：** 2008 年，ES4 被正式放弃，团队分裂。

### ECMAScript 3.1 → ECMAScript 5（2009）

作为妥协，TC39 发布了 ES5，专注于"安全网"和改进现有功能。

**关键新特性：**

```javascript
// 严格模式
"use strict";
x = 3.14; // 报错：未声明变量

// JSON 原生支持
var obj = JSON.parse('{"name": "Alice"}');
var str = JSON.stringify(obj);

// 数组新方法
[1, 2, 3].forEach(function(n) {
    console.log(n);
});

[1, 2, 3].map(function(n) {
    return n * 2;
}); // [2, 4, 6]

[1, 2, 3, 4].filter(function(n) {
    return n > 2;
}); // [3, 4]

[1, 2, 3].reduce(function(sum, n) {
    return sum + n;
}, 0); // 6

// Object 新方法
Object.keys({a: 1, b: 2}); // ["a", "b"]
Object.create(proto);
Object.defineProperty(obj, "prop", {value: 42});

// Getter/Setter
var obj = {
    get name() {
        return this._name;
    },
    set name(val) {
        this._name = val;
    }
};

// Function 绑定
var boundFunc = func.bind(thisArg);
```

**浏览器支持时间表：**
- IE 9（2011）：首个支持 ES5 的 IE 版本
- Firefox 4（2011）
- Chrome 13（2011）
- Safari 5.1（2011）

### Node.js 革命（2009）

瑞安·达尔（Ryan Dahl）创造了 **Node.js**，让 JavaScript 进入后端世界。

**核心理念：**
- 事件驱动
- 非阻塞 I/O
- 单线程高并发
- npm 包管理器

```javascript
// 简单的 HTTP 服务器
const http = require("http");

http.createServer((req, res) => {
    res.writeHead(200, {"Content-Type": "text/plain"});
    res.end("Hello World\n");
}).listen(3000);

console.log("Server running at http://localhost:3000/");
```

**影响：**
- 前后端都用 JavaScript（全栈开发）
- 催生了庞大的 npm 生态系统
- 改变了后端开发格局

### 模块化战争

在浏览器端，出现了多种模块规范：

**CommonJS**（Node.js 采用）：
```javascript
// math.js
module.exports = {
    add: function(a, b) {
        return a + b;
    }
};

// main.js
var math = require("./math");
console.log(math.add(2, 3));
```

**AMD**（RequireJS）：
```javascript
define(["./math"], function(math) {
    console.log(math.add(2, 3));
});
```

**UMD**（通用模块定义）：
```javascript
(function (root, factory) {
    if (typeof define === "function" && define.amd) {
        define(["exports"], factory);
    } else if (typeof exports === "object") {
        factory(exports);
    } else {
        factory((root.MyModule = {}));
    }
}(this, function (exports) {
    exports.add = function(a, b) { return a + b; };
}));
```

---

## 第六章：现代 JavaScript 时代（2015-至今）

### ECMAScript 2015（ES6）— 里程碑

2015 年 6 月，**ES6** 发布，这是自 1999 年以来最大的更新。

**年度发布周期：**
- 从 ES6 开始，改为每年发布一个版本
- 版本号改为年份：ES2015, ES2016, ES2017...

#### ES6 核心特性

**1. let 和 const**
```javascript
// 块级作用域
if (true) {
    let x = 10;
    const PI = 3.14;
    console.log(x); // 10
}
console.log(x); // ReferenceError

// const 不可重新赋值
const arr = [1, 2, 3];
arr.push(4); // ✅ 可以修改内容
arr = [5, 6]; // ❌ 不能重新赋值
```

**2. 箭头函数**
```javascript
// 传统函数
var numbers = [1, 2, 3];
var doubled = numbers.map(function(n) {
    return n * 2;
});

// 箭头函数
const doubled = numbers.map(n => n * 2);

// 多参数
const add = (a, b) => a + b;

// 无参数
const sayHi = () => console.log("Hi");

// 返回对象
const createPerson = (name, age) => ({name, age});

// this 绑定（不绑定自己的 this）
class Person {
    constructor(name) {
        this.name = name;
    }
    greet() {
        setTimeout(() => {
            console.log(`Hello, ${this.name}`);
        }, 1000);
    }
}
```

**3. 类和继承**
```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        console.log(`${this.name} makes a sound`);
    }
    
    static info() {
        return "Animals are living beings";
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name);
        this.breed = breed;
    }
    
    speak() {
        console.log(`${this.name} barks`);
    }
}

const dog = new Dog("Buddy", "Golden Retriever");
dog.speak(); // "Buddy barks"
console.log(Animal.info()); // "Animals are living beings"
```

**4. 模板字符串**
```javascript
const name = "Alice";
const age = 25;

// 多行字符串
const message = `Hello, ${name}!
You are ${age} years old.
Next year you will be ${age + 1}.`;

// 标签模板
function uppercase(strings, ...values) {
    return strings.reduce((result, str, i) => {
        return result + str + (values[i] || "").toUpperCase();
    }, "");
}
uppercase`Hello ${name}!`; // "Hello ALICE!"
```

**5. 解构赋值**
```javascript
// 数组解构
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first); // 1
console.log(rest); // [3, 4, 5]

// 对象解构
const {name, age, city = "Unknown"} = {name: "Bob", age: 30};
console.log(city); // "Unknown"

// 函数参数解构
function printUser({name, age}) {
    console.log(`${name} is ${age}`);
}

// 交换变量
let a = 1, b = 2;
[a, b] = [b, a];
```

**6. 默认参数**
```javascript
function greet(name = "Guest", greeting = "Hello") {
    return `${greeting}, ${name}!`;
}

greet(); // "Hello, Guest!"
greet("Alice"); // "Hello, Alice!"
greet("Bob", "Hi"); // "Hi, Bob!"
```

**7. 展开运算符**
```javascript
// 数组展开
const arr1 = [1, 2, 3];
const arr2 = [...arr1, 4, 5]; // [1, 2, 3, 4, 5]

// 对象展开
const obj1 = {a: 1, b: 2};
const obj2 = {...obj1, c: 3}; // {a: 1, b: 2, c: 3}

// 函数参数
function sum(...numbers) {
    return numbers.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3, 4); // 10
```

**8. Promise**
```javascript
// 回调地狱
getData(function(a) {
    getMoreData(a, function(b) {
        getEvenMoreData(b, function(c) {
            console.log(c);
        });
    });
});

// Promise 链
getData()
    .then(a => getMoreData(a))
    .then(b => getEvenMoreData(b))
    .then(c => console.log(c))
    .catch(err => console.error(err));

// Promise.all
Promise.all([promise1, promise2, promise3])
    .then(results => {
        console.log(results);
    });

// Promise.race
Promise.race([fastPromise, slowPromise])
    .then(result => {
        console.log("First to finish:", result);
    });
```

**9. 模块系统**
```javascript
// export.js
export const PI = 3.14159;
export function add(a, b) {
    return a + b;
}
export default class Calculator {}

// import.js
import Calculator, {PI, add as plus} from "./export.js";
console.log(PI);
console.log(plus(2, 3));

// 全部导入
import * as MathUtils from "./export.js";
```

**10. 迭代器和生成器**
```javascript
// 生成器函数
function* fibonacci() {
    let [a, b] = [0, 1];
    while (true) {
        yield a;
        [a, b] = [b, a + b];
    }
}

const fib = fibonacci();
console.log(fib.next().value); // 0
console.log(fib.next().value); // 1
console.log(fib.next().value); // 1
console.log(fib.next().value); // 2

// for...of 循环
for (const num of [1, 2, 3]) {
    console.log(num);
}
```

**11. Map 和 Set**
```javascript
// Map
const map = new Map();
map.set("name", "Alice");
map.set("age", 25);
console.log(map.get("name")); // "Alice"
console.log(map.size); // 2

// 任意类型作为键
const obj = {};
map.set(obj, "value");

// Set
const set = new Set([1, 2, 2, 3, 3, 3]);
console.log(set); // Set{1, 2, 3}
set.add(4);
set.delete(2);
```

**12. Symbol**
```javascript
const id = Symbol("id");
const user = {
    [id]: 123,
    name: "Alice"
};
console.log(user[id]); // 123
console.log(Object.keys(user)); // ["name"] (Symbol 不可枚举)
```

### ES2016（ES7）

**新特性：**

```javascript
// Array.prototype.includes
[1, 2, 3].includes(2); // true
[1, 2, 3].includes(4); // false

// 指数运算符
2 ** 3; // 8
4 ** 0.5; // 2
```

### ES2017（ES8）

**async/await — 异步编程的革命**

```javascript
// Promise 链
fetch("/api/user")
    .then(response => response.json())
    .then(user => fetch(`/api/posts/${user.id}`))
    .then(posts => console.log(posts))
    .catch(error => console.error(error));

// async/await
async function getUserPosts() {
    try {
        const response = await fetch("/api/user");
        const user = await response.json();
        const postsResponse = await fetch(`/api/posts/${user.id}`);
        const posts = await postsResponse.json();
        console.log(posts);
    } catch (error) {
        console.error(error);
    }
}

// 并行执行
async function getAllData() {
    const [users, posts, comments] = await Promise.all([
        fetch("/api/users").then(r => r.json()),
        fetch("/api/posts").then(r => r.json()),
        fetch("/api/comments").then(r => r.json())
    ]);
    return {users, posts, comments};
}
```

**其他特性：**
```javascript
// Object.values / Object.entries
const obj = {a: 1, b: 2, c: 3};
Object.values(obj); // [1, 2, 3]
Object.entries(obj); // [["a", 1], ["b", 2], ["c", 3]]

// String padding
"hello".padStart(10, "*"); // "*****hello"
"hello".padEnd(10, "*");   // "hello*****"

// Object.getOwnPropertyDescriptors
// Trailing commas in function parameters
function foo(a, b, c,) {}
```

### ES2018（ES9）

```javascript
// 异步迭代
async function* asyncGenerator() {
    yield await Promise.resolve(1);
    yield await Promise.resolve(2);
}

// for await...of
(async () => {
    for await (const num of asyncGenerator()) {
        console.log(num);
    }
})();

// 对象 rest/spread
const {a, ...rest} = {a: 1, b: 2, c: 3};
console.log(rest); // {b: 2, c: 3}

const obj1 = {a: 1};
const obj2 = {...obj1, b: 2}; // {a: 1, b: 2}

// 正则表达式改进
// s 标志（dotAll）
/./s.test("\n"); // true

// 命名捕获组
const match = "2024-05-03".match(/(?<year>\d{4})-(?<month>\d{2})-(?<day>\d{2})/);
console.log(match.groups.year); // "2024"

// 后行断言
/(?<=\$)\d+/.test("$100"); // true
/\d+(?!px)/.test("100em"); // true
```

### ES2019（ES10）

```javascript
// Array.flat / flatMap
[1, [2, 3], [4, [5, 6]]].flat(); // [1, 2, 3, 4, [5, 6]]
[1, [2, 3], [4, [5, 6]]].flat(2); // [1, 2, 3, 4, 5, 6]

[1, 2, 3].flatMap(n => [n, n * 2]); // [1, 2, 2, 4, 3, 6]

// Object.fromEntries
Object.fromEntries([["a", 1], ["b", 2]]); // {a: 1, b: 2}

// String.trimStart / trimEnd
"  hello  ".trimStart(); // "hello  "
"  hello  ".trimEnd();   // "  hello"

// Optional catch binding
try {
    riskyOperation();
} catch {
    // 不需要 error 变量
}

// Symbol.description
const sym = Symbol("desc");
sym.description; // "desc"
```

### ES2020（ES11）

```javascript
// BigInt
const huge = 9007199254740991n;
const another = BigInt(9007199254740991);

// 可选链
const street = user?.address?.street;
const name = arr?.[0]?.name;
const value = obj?.method?.();

// 空值合并运算符
const name = userInput ?? "Default";
// 只有 null 或 undefined 时才使用默认值

// dynamic import
const module = await import("./module.js");

// Promise.allSettled
const results = await Promise.allSettled([
    promise1,
    promise2,
    promise3
]);
// 所有 Promise 都完成，不管成功失败

// globalThis
// 统一的全局对象引用（替代 window, global, self）

// matchAll
const matches = [..."test1test2".matchAll(/test(\d)/g)];
```

### ES2021（ES12）

```javascript
// String.replaceAll
"hello world".replaceAll("l", "L"); // "heLLo worLd"

// Promise.any
const first = await Promise.any([
    fetch("/api/1"),
    fetch("/api/2"),
    fetch("/api/3")
]);
// 只要有一个成功就返回

// 逻辑赋值运算符
a ||= b;  // a || (a = b)
a &&= b;  // a && (a = b)
a ??= b;  // a ?? (a = b)

// Numeric separators
const billion = 1_000_000_000;
const binary = 0b1010_0001_1100_0101;

// WeakRefs 和 FinalizationRegistry
const weakRef = new WeakRef(object);
```

### ES2022（ES13）

```javascript
// 类的字段
class MyClass {
    static staticField = "static";
    field = "instance";
    #privateField = "private";
    
    #privateMethod() {
        return "private";
    }
}

// 私有字段访问
class MyClass {
    #count = 0;
    
    increment() {
        this.#count++;
    }
    
    get count() {
        return this.#count;
    }
}

// top-level await
const data = await fetch("/api/data").then(r => r.json());
export default data;

// 对象增强
const key = "dynamic";
const obj = {
    [key]: "value",
    method() {},
    // 等同于
    // method: function() {}
};

// RegExp.matchIndices
const match = /test(\d)/g.exec("test1");
match.indices; // [[0, 5], [4, 5]]

// .at() 方法
const arr = [1, 2, 3, 4, 5];
arr.at(-1); // 5 (最后一个元素)
```

### ES2023（ES14）

```javascript
// Array.findLast / findLastIndex
const lastEven = [1, 2, 3, 4, 5].findLast(n => n % 2 === 0); // 4

// Array.toReversed / toSorted / toSpliced / with
const original = [1, 2, 3];
const reversed = original.toReversed(); // [3, 2, 1]
console.log(original); // [1, 2, 3] (不变)

const sorted = [3, 1, 2].toSorted(); // [1, 2, 3]
const spliced = [1, 2, 3, 4].toSpliced(1, 2, 9, 8); // [1, 9, 8, 4]
const withReplaced = [1, 2, 3].with(1, 99); // [1, 99, 3]

// Hashbang 语法
#!/usr/bin/env node
console.log("Hello");

// Symbols as WeakMap keys
const sym = Symbol();
const weakMap = new WeakMap();
weakMap.set(sym, "value");
```

### ES2024（最新提案）

**即将或已经包含的特性：**

```javascript
// Array grouping
Object.groupBy([
    {type: "fruit", name: "apple"},
    {type: "vegetable", name: "carrot"},
    {type: "fruit", name: "banana"}
], item => item.type);
// {
//   fruit: [{type: "fruit", name: "apple"}, {type: "fruit", name: "banana"}],
//   vegetable: [{type: "vegetable", name: "carrot"}]
// }

// Map.groupBy (类似)

// Promise.withResolvers
const {promise, resolve, reject} = Promise.withResolvers();

// 正则表达式修饰符
// v 标志（集合表示法）
/[\p{Emoji}--\p{Emoji_Component}]/v

// 装饰器（阶段 3）
@logged
class MyClass {
    @readonly
    method() {}
}
```

---

## 第七章：前端框架演进

### 第一代：库时代（2006-2012）

**jQuery（2006）**
```javascript
$("div.container")
    .find("p")
    .addClass("highlight")
    .click(function() {
        $(this).hide();
    });
```

**Backbone.js（2010）**
```javascript
var Todo = Backbone.Model.extend({
    defaults: {
        title: "",
        completed: false
    }
});

var todo = new Todo({title: "Learn JavaScript"});
todo.save();
```

### 第二代：框架崛起（2013-2016）

**Angular（2010，2016 重写）**
```typescript
@Component({
    selector: "app-root",
    template: `<h1>{{title}}</h1>`
})
export class AppComponent {
    title = "My App";
}
```

**React（2013）**
```jsx
function Welcome(props) {
    return <h1>Hello, {props.name}</h1>;
}

class Counter extends React.Component {
    state = {count: 0};
    
    render() {
        return (
            <button onClick={() => this.setState({count: this.state.count + 1})}>
                Count: {this.state.count}
            </button>
        );
    }
}
```

**Vue（2014）**
```vue
<template>
    <div>
        <h1>{{ title }}</h1>
        <button @click="count++">Count: {{ count }}</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            title: "My App",
            count: 0
        };
    }
};
</script>
```

### 第三代：现代框架（2017-至今）

**React Hooks（2018）**
```jsx
function Counter() {
    const [count, setCount] = useState(0);
    const [name, setName] = useState("");
    
    useEffect(() => {
        document.title = `Count: ${count}`;
    }, [count]);
    
    return (
        <div>
            <input value={name} onChange={e => setName(e.target.value)} />
            <button onClick={() => setCount(count + 1)}>
                Count: {count}
            </button>
        </div>
    );
}
```

**Vue 3 Composition API（2020）**
```vue
<script setup>
import {ref, computed, onMounted} from "vue";

const count = ref(0);
const double = computed(() => count.value * 2);

function increment() {
    count.value++;
}

onMounted(() => {
    console.log("Component mounted");
});
</script>

<template>
    <button @click="increment">{{ double }}</button>
</template>
```

**Svelte（编译时框架）**
```svelte
<script>
    let count = 0;
    
    function increment() {
        count += 1;
    }
    
    $: doubled = count * 2;
</script>

<button on:click={increment}>
    {doubled}
</button>

<style>
    button {
        color: blue;
    }
</style>
```

**SolidJS（细粒度响应式）**
```jsx
function Counter() {
    const [count, setCount] = createSignal(0);
    const doubled = createMemo(() => count() * 2);
    
    return (
        <button onClick={() => setCount(count() + 1)}>
            {doubled()}
        </button>
    );
}
```

### 元框架时代（2016-至今）

**Next.js（React）**
```jsx
// pages/index.js
export async function getServerSideProps() {
    const res = await fetch("https://api.example.com/data");
    const data = await res.json();
    
    return {props: {data}};
}

export default function Home({data}) {
    return <div>{data.title}</div>;
}
```

**Nuxt（Vue）**
```vue
<!-- pages/index.vue -->
<script setup>
const {data} = await useFetch("/api/data");
</script>

<template>
    <div>{{ data.title }}</div>
</template>
```

**Remix（React，专注 Web 标准）**
```jsx
// routes/index.jsx
export async function loader() {
    const data = await fetchData();
    return json({data});
}

export default function Index() {
    const {data} = useLoaderData();
    return <div>{data.title}</div>;
}
```

**Astro（岛屿架构）**
```astro
---
import ReactComponent from "../components/ReactComponent.jsx";
const data = await fetchData();
---

<html>
    <body>
        <ReactComponent client:load data={data} />
    </body>
</html>
```

---

## 第八章：构建工具和生态

### 模块打包器

**Webpack（2012）**
```javascript
// webpack.config.js
module.exports = {
    entry: "./src/index.js",
    output: {
        filename: "bundle.js",
        path: __dirname + "/dist"
    },
    module: {
        rules: [
            {
                test: /\.jsx?$/,
                use: "babel-loader"
            },
            {
                test: /\.css$/,
                use: ["style-loader", "css-loader"]
            }
        ]
    }
};
```

**Rollup（2015）**
```javascript
// rollup.config.js
export default {
    input: "src/index.js",
    output: {
        file: "dist/bundle.js",
        format: "es"
    },
    plugins: [resolve(), babel()]
};
```

**Vite（2020）**
```javascript
// vite.config.js
export default {
    plugins: [vue()],
    server: {
        port: 3000,
        hmr: true
    },
    build: {
        rollupOptions: {
            output: {
                manualChunks: {
                    vendor: ["vue", "vue-router"]
                }
            }
        }
    }
};
```

**esbuild（2020）**
```javascript
require("esbuild").build({
    entryPoints: ["app.js"],
    bundle: true,
    outfile: "out.js",
    minify: true
}).catch(() => process.exit(1));
```

### 转译器

**Babel（2014）**
```javascript
// .babelrc
{
    "presets": [
        ["@babel/preset-env", {
            "targets": {
                "browsers": ["> 1%", "last 2 versions"]
            }
        }],
        "@babel/preset-react"
    ],
    "plugins": [
        "@babel/plugin-proposal-class-properties"
    ]
}
```

**TypeScript（2012）**
```typescript
interface User {
    id: number;
    name: string;
    email?: string;
}

function greet(user: User): string {
    return `Hello, ${user.name}!`;
}

class UserService {
    private users: User[] = [];
    
    async getUser(id: number): Promise<User | null> {
        return this.users.find(u => u.id === id) || null;
    }
}
```

### 包管理器

**npm（2010）**
```bash
npm install express
npm install --save-dev jest
npm run build
```

**yarn（2016）**
```bash
yarn add express
yarn add --dev jest
yarn build
```

**pnpm（2016）**
```bash
pnpm add express
pnpm add --save-dev jest
pnpm build
```

### 测试框架

**Jest**
```javascript
describe("Calculator", () => {
    test("adds numbers", () => {
        expect(add(2, 3)).toBe(5);
    });
    
    test("async operations", async () => {
        const data = await fetchData();
        expect(data).toHaveProperty("id");
    });
    
    test("mocking", () => {
        const mockFn = jest.fn();
        mockFn("arg");
        expect(mockFn).toHaveBeenCalledWith("arg");
    });
});
```

**Vitest**
```javascript
import {describe, it, expect, vi} from "vitest";

describe("My Component", () => {
    it("renders correctly", () => {
        // ...
    });
});
```

**Playwright / Cypress（E2E 测试）**
```javascript
// Playwright
import {test, expect} from "@playwright/test";

test("login flow", async ({page}) => {
    await page.goto("/login");
    await page.fill("#email", "test@example.com");
    await page.fill("#password", "password");
    await page.click("button[type=submit]");
    await expect(page).toHaveURL("/dashboard");
});
```

---

## 第九章：JavaScript 的应用领域

### 前端开发

- **单页应用（SPA）**：React、Vue、Angular
- **静态站点生成（SSG）**：Next.js、Nuxt、Gatsby
- **服务端渲染（SSR）**：Next.js、Nuxt
- **渐进式 Web 应用（PWA）**
- **Web Components**

### 后端开发

**Node.js 框架：**
- **Express**：最小灵活的框架
- **Koa**：下一代 Node.js 框架
- **Fastify**：高性能低开销
- **NestJS**：企业级框架（受 Angular 启发）

```javascript
// Fastify 示例
const fastify = require("fastify")();

fastify.get("/", async (request, reply) => {
    return {hello: "world"};
});

fastify.post("/users", async (request, reply) => {
    const {name, email} = request.body;
    const user = await createUser({name, email});
    return user;
});

fastify.listen({port: 3000});
```

### 移动开发

- **React Native**：Facebook 出品
- **Ionic**：基于 Web 技术
- **NativeScript**：原生性能
- **Expo**：React Native 的开发平台

```jsx
// React Native
import {View, Text, Button} from "react-native";

function App() {
    return (
        <View>
            <Text>Hello, Mobile!</Text>
            <Button title="Press me" onPress={() => {}} />
        </View>
    );
}
```

### 桌面应用

- **Electron**：VS Code、Slack、Discord
- **Tauri**：更轻量的替代品
- **NW.js**：早期的桌面框架

```javascript
// Electron 主进程
const {app, BrowserWindow} = require("electron");

function createWindow() {
    const win = new BrowserWindow({
        width: 800,
        height: 600,
        webPreferences: {
            nodeIntegration: true
        }
    });
    
    win.loadFile("index.html");
}

app.whenReady().then(createWindow);
```

### 游戏开发

- **Three.js**：3D 图形
- **Phaser**：2D 游戏框架
- **Babylon.js**：微软出品的 3D 引擎
- **PixiJS**：2D 渲染引擎

```javascript
// Three.js 示例
import * as THREE from "three";

const scene = new THREE.Scene();
const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
const renderer = new THREE.WebGLRenderer();

const geometry = new THREE.BoxGeometry();
const material = new THREE.MeshBasicMaterial({color: 0x00ff00});
const cube = new THREE.Mesh(geometry, material);

scene.add(cube);
camera.position.z = 5;

function animate() {
    requestAnimationFrame(animate);
    cube.rotation.x += 0.01;
    cube.rotation.y += 0.01;
    renderer.render(scene, camera);
}

animate();
```

### 数据科学和机器学习

- **TensorFlow.js**：浏览器中的 ML
- **Brain.js**：神经网络
- **Danfo.js**：类似 Pandas 的数据分析
- **Plotly.js**：数据可视化

```javascript
// TensorFlow.js
import * as tf from "@tensorflow/tfjs";

const model = tf.sequential();
model.add(tf.layers.dense({units: 1, inputShape: [1]}));

model.compile({loss: "meanSquaredError", optimizer: "sgd"});

const xs = tf.tensor2d([[1], [2], [3], [4]], [4, 1]);
const ys = tf.tensor2d([[2], [4], [6], [8]], [4, 1]);

await model.fit(xs, ys, {epochs: 250});

const result = model.predict(tf.tensor2d([[5]], [1, 1]));
result.print(); // 应该接近 10
```

### 物联网（IoT）

- **Johnny-Five**：Arduino 和 IoT 平台
- **Cylon.js**：机器人和物理计算
- **Espruino**：JavaScript 微控制器

```javascript
// Johnny-Five
const five = require("johnny-five");
const board = new five.Board();

board.on("ready", () => {
    const led = new five.Led(13);
    
    led.blink(500);
    
    board.repl.inject({
        stop: () => led.stop(),
        start: () => led.start()
    });
});
```

### 区块链和 Web3

- **web3.js**：以太坊交互
- **ethers.js**：更现代的以太坊库
- **Hardhat**：以太坊开发环境

```javascript
// web3.js
import Web3 from "web3";

const web3 = new Web3("https://mainnet.infura.io/v3/YOUR_KEY");

const contract = new web3.eth.Contract(ABI, contractAddress);

const balance = await contract.methods.balanceOf(address).call();

await contract.methods.transfer(toAddress, amount).send({from: myAddress});
```

---

## 第十章：知名应用案例

### 使用 JavaScript 构建的著名应用

| 应用 | 技术栈 | 说明 |
|------|--------|------|
| **Netflix** | React, Node.js | 全球流媒体巨头 |
| **Facebook** | React, GraphQL | React 的发源地 |
| **Instagram** | React, Django | 图片社交网络 |
| **Airbnb** | React, Node.js | 民宿预订平台 |
| **Uber** | Node.js, React | 打车服务 |
| **Twitter** | Node.js, React | 社交媒体 |
| **LinkedIn** | Node.js, React | 职业社交网络 |
| **PayPal** | Node.js | 支付平台 |
| **WhatsApp** | Erlang, JavaScript | 即时通讯 |
| **Slack** | Electron, React | 团队协作工具 |
| **Discord** | Electron, React | 游戏聊天平台 |
| **VS Code** | Electron, TypeScript | 流行代码编辑器 |
| **Figma** | WebGL, React | 在线设计工具 |
| **Notion** | React, Node.js | 笔记和协作 |
| **Twitch** | React, Node.js | 游戏直播平台 |

### 性能对比案例

**PayPal 迁移到 Node.js：**
- 页面响应时间减少 **35%**
- 每秒请求数翻倍
- 代码量减少 **40%**
- 开发人员效率提升

**Netflix 使用 Node.js：**
- 启动时间从 **40 分钟** 减少到 **1 分钟**
- 能够处理 **数百万** 并发用户
- A/B 测试更加灵活

---

## 第十一章：统计数据和排行榜

### 使用率统计（2024）

**前端：**
- **98.7%** 的网站使用 JavaScript（W3Techs）
- **95%** 的开发者使用 JavaScript（Stack Overflow 调查）
- **70%** 的开发者使用 React、Vue 或 Angular

**后端：**
- **47%** 的后端开发者使用 Node.js
- **npm** 拥有超过 **200 万** 个包
- 每天下载量超过 **30 亿** 次

**移动端：**
- **42%** 的移动应用使用 React Native 或类似技术

### 开发者调查（Stack Overflow 2023）

**最受欢迎的技术：**
1. JavaScript — 65.36%
2. HTML/CSS — 52.97%
3. Python — 48.07%
4. SQL — 47.52%
5. TypeScript — 38.87%

**最想学习的语言：**
1. Rust
2. Python
3. TypeScript
4. JavaScript
5. Go

**薪资水平（美国）：**
- JavaScript 开发者平均年薪：$110,000
- 全栈（含 JS）开发者：$120,000
- 高级前端工程师：$150,000+

### TIOBE 指数（2024 年 5 月）

| 排名 | 语言 | 占比 | 趋势 |
|------|------|------|------|
| 1 | Python | 15.03% | ↑ |
| 2 | C | 12.34% | ↓ |
| 3 | C++ | 11.28% | ↑ |
| 4 | Java | 10.92% | ↓ |
| 5 | **JavaScript** | 8.45% | ↑ |
| 6 | C# | 6.78% | → |
| 7 | Go | 4.23% | ↑ |
| 8 | Ruby | 2.89% | ↓ |
| 9 | PHP | 2.67% | ↓ |
| 10 | Swift | 2.34% | ↑ |

### GitHub 统计数据

**最流行的仓库（按星标）：**
1. freeCodeCamp — 400k+ ⭐
2. React — 220k+ ⭐
3. Vue.js — 210k+ ⭐
4. Node.js — 100k+ ⭐
5. Angular — 90k+ ⭐
6. Express — 60k+ ⭐
7. Three.js — 100k+ ⭐
8. D3.js — 110k+ ⭐

**贡献者数量：**
- Node.js：3,000+ 贡献者
- React：1,500+ 贡献者
- Vue.js：400+ 贡献者
- TypeScript：600+ 贡献者

---

## 第十二章：最佳实践和编码规范

### 代码风格指南

**Airbnb JavaScript Style Guide：**
```javascript
// 使用 const 和 let，不用 var
const name = "Alice";
let count = 0;

// 使用模板字符串
const greeting = `Hello, ${name}!`;

// 箭头函数优先
const add = (a, b) => a + b;

// 对象简写
const name = "Bob";
const user = {name}; // 等价于 {name: name}

// 使用解构
const {name, age} = user;

// 使用扩展运算符
const newArr = [...oldArr, newItem];

// 使用可选链
const street = user?.address?.street;

// 使用空值合并
const value = input ?? defaultValue;

// 使用 async/await
async function fetchData() {
    const response = await fetch("/api");
    return response.json();
}

// 使用 ES6 模块
import {something} from "./module.js";
export default Something;
```

### 性能优化

**1. 减少重绘和回流**
```javascript
// ❌ 差
element.style.width = "100px";
element.style.height = "200px";
element.style.color = "red";

// ✅ 好
element.style.cssText = "width: 100px; height: 200px; color: red;";
```

**2. 防抖和节流**
```javascript
// 防抖
function debounce(fn, delay) {
    let timer;
    return function(...args) {
        clearTimeout(timer);
        timer = setTimeout(() => fn.apply(this, args), delay);
    };
}

// 节流
function throttle(fn, limit) {
    let inThrottle;
    return function(...args) {
        if (!inThrottle) {
            fn.apply(this, args);
            inThrottle = true;
            setTimeout(() => inThrottle = false, limit);
        }
    };
}
```

**3. 懒加载**
```javascript
// 图片懒加载
const images = document.querySelectorAll("img[data-src]");

const imageObserver = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            const img = entry.target;
            img.src = img.dataset.src;
            imageObserver.unobserve(img);
        }
    });
});

images.forEach(img => imageObserver.observe(img));

// 代码分割
const Module = () => import("./heavy-module.js");
```

**4. 使用 Web Workers**
```javascript
// 主线程
const worker = new Worker("worker.js");
worker.postMessage({data: largeDataset});
worker.onmessage = (e) => {
    console.log("Result:", e.data);
};

// worker.js
self.onmessage = (e) => {
    const result = heavyComputation(e.data);
    self.postMessage(result);
};
```

### 安全最佳实践

**1. XSS 防护**
```javascript
// ❌ 危险
element.innerHTML = userInput;

// ✅ 安全
element.textContent = userInput;

// 或使用库
import DOMPurify from "dompurify";
element.innerHTML = DOMPurify.sanitize(userInput);
```

**2. CSRF 防护**
```javascript
// 使用 token
fetch("/api/action", {
    method: "POST",
    headers: {
        "X-CSRF-Token": csrfToken
    },
    body: JSON.stringify(data)
});
```

**3. 依赖安全**
```bash
# 定期审计
npm audit
yarn audit

# 自动修复
npm audit fix
```

### 测试策略

```javascript
// 单元测试
describe("UserService", () => {
    it("should create user", () => {
        const user = UserService.create({name: "Alice"});
        expect(user.id).toBeDefined();
    });
});

// 集成测试
describe("API Integration", () => {
    it("should fetch user data", async () => {
        const response = await fetch("/api/users/1");
        const user = await response.json();
        expect(user.name).toBe("Alice");
    });
});

// E2E 测试
describe("Login Flow", () => {
    it("should login successfully", async () => {
        await page.goto("/login");
        await page.fill("#email", "test@example.com");
        await page.fill("#password", "password");
        await page.click("button[type=submit]");
        await expect(page).toHaveURL("/dashboard");
    });
});
```

---

## 第十三章：学习资源和路径

### 入门资源

**免费在线教程：**
- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide)
- [freeCodeCamp](https://www.freecodecamp.org/)
- [JavaScript.info](https://javascript.info/)
- [Codecademy JavaScript Course](https://www.codecademy.com/learn/introduction-to-javascript)

**书籍推荐：**
- 《JavaScript 高级程序设计》（红宝书）
- 《你不知道的 JavaScript》系列
- 《JavaScript 权威指南》（犀牛书）
- 《Eloquent JavaScript》（免费在线版）

**视频课程：**
- [The Modern JavaScript Tutorial](https://javascript.info/)
- [Frontend Masters](https://frontendmasters.com/)
- [Udemy JavaScript Courses](https://www.udemy.com/)

### 进阶主题

**必须掌握的概念：**
1. 闭包和作用域
2. this 关键字
3. 原型和继承
4. 异步编程（Promise、async/await）
5. 事件循环
6. 模块系统
7. 设计模式
8. 函数式编程

**练习平台：**
- [LeetCode](https://leetcode.com/)
- [HackerRank](https://www.hackerrank.com/)
- [Codewars](https://www.codewars.com/)
- [Exercism](https://exercism.org/tracks/javascript)

### 实战项目建议

**初级：**
- 待办事项列表
- 天气预报应用
- 计算器
- 简单的游戏（井字棋、猜数字）

**中级：**
- 博客系统
- 电子商务网站
- 实时聊天应用
- 地图应用

**高级：**
- 社交网络平台
- 在线协作工具
- 数据可视化仪表板
- 完整的 SaaS 应用

### 社区和资源

**论坛和社区：**
- [Stack Overflow](https://stackoverflow.com/questions/tagged/javascript)
- [Reddit r/javascript](https://www.reddit.com/r/javascript/)
- [Dev.to](https://dev.to/t/javascript)
- [Hashnode](https://hashnode.com/)

**播客：**
- JavaScript Jabber
- Syntax.fm
- Front End Happy Hour
- ShopTalk Show

**YouTube 频道：**
- Fireship
- Traversy Media
- The Net Ninja
- Web Dev Simplified
- freeCodeCamp

**新闻通讯：**
- JavaScript Weekly
- Node Weekly
- Frontend Focus
- Smashing Newsletter

---

## 第十四章：挑战与未来展望

### 当前挑战

**1. 复杂性爆炸**
- 框架太多，选择困难
- 构建配置复杂
- 学习曲线陡峭
- 技术更新太快

**2. 性能问题**
- 大型应用加载慢
- 内存泄漏
- 移动端性能不足
- SEO 挑战

**3. 安全问题**
- 依赖漏洞
- XSS 攻击
- CSRF 攻击
- 供应链攻击

**4. 碎片化**
- 浏览器兼容性
- 不同运行环境
- 模块格式混乱
- 工具链碎片化

### 未来趋势

**1. TypeScript 的普及**
```typescript
// 类型安全成为标配
interface User {
    id: number;
    name: string;
    email: string;
}

function greet(user: User): string {
    return `Hello, ${user.name}!`;
}
```

**2. 边缘计算**
```javascript
// Cloudflare Workers
export default {
    async fetch(request, env) {
        return new Response("Hello from the edge!");
    }
};
```

**3. WebAssembly 集成**
```javascript
// JavaScript 与 WASM 协作
const wasmModule = await WebAssembly.instantiateStreaming(
    fetch("module.wasm")
);
const result = wasmModule.instance.exports.add(2, 3);
```

**4. 服务端组件**
```jsx
// React Server Components
async function UserProfile({userId}) {
    const user = await db.user.find(userId);
    return <div>{user.name}</div>;
}
```

**5.  Islands 架构**
```astro
---
// Astro：只有交互式组件发送 JavaScript
import InteractiveComponent from "../components/Interactive.jsx";
---
<InteractiveComponent client:load />
```

**6. 更好的开发者体验**
- 更快的构建工具（esbuild、swc）
- 更好的错误提示
- 智能代码补全
- 自动化重构工具

**7. 去中心化 Web**
- Web3 和区块链集成
- IPFS 存储
- 去中心化身份
- P2P 通信

### JavaScript 的未来地位

**预测（2025-2030）：**

✅ **继续主导前端开发**
- 没有其他语言能威胁其地位
- Web 标准将继续围绕 JavaScript 构建

✅ **后端份额稳定增长**
- Node.js 持续改进
- Deno、Bun 等新运行时出现
- 边缘计算推动服务器端 JS

✅ **跨平台开发主流化**
- 移动端：React Native、Flutter（Dart）
- 桌面端：Electron、Tauri
- IoT：更多 JavaScript 支持

✅ **与 AI/ML 深度融合**
- TensorFlow.js 等库成熟
- 浏览器内推理成为常态
- AI 辅助编程工具普及

⚠️ **面临挑战**
- WebAssembly 可能分担部分计算密集型任务
- 新语言（如 Mojo、Carbon）的出现
- 低代码/无代码平台的兴起

---

## 结语

从 1995 年那个 10 天诞生的"玩具语言"，到如今统治互联网的霸主，JavaScript 走过了近 30 年的传奇历程。

**它证明了：**
- 简单和灵活比完美设计更重要
- 社区生态比语言特性更有价值
- 适应性和演化能力是生存的关键
- 开放标准胜过专有技术

**给学习者的建议：**

1. **打好基础**：深入理解核心概念，不要只学框架
2. **持续学习**：技术更新快，保持好奇心
3. **动手实践**：写代码是最好的学习方式
4. **参与社区**：开源、分享、交流
5. **关注本质**：框架会变，原理永恒

正如布兰登·艾克所说：
> "JavaScript 的美在于它的包容性和适应性。它不是完美的，但它属于每个人。"

下一个 30 年，JavaScript 仍将是构建数字世界的核心工具。而你，可以是这个未来的创造者之一。

---

## 附录：快速参考

### 版本时间线

| 年份 | 版本 | 关键特性 |
|------|------|---------|
| 1995 | Mocha/LiveScript | 诞生 |
| 1997 | ES1 | 首个标准 |
| 1999 | ES3 | 正则、异常处理 |
| 2009 | ES5 | 严格模式、JSON、数组方法 |
| 2015 | ES6 | 类、模块、箭头函数、Promise |
| 2016 | ES7 | includes、指数运算符 |
| 2017 | ES8 | async/await |
| 2018 | ES9 | 异步迭代、对象 rest/spread |
| 2019 | ES10 | flat、fromEntries |
| 2020 | ES11 | BigInt、可选链、空值合并 |
| 2021 | ES12 | replaceAll、Promise.any |
| 2022 | ES13 | 类的字段、top-level await |
| 2023 | ES14 | findLast、toSorted |
| 2024 | ES15 | 分组、装饰器（提案） |

### 常用命令

```bash
# 初始化项目
npm init -y

# 安装包
npm install package-name
npm install --save-dev package-name

# 运行脚本
npm run dev
npm run build
npm test

# 更新依赖
npm update
npm outdated

# 安全检查
npm audit
npm audit fix
```

### 调试技巧

```javascript
// console 方法
console.log("普通日志");
console.error("错误日志");
console.warn("警告");
console.table([{name: "Alice"}, {name: "Bob"}]);
console.time("timer");
// ... code
console.timeEnd("timer");

// debugger 语句
function problematicFunction() {
    debugger; // 浏览器会在此处暂停
    // ...
}

// Source Map
// 确保构建工具生成了 .map 文件
```

---

**参考资料：**
- [ECMAScript 规范](https://tc39.es/ecma262/)
- [MDN JavaScript 文档](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- [JavaScript 编年史](https://jamesgreig.ca/javascript-timeline)
- [Node.js 历史](https://nodejs.org/en/about/history/)
- [TC39 提案流程](https://github.com/tc39/proposals)

**本文最后更新：** 2024 年 5 月
