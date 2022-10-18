# 函数的 this 指向

函数的 this 指向问题，分为两种情况，普通函数和箭头函数。

## 普通函数

### 定义

用`function`关键字或者`Funtion`构造函数定义的函数

```javascript
function fun() {
  console.debug(this);
}

const fun2 = function () {
  console.debug(this);
};

const fun3 = Function("console.debug(this)");
```

### this 指向

普通函数的`this`指向是动态指定的，除了被`call`，`apply`，`bind`，改变`this`之外，默认是谁调用它，它就指向谁，如果是全局调用，浏览器端指向`window`，node 端指向`gloabl`。

```javascript
function fun() {
  console.debug(this);
}

fun(); // global {...}

const savage = {
  say() {
    console.debug(this);
  },
};

savage.say(); // savage {...}
```

## 箭头函数

### 定义

用`=>`语法定义的函数

```javascript
const fun4 = () => {
  console.debug(this);
};
```

### this 指向

箭头函数的`this`指向是静态指定的，`call`，`apply`，`bind`是无法改变，默认的情况是，指向它自身所定义位置的父函数，如果没有父函数，浏览器端指向`window`，node 端指向空对象`{}`

```javascript
const func = () => {
  console.debug(this);
};

func(); // {}

const savage = {
  say() {
    console.debug(this); // { say: [Function: say] }

    const innerFun = () => {
      console.debug(this, "innerFun"); // { say: [Function: say] } innerFun
    };
    innerFun();
  },
};

savage.say();
```
