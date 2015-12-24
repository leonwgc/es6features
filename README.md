# ECMAScript 6 <sup>[git.io/es6features](http://git.io/es6features)</sup>

## Introduction
<!-- ECMAScript 6, also known as ECMAScript 2015, is the latest version of the ECMAScript standard.  ES6 is a significant update to the language, and the first update to the language since ES5 was standardized in 2009. Implementation of these features in major JavaScript engines is [underway now](http://kangax.github.io/es5-compat-table/es6 -->

ECMAScript6 是最新版本的ECMAScript标准，于2015年6月正式推出(所以也称为ECMAScript 2015),相比于2009年推出的es5规范,es6定义了更加丰富的语言特性，基于该规范的重量级实现，JS也迎来了语法上的重大变革，JS正变得越来越强大。



See the [ES6 standard](http://www.ecma-international.org/ecma-262/6.0/) for full specification of the ECMAScript 6 language.

<!-- ES6 includes the following new features: -->
ES6包含以下新的特性:
- [箭头函数](#_1)
- [类](#classes)
- [增强的对象字面量](#enhanced-object-literals)
- [模板字符串](#template-strings)
- [解构](#destructuring)
- [默认参数和不定参数](#default--rest--spread)
- [let + const 块级作用域变量](#let--const)
- [迭代器 + for..of](#iterators--forof)
- [generators](#generators)
- [unicode](#unicode)
- [模块](#modules)
- [模块加载器](#module-loaders)
- [map + set + weakmap + weakset](#map--set--weakmap--weakset)
- [代理](#proxies)
- [symbols](#symbols)
- [subclassable built-ins](#subclassable-built-ins)
- [promises](#promises)
- [math + number + string + array + object APIs](#math--number--string--array--object-apis)
- [binary and octal literals](#binary-and-octal-literals)

## ECMAScript 6 特性

### 箭头函数
箭头函数用 "=>"简化函数定义，语义上类似于C#, Java8 和 CoffeeScript (Lambda表达式),支持语句块和表达式函数体，和函数的唯一区别在于函数体引用的 this 和包裹代码的this一致。

```JavaScript
// 表达式
var odds = evens.map(v => v + 1);
var nums = evens.map((v, i) => v + i);
var pairs = evens.map(v => ({even: v, odd: v + 1}));

// 代码块
nums.forEach(v => {
  if (v % 5 === 0)
    fives.push(v);
});

// this , 这里 this引用bob
var bob = {
  _name: "Bob",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```

### 类
ES6的类只是基于原型继承的简单语法糖, 使用方便的声明式定义，使得类型定义和其他面向对象语言一致。类支持原型继承，父类调用，实体和静态方法以及构造函数。

```JavaScript
class SkinnedMesh extends THREE.Mesh {
  constructor(geometry, materials) {
    super(geometry, materials);

    this.idMatrix = SkinnedMesh.defaultMatrix();
    this.bones = [];
    this.boneMatrices = [];
    //...
  }
  update(camera) {
    //...
    super.update();
  }
  get boneCount() {
    return this.bones.length;
  }
  set matrixType(matrixType) {
    this.idMatrix = SkinnedMesh[matrixType]();
  }
  static defaultMatrix() {
    return new THREE.Matrix4();
  }
}
```

### 增强的对象字面量
对象字面量现在支持在对象构建时设置原型对象，定义方法，调用父类构造函数，以及用表达式计算属性. 这些特性使得对象字面量定义和类型定义十分相似， 给基于对象的设计带来了便利。

```JavaScript
var obj = {
    // 原型对象
    __proto__: theProtoObj,
    // 简化定义成员, ‘handler: handler’
    handler,
    // 方法
    toString() {
     // 父类调用
     return "d " + super.toString();
    },
    // 动态属性名
    [ 'prop_' + (() => 42)() ]: 42
};
```

### 模板字符串
模板字符串提供了一种构建字符串的语法糖，类似于Perl, Python和其他语言中的字符串插值特性. 

```JavaScript

// 多行字符串
`In JavaScript this is
 not legal.`

// 字符串插值
var name = "Leon", time = "today";
`Hello ${name}, how are you ${time}?`

```

### 解构赋值
解构赋值允许你使用类似数组或对象字面量的语法将数组和对象的属性赋给各种变量。这种赋值语法极度简洁，同时还比传统的属性访问方法更为清晰。



```JavaScript
// 数组  a=1,b=2,c=3
var [a, b, c] = [1,2,3];

// 数组,没有匹配的返回undefined , a=1,b=2,c=undefined
var [a, b, c] = [1,2];

// 对象, m='leon', n=18
var {name:m,age:n}={name:'leon',age:18}

// 对象属性名与变量名一致时，可以简写为
var {foo,bar}={foo:'1',bar:2}

// 嵌套 name='leon'
var {x:{y:{z:name}}}={x:{y:{z:'leon'}}}

```

### 默认参数和不定参数
函数调用者不需要传递所有可能存在的参数，没有被传递的参数由默认参数进行填充。
在所有函数参数中，只有最后一个才可以被标记为不定参数。函数被调用时，不定参数前的所有参数都正常填充，任何“额外的”参数都被放进一个数组中并赋值给不定参数。如果没有额外的参数，不定参数就是一个空数组，它永远不会是undefined。

```JavaScript
// 默认参数
function f(x, y=12) {
  // 如果没有传入y 或传入的值为undefined ,则y=12
  return x + y;
}
f(3) == 15
```
```JavaScript
// 不定参数
function f(x, ...y) {
  // y 是一个数组
  return x * y.length;
}
f(3, "hello", true) == 6
```
```JavaScript
function f(x, y, z) {
  return x + y + z;
}
// 数组中的每个元素作为参数传入
f(...[1,2,3]) == 6
```

### let + const 块级作用域变量
Let 和 Const 声明的变量具有块级作用域, 传统使用var申明的变量在整个函数内都可访问。const声明的变量只可以在声明时赋值，不可随意修改，否则会导致SyntaxError（语法错误）。


```JavaScript
function f() {
  {
    let x;
    {
      const x = "me";
      // 因为是const， 所以不可修改，报错
      x = "foo";
    }
    // 报错， 用let重定义变量会抛出一个语法错误
    let x = "inner";
  }
}
```

### 迭代器 和 for..of
迭代器类似于 .NET CLR 的 IEnumerable 或 Java 的 Iterable, 所有拥有[Symbol.iterator]()的对象被称为可迭代的, 而 for ..of 用于遍历实现了迭代器方法的对象，比如Array, Map, Set, Array-like Object


```JavaScript
for (var value of [1,2,3]) {
  // 依次打印 1,2,3
  console.log(value); 
}

```

### 生成器

生成器对象由生成器函数(function* 定义的函数)返回,它同时准守iterator 和 Iterable 协议。著名的koa nodejs框架就是基于此特性构建。

```JavaScript
function* gen() { 
  yield 1;
  yield 2;
  yield 3;
}

// g 是生成器对象
var g = gen(); 
```

### Unicode
增加了一些新的Unicode字符，并且增加了新的正则表达式匹配模式`u`, 更方便的支持国际化的js应用

```JavaScript

"𠮷".match(/./u)[0].length == 2

"\u{20BB7}"=="𠮷"=="\uD842\uDFB7"

"𠮷".codePointAt(0) == 0x20BB7

```

### 模块
模块已经得到语言级别的支持，ES6的模块设计参照了AMD CommonJS 规范。

```JavaScript
// lib/math.js
// export 定义要导出的对象
export function sum(x, y) {
  return x + y;
}
export var pi = 3.141593;

// 没加export, 所以foo不被导出
function foo(){}
```
```JavaScript
// app.js
// 导入全部在lib/math.js中标记为导出的对象
import * as math from "lib/math";
alert("2π = " + math.sum(math.pi, math.pi));
```
```JavaScript
// otherApp.js 
// 显示标记需要导入的成员
import {sum, pi} from "lib/math";
alert("2π = " + sum(pi, pi));
```

```JavaScript
// lib/mathplusplus.js
// 导入lib/math中的全部对象并重新导出
export * from "lib/math";
export var e = 2.71828182846;
// 默认导出对象
export default function(x) {
    return Math.log(x);
}
```
```JavaScript
// app.js
// ln为上面的默认导出对象, pi来自于lib/math,e来自于mathplusplus
import ln, {pi, e} from "lib/mathplusplus";
alert("2π = " + ln(e)*pi*2);
```

### Module Loaders
Module loaders support:
- Dynamic loading
- State isolation
- Global namespace isolation
- Compilation hooks
- Nested virtualization

The default module loader can be configured, and new loaders can be constructed to evaluate and load code in isolated or constrained contexts.

```JavaScript
// Dynamic loading – ‘System’ is default loader
System.import('lib/math').then(function(m) {
  alert("2π = " + m.sum(m.pi, m.pi));
});

// Create execution sandboxes – new Loaders
var loader = new Loader({
  global: fixup(window) // replace ‘console.log’
});
loader.eval("console.log('hello world!');");

// Directly manipulate module cache
System.get('jquery');
System.set('jquery', Module({$: $})); // WARNING: not yet finalized
```

### Map + Set + WeakMap + WeakSet

```JavaScript
// Sets 
var s = new Set();
s.add("hello").add("goodbye").add("hello");
s.size === 2;
s.has("hello") === true;

// Maps
var m = new Map();
m.set("hello", 42);
m.set(s, 34);
m.get(s) == 34;

// Weak Maps
var wm = new WeakMap();
wm.set(s, { extra: 42 });
wm.size === undefined

// Weak Sets
var ws = new WeakSet();
ws.add({ data: 42 });
```

### Proxies
Proxies enable creation of objects with the full range of behaviors available to host objects.  Can be used for interception, object virtualization, logging/profiling, etc.

```JavaScript
// Proxying a normal object
var target = {};
var handler = {
  get: function (receiver, name) {
    return `Hello, ${name}!`;
  }
};

var p = new Proxy(target, handler);
p.world === 'Hello, world!';
```

```JavaScript
// Proxying a function object
var target = function () { return 'I am the target'; };
var handler = {
  apply: function (receiver, ...args) {
    return 'I am the proxy';
  }
};

var p = new Proxy(target, handler);
p() === 'I am the proxy';
```

There are traps available for all of the runtime-level meta-operations:

```JavaScript
var handler =
{
  get:...,
  set:...,
  has:...,
  deleteProperty:...,
  apply:...,
  construct:...,
  getOwnPropertyDescriptor:...,
  defineProperty:...,
  getPrototypeOf:...,
  setPrototypeOf:...,
  enumerate:...,
  ownKeys:...,
  preventExtensions:...,
  isExtensible:...
}
```

### Symbols
Symbols enable access control for object state.  Symbols allow properties to be keyed by either `string` (as in ES5) or `symbol`.  Symbols are a new primitive type. Optional `description` parameter used in debugging - but is not part of identity.  Symbols are unique (like gensym), but not private since they are exposed via reflection features like `Object.getOwnPropertySymbols`.


```JavaScript
var MyClass = (function() {

  // module scoped symbol
  var key = Symbol("key");

  function MyClass(privateData) {
    this[key] = privateData;
  }

  MyClass.prototype = {
    doStuff: function() {
      ... this[key] ...
    }
  };

  return MyClass;
})();

var c = new MyClass("hello")
c["key"] === undefined
```

### Subclassable Built-ins
In ES6, built-ins like `Array`, `Date` and DOM `Element`s can be subclassed.

Object construction for a function named `Ctor` now uses two-phases (both virtually dispatched):
- Call `Ctor[@@create]` to allocate the object, installing any special behavior
- Invoke constructor on new instance to initialize

The known `@@create` symbol is available via `Symbol.create`.  Built-ins now expose their `@@create` explicitly.

```JavaScript
// Pseudo-code of Array
class Array {
    constructor(...args) { /* ... */ }
    static [Symbol.create]() {
        // Install special [[DefineOwnProperty]]
        // to magically update 'length'
    }
}

// User code of Array subclass
class MyArray extends Array {
    constructor(...args) { super(...args); }
}

// Two-phase 'new':
// 1) Call @@create to allocate object
// 2) Invoke constructor on new instance
var arr = new MyArray();
arr[1] = 12;
arr.length == 2
```

### Math + Number + String + Array + Object 新增的方法属性

```JavaScript
Number.EPSILON
Number.isInteger(Infinity) // false
Number.isNaN("NaN") // false

Math.acosh(3) // 1.762747174039086
Math.hypot(3, 4) // 5
Math.imul(Math.pow(2, 32) - 1, Math.pow(2, 32) - 2) // 2

"abcde".includes("cd") // true
"abc".repeat(3) // "abcabcabc"

Array.from(document.querySelectorAll('*')) // 返回一个真实的数组
[1, 2, 3].find(x => x == 3) // 3
[1, 2, 3].findIndex(x => x == 2) // 1
["a", "b", "c"].keys() // iterator 0, 1, 2
["a", "b", "c"].values() // iterator "a", "b", "c"

Object.assign(Point, { origin: new Point(0,0) })
```

### 二进制和八进制字面量

```JavaScript
// 二进制
0b111110111 === 503 // true

// 八进制
0o767 === 503 // true
```

### Promises
Promise 是异步编程的一种规范，解决了js异步编程中callback hell问题, 在es6中原生支持.

```JavaScript
function timeout(duration = 0) {
    return new Promise((resolve, reject) => {
        setTimeout(resolve, duration);
    })
}

var p = timeout(1000).then(() => {
    return timeout(2000);
}).then(() => {
    throw new Error("hmm");
}).catch(err => {
    return Promise.all([timeout(100), timeout(200)]);
})
```
