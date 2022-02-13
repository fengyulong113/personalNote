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
这个方法返回一个新的`promise`对象，该`promise`对象在`iterable`参数对象里所有的`promise`对象都成功的时候才会触发成功，一旦有任何一个`iterable`里面的`promise`对象失败则立即触发该`promise`对象的失败。这个新的`promise`对象在触发成功状态以后，会把一个包含`iterable`里所有`promise`返回值的数组作为成功回调的返回值，顺序跟`iterable`的顺序保持一致；如果这个新的`promise`对象触发了失败状态，它会把`iterable`里第一个触发失败的`promise`对象的错误信息作为它的失败错误信息。`Promise.all`方法常被用于处理多个promise对象的状态集合。
```js
var p1 = new Promise((resolve, reject) => {
  resolve("success1");
})

var p2 = new Promise((resolve, reject) => {
  resolve("success2");
})

var p3 = new Promise((resolve, reject) => {
  reject("fail");
})

Promise.all([p1, p2]).then((result) => {
  console.log(result);      // 成功状态 //["success1", "success2"]
}).catch((error) => {
  console.log(error);
})

Promise.all([p1,p3,p2]).then((result) => {
  console.log(result);
}).catch((error) => {
  console.log(error);      // 失败状态 // fail
})
```
### Promise.race(iterable)
当`iterable`参数里的任意一个子`promise`被成功或失败后，父`promise`马上也会用子`promise`的成功返回值或失败详情作为参数调用父`promise`绑定的相应句柄，并返回该`promise`对象。
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
### 实现Ajax
```js
function ajax(method, url, data) {
    var request = new XMLHttpRequest();
    return new Promise(function (resolve, reject) {
        request.onreadystatechange = function () {
            if (request.readyState === 4) {
                if (request.status === 200) {
                    resolve(request.responseText);
                } else {
                    reject(request.status);
                }
            }
        };
        request.open(method, url);
        request.send(data);
    });
}
ajax("GET","https://www.baidu.com",[]).then((res) => {
    console.log(res);
}).catch((err) => {
    console.log(err);
})
```

## 手动实现Promise