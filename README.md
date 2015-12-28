# 浅尝ECMAScript6

## 简介

ECMAScript6 是最新的ECMAScript标准，于2015年6月正式推出(所以也称为ECMAScript 2015),相比于2009年推出的es5, es6定义了更加丰富的语言特性，基于该标准的Javascript语言也迎来了语法上的重大变革。本文列举了部分es6新特性，希望之前没接触es6的小伙伴读完本文能对下一代js编程有一个初步的认识。

#### 箭头函数
箭头函数用 "=>"简化函数定义，类似于C#, Java8中的Lambda表达式,支持语句块和表达式函数体，和普通函数的唯一区别在于函数体引用的 this 和包裹代码的this一致。

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

// this , 这里 this引用 leon
var bob = {
  _name: "leon",
  _friends: [],
  printFriends() {
    this._friends.forEach(f =>
      console.log(this._name + " knows " + f));
  }
}
```

#### 类
ES6的类简单而言只是基于原型继承的语法糖, 使用方便的声明式定义，使得类型定义和其他面向对象语言一致。类支持原型继承，父类调用，实体和静态方法以及构造函数。

```JavaScript

class Animal { 
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    console.log(this.name + ' makes a noise.');
  }
}

class Cat extends Animal {
  constructor(name, color) {
     // 调用父类的构造函数
     super(name);
     this.color = color;
  }

  speak() {
    // 调用父类的 speak 方法
    super.speak();
    console.log(this.name + ' miao~~~.');
  }
}

```

#### 增强的对象字面量
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

#### 模板字符串
模板字符串提供了一种构建字符串的语法糖，类似于Perl, Python和其他语言中的字符串插值特性. 

```JavaScript

// 多行字符串
`In JavaScript this is
 not legal.`

// 字符串插值
var name = "Leon", time = "today";
`Hello ${name}, how are you ${time}?`

```

#### 解构赋值
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

#### 默认参数和不定参数
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

#### let + const 块级作用域变量
let 和 const 声明的变量具有块级作用域, 传统使用var申明的变量在整个函数内都可访问。const声明的变量只可以在声明时赋值，不可随意修改，否则会导致SyntaxError（语法错误）。


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

#### 迭代器 和 for..of
迭代器类似于 .NET CLR 的 IEnumerable 或 Java 的 Iterable, 所有拥有[Symbol.iterator]的对象被称为可迭代的, 而 for ..of 用于遍历实现了迭代器方法的对象，比如Array, Map, Set, Array-like Object


```JavaScript
for (var value of [1,2,3]) {
  // 依次打印 1,2,3
  console.log(value); 
}

```

#### 生成器

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

#### 模块
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

#### Map + Set + WeakMap + WeakSet

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

#### Symbol
Symbol是es6新添加的一个基元数据类型, 其他的基元类型有string,number,boolean,null,undefined. Symbol是唯一的，一般用作对象的key以存取相关状态信息。


```JavaScript
var MyClass = (function() {

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

#### 子类化内置对象
ES6中的内置对象，比如 Array, Date 等可以被子类继承

```JavaScript

// 这里定义一个Array的子类MyArray
class MyArray extends Array {
    constructor(...args) { super(...args); }
}

```

#### Math + Number + String + Array + Object 新增的方法和属性

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

#### 二进制和八进制字面量

```JavaScript
// 二进制
0b111110111 === 503 // true

// 八进制
0o767 === 503 // true
```

#### Promises
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
## 结语
除了以上特性，es6还增强了对unicode编码的支持，以便更好的支持应用国际化, 还有代理等等特性，这里就不一一列举了，目前javascript开发范围越来广泛，web, 移动应用，智能家居...记得哪个牛人说过这样一句话，
"未来所有可以用js编写的应用最终都会用js编写"。今年es6规范正式推出，尽管当前各厂商浏览器和js处理引擎对es6还没有完全支持(可以用babel编译成es5)，但是未来某一天，es6的几乎所有特性都会被全部支持，所以开始尝试es6是一件很值得花时间的事情。
