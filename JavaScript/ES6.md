# ES6特性
## let、const
ES6新增两个重要的关键字：let 和 const，它们两个声明的变量只能在块级作用中使用，同一个代码块中不可重复声明相同的变量，也不可以重复声明函数内的参数，而且不会出现变量提升。const声明常量，且不可修改。

let、const和var的区别：

- let和const只能声明一次，var可以声明多次。
- var声明的变量会提升，let和const并不会表现提升。
- const声明一个常量，一旦声明，则不能改变。而引用类型`object`，`array`，`function`等，变量指向的内存地址其实是保存了一个指向实际数据的指针，所以const只能保证指针是固定的，至于指针指向的数据结构变不变就无法控制了。
- let和const声明的变量只在其声明所在的代码块内有效，形成块级作用域。

## 解构赋值
按照一定的模式，从数组和对象中提取值，对变量进行赋值。
```js
let [a, b, c] = [1, 2, 3];
let [a, [[b], c]] = [1, [[2], 3]];
let { a, b } = { a:"aaa", b:"bbb" }
```
## Symbol
ES6新增一种数据类型`Symbol`，表示独一无二的值。
```js
let s1 = Symbol("s");
let s2 = Symbol("s");
console.log(s1 === s2); //false
```
## 模板字面量
在字符串中插入一个变量
```js
let num = 18;
let str = `小明今年${num}岁`;
console.log(str); //小明今年18岁
```

## 拓展运算符
拓展运算符(spread)是`...`,主要的作用是将数组或对象展开。
```js
let arr = [1, 2, 3];
function f(s1, s2, s3){
    console.log("三个参数分别为：", s1, s2, s3);
}
f(...arr); //三个参数分别为： 1 2 3
```
## 箭头函数
箭头函数是函数的一种简写形式，继承当前上下文的`this`关键字。
```js
let add = (a, b) => a + b;
let show = a => console.log(a);
let test = (a, b, c) => { console.log(a,b,c); return a + b + c};
add(1,2); //3
show(1); //1
test(1, 1, 1); // 1 1 1
```
箭头函数与普通函数的区别：

**没有单独的this**
箭头函数没有自己的`this`,在箭头函数中调用`this`时，会取其上下文环境中的`this`，它只会从作用域链的上层继承`this`。

**不能用作构造函数**
构造函数是通过`new`来生成对象实例，生成对象实例的过程就是通过构造函数给实例绑定`this`的过程。而箭头函数没有自己的`this`，所以不能用作构造函数。

## 类
ES6提供了更接近传统语言的写法，引入了`class`这个概念，作为对象的模板。通过`class`关键字，可以定义类，与多数传统语言类似。不过，ES6的`class`不是新的对象继承模型，它只是原型链的语法糖表现形式。
```js
class Me {
    constructor(){
        console.log("constructor");
    }
    study(){
        console.log("study");
    }
}
console.log(typeof Me); //function
me.study(); //study
```

## Promise
`Promise`是异步编程的一种解决方案。从语法上来说，`Promise`是一个对象，从它可以获取异步操作的消息。
`Promise`异步操作有三种状态：`pending`、`fulfilled`和`rejected`。除了异步操作的结果，任何其他操作都无法改变这个状态。
`then`方法接受两个函数作为参数，第一个参数是`Promise`执行成功的回调，第二个参数是`Promise`执行失败时的回调，两个参数只能有一个被调用。