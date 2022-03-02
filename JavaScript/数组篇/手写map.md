```js



Array.prototype.map = function (callbackFn, thisArg) {
  // 处理数组类型异常
  if (this === null || this === undefined) {
    throw new TypeError("Cannot read property 'map' of null or undefined");
  }

  // 处理回调类型异常
  if (Object.prototype.toString.call(callbackFn) != '[object Function]') {
    throw new TypeError(callbackFn + 'is not a function')
  }

  let O = Object(this); // 转为对象
  let T = thisArg;

  let len = O.length >>> 0; //保证其为整数
  let A = new Array(len);
  for (let k = 0; k < len; k++) {
    if (k in O) {
      let kValue = O[k];
      let mapValue = callbackFn.call(T, kValue, k, O);
      A[k] = mapValue
    }
  }
  return A;
}

let arr = [1, 2, 3];
let obj = {
  value: 1
}
let newArr = arr.map(function(item){
  return item + 1 + this.value
},obj)

console.log(newArr)
```