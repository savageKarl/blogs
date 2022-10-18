# js 的数据类型

## 数据类型的分类

**数据类型分为两种**，原始值（基本）类型和引用类型。

- 原始值类型：表示按值传递，无法改变原始值类型的值。

- 引用类型：按引用地址传递，改变的会是引用类型的值。

**原始值类型有**：
- 字符串（String）
- 数字(Number)
- 布尔(Boolean)
- 空对象（Null）指示变量未指向任何对象
- 未定义（Undefined）
- 符号(symbol)

除了原始值类型之外的都是引用类型

**引用类型**
- Object
- Array
- Function
- Date
- RegExp

上面只是写出常用的引用类型，引用的所有的种类可以参考MDN文档

## typeof

typeof 关键字只能返回原始值类型以及一个特殊function类型，这里不知道为什么要返回一个function类型，可能是因为js是一门函数优先的语言。

```javascript
console.debug(typeof "savage"); // string
console.debug(typeof 1); // number
console.debug(typeof null); // object 这里是由于历史原因，但是意义不大
console.debug(typeof false); // boolean
console.debug(typeof BigInt(1)); // biginit
console.debug(typeof Symbol()); // symbol
console.debug(typeof function () {}); // function
```

除了上面之外的，其他的引用类型都会返回`object`

```javascript
console.debug(typeof {});  // object
console.debug(typeof []);  // object
console.debug(typeof /reg/);  // object
console.debug(typeof new Date());  // object
console.debug(typeof Math);  // object
```

## instanceof

`instanceof`关键字用于表示一个对象是否一个构造器函数的实例，可以用来判断数据类型，只能判断引用数据类型。

```javascript
console.debug( {} instanceof Object);  // true
console.debug([] instanceof Array);  // true
console.debug(/reg/ instanceof RegExp);  // true
console.debug(new Date() instanceof Date);  // true
```


## Object.prototype.toString

`Object.prototype.toString`表示返回一个对象的字符串，可以用来判断数据类型，但是不一定可靠。

`Object.prototype.toString.call(x)`返回 `[object xxxx]`，这个xxx无论是基本类型还是引用类型，都是大写的。
```javascript
console.debug(Object.prototype.toString.call(1)); // [object Number]
console.debug(Object.prototype.toString.call(false)); // [object Boolean]
console.debug(Object.prototype.toString.call(undefined)); // [object Undefined]
console.debug(Object.prototype.toString.call(null)); // [object Null]
console.debug(Object.prototype.toString.call([])); // [object Array]
console.debug(Object.prototype.toString.call(/reg/)); // [object RegExp]
console.debug(Object.prototype.toString.call(new Date())); // [object Date]
console.debug(Object.prototype.toString.call(Math)); // [object Math]
```

上面说的`Object.prototype.toString`不可靠，是因为ES6的`[Symbol.toStringTag]`对象属性，可以改变`Object.prototype.toString`的值。

```javascript
const foo = {};
console.debug(Object.prototype.toString.call(foo)); // [object Object]

foo[Symbol.toStringTag] = "bar";
console.debug(Object.prototype.toString.call(foo)); // [object bar]

const arr = [];
arr[Symbol.toStringTag] = "savage";
console.debug(Object.prototype.toString.call(arr)); // [object savage]
```

注意：`[Symbol.toStringTag]`只能修改引用类型，不能修改基础类型


```javascript
const str = '';
str[Symbol.toStringTag] = "savage";
console.debug(Object.prototype.toString.call(str)); // [object String]
```