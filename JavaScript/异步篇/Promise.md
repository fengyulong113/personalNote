# Promise
`Promise`是异步操作的一个解决方案，用于表示异步操作的最终的完成或失败，极其结果值。
## 语法
```js
new Promise((resolve, reject) => { /* executor */
    //执行异步操作代码
})
```
`executor`是带有`resolve`和`reject`两个参数的函数。`Promise`构造函数执行时立即调用`execitor`函数。`resolve`和`reject`执行时，分别将`promise`的状态从`pending`改为`fulfilled`和`rejected`。
## 状态

    pending: 初始状态，既不表示成功也不表示失败
    fulfilled: 意味着操作成功
    rejected: 意味着操作失败

`Promise`只有从`pending`改为`fulfilled`和从`pending`改为`rejected`，一旦状态改变，就无法再改变其状态。
## 示例
`Promise`可以进行链式调用，避免过多的异步操作造成的回调地狱。`then()`函数会返回一个和原来不同的新的`Promise`。

```js
var promise = new Promise((resolve, reject) => {
    var rand = Math.random() * 2;
    setTimeout(() => {
        if (rand < 1) resolve(rand);
        else reject(rand);
    }, 1000)
});
promise.then((rand) => {
    console.log("resolve", rand);
}).catch((rand) => {
    console.log("rejcet", rand);
})
```

## 方法

### Promise.all(iterable)
`Promise.all`方法用于将多个**Promise**实例，包装成一个新的**Promise**实例。
```js
const p = Promise.all([p1, p2, p3]);
```
`Promise.all`接收一个数组作为参数，`p1`，`p2`，`p3`都是**Promise**的实例。

`p`的状态由`p1`，`p2`，`p3`决定，分为两种情况：

1. 只有`p1`、`p2`、`p3`的状态都为`fulfilled`，`p`的状态才为`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。
2. 只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`rejected`的实例的返回值，会传递给`p`的回调函数。


### Promise.race(iterable)
`Promise.race()`方法同样是将多个 **Promise** 实例，包装成一个新的 **Promise** 实例。
```js
const p = Promise.race([p1, p2, p3]);
```
上面代码中，只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 **Promise** 实例的返回值，就传递给`p`的回调函数。

栗子
```js
var p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("success");
  },1000);
})

var p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject("failed");
  }, 2000);
})

Promise.race([p1, p2]).then((result) => {
  console.log(result); // p1先获得结果，那么就执行p1的回调
}).catch((error) => {
  console.log(error);
})
```
### 原型方法
#### Promise.prototype.then(onFulfilled, onRejected)
添加解决`fulfillment`和拒绝`rejection`回调到当前`promise`,返回一新的`promise`,将以回调的返回值来`resolve`。

#### Promise.prototype.catch(onRejected)
添加一个拒绝`rejection`回调到当前`promise`,返回一个新的`promise`当这个回调函数被调用，新`promise`将以它的返回值来`resolve`，否则果当前`promise`进入`fulfilled`状态，则以当前`promise`的完成结果作新`promise`的完成结果。

#### Promise.prototype.finally(onFinally)
添加一个事件处理回调于当前`promise`对象，并且在原`promise`对象解完毕  后，返回一个新的`promise`对象。回调会在当前`promise`运行毕后被调用，无论当前`promise`的状态是完成`fulfilled`还是失`rejected`。


## 实现Promise.all
```js
Promise.all = function (promises) {
  return new Promise((resolve, reject) => {
    // 参数可以不是数组，但必须具有 Iterator 接口
    if (typeof promises[Symbol.iterator] !== "function") {
      reject("Type error");
    }
    if (promises.length === 0) {
      resolve([]);
    } else {
      const res = [];
      let count = 0;
      const len = promises.length;
      for (let i = 0; i < len; i++) {
        //考虑到 promises[i] 可能是 thenable 对象也可能是普通值
        Promise.resolve(promises[i])
          .then((data) => {
            res[i] = data;
            if (++count === len) {
              resolve(res);
            }
          })
          .catch((err) => {
            reject(err);
          });
      }
    }
  });
};
```

## 总结

**Promise**是异步编程的的一种解决方案，相比于传统的回调函数它显得更合理，更强大。
`promise`的出现解决了回调地狱。在没有`promise`之前，每种任务都有两种结果，成功或失败。需要在每种任务执行结束后分别处理这两种结果，如果回调函数的两种结果中再嵌套回调函数，就会变得难以维护。
`promise`很好的解决了这种问题，它用了三种技术手段：

1. 回调函数的延迟绑定
   `promise`将回调函数放在`then()`方法中，而不是直接声明回调函数。
2. 返回值穿透
    `promise`通过`resolve`和`reject`将需要处理的值穿透出去，作为回调函数的参数。在`then()`方法中根据回调函数传入的值创建不同类型的`promise`，然后将放回的`promise`穿透出去，链式调用`then()`方法。
3. 错误冒泡
   当链式调用时，前面产生的错误会一直向后传递，被`catch`接收到，就不用频繁的检查错误了。

`promise`有三种状态，分别是`pending`、`fulfilled`和`rejected`，只能从`pending`转变为`fulfilled`或`rejected`，且状态一旦改变就无法再重新改变。
`promise`构造函数接收一个函数作为参数，该函数有两个参数分别是`resolve`和`reject`。它们两个也是函数。

`resolve`的作用是将`promise`的状态从`pending`变为`fulfilled`，
`reject`的作用是将`promise`的状态从`pending`变为`rejected`。

`then`中有两个回调函数，分别为`resolved`状态的回调函数和`rejected`状态的回调函数。处理失败的一般用`catch`方法，相当于`then`方法的第一个参数为`null`。
