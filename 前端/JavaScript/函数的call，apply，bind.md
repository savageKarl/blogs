# 函数的 call，apply，bind

函数的`call`，`apply`，`bind`方法是定义在 Funtion 构造器原型上的方法，都是用来改变函数的`this`指向。

## call

call() 方法使用一个指定的 this 值和单独给出的一个或多个参数来调用一个函数。

```javasciprt
function.call(thisArg, arg1, arg2, ...)
```

```javascript
const foo = {
  name: "savage",
};

const obj = {
  name: "bar",
  printName() {
    console.debug(this.name, ...arguments);
  },
};

obj.printName(); // bar
obj.printName.call(foo, "x", "y", "z"); //savage x y z
```

## call

call() 方法使用一个指定的 this 值和单独给出的一个或多个参数来调用一个函数。

```javasciprt
function.call(thisArg, arg1, arg2, ...)
```

```javascript
const foo = {
  name: "savage",
};

const obj = {
  name: "bar",
  printName() {
    console.debug(this.name, ...arguments);
  },
};

obj.printName(); // bar
// 传入多个分散的参数
obj.printName.call(foo, "x", "y", "z"); //savage x y z
```

## apply

apply() 方法使用一个指定的 this 值和以一个数组或一个类数组参数来调用一个函数。

```javasciprt
function.apply(thisArg, arg)
```

`apply`会将 arg 集合分散成单个的参数传入函数

```javascript
const foo = {
  name: "savage",
};

const obj = {
  name: "bar",
  printName() {
    console.debug(this.name, ...arguments);
  },
};

obj.printName(); // bar
// 传入一个集合
obj.printName.apply(foo, ["x", "y", "z"]); //savage x y z
```

## bind

```
function.bind(thisArg, arg1, arg2, ...)
```

`bind`方法会返回一个原函数的拷贝，然后指定原函数的`this`是传入`thisArg`参数，而可选的`arg`参数，会在新函数调用的时候，自动传入新函数。

```javascript
const foo = {
  name: "savage",
};

const obj = {
  name: "bar",
  printName() {
    console.debug(this.name, ...arguments);
  },
};

obj.printName(); // bar

const newPrintName = obj.printName.bind(foo, "a", "b");
newPrintName("c"); // savage a b c
```
