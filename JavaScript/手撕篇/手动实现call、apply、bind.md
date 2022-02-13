# call
`call()`方法在使用一个指定的`this`值和若干参数的前提下调用某个函数或方法。
```js
var foo = {
    value: 1
}

function bar(){
    console.log(this.value);
}

bar.call(foo) // 1
```
其具体实现的步骤：
1. 改变了`this`的指向，指向`foo`;
2. `bar`函数执行，相当于在`foo`中添加了一属性
   ```js
    var foo = {
        value: 1,
        bar: function(){
            console.log(this.value);
        }
    }
    foo.bar(); //1
    ```
3. 再删除这个属性`bar`

## 实现call
```js
Function.prototype._call = function(context){
    //用this获取调用_call函数的函数，即bar
    context.fn = this;
    //在context上执行这个函数
    context.fn();
    //删除这个属性
    delete context.fn；
}
```
但是`call()`方法可以传入若干参数，并且当第一个参数为`null`时，默认指向`window`,并且`call()`方法有返回值，使用调用者提供的`this`值和参数调用该函数的返回值。若该方法没有返回值，则返回`undefined`。
``` js
var foo = {
    value: 1;
}

var value = 2;

function bar(name, age){
    console.log(name);
    console.log(age);
    console.log(this.value);
}

bar.call(foo, 'xxx', 10) // 'xxx' 10 1
bar.call(null, 'yyy', 20) // 'yyy' 20 2
```
修改一下我们的`_call`方法
```js
Function.prototype._call = function(context = window, ...arg){
    if(this === Function.prototype){
        return undefined; //防止Function.prototype._call()直接调用
    }
    context = context || window;
    context.fn = this;
    const result = context.fn(...arg);
    delete context.fn;
    return result;
}
```
# apply
`apply`的实现跟`call`类似，只是后面传的参数是一个数组或者类数组对象。
```js
Function.prototype._apply = function (context = window, args) {
    if (this === Function.prototype) {
        return undefined; //防止Function.prototype._apply()直接调用
    }
    context = context || window;
    context.fn = this;
    let result;
    if(Array.isArray(args)){
        result = context.fn(...args);
    }else{
        result = context.fn()
    }
    delete context.fn;
    return result;
}
```

# bind
`bind()`方法会返回一个新的函数。当这个新的函数被调用时，`bind()`的第一个参数将会作为它运行时的`this`，之后的一系列参数将会在传递的实参前传入作为它的参数。
