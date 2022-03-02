```js
Array.prototype.reduce = function (callbackFn, initValue) {
  // 处理数组类型异常
  if (this === null || this === undefined) {
    throw new TypeError("Cannot read property 'reduce' of null or undefined");
  }
  // 处理回调类型异常
  if (Object.prototype.toString.call(callbackFn) != '[object Function]') {
    throw new TypeError(callbackFn + ' is not a function')
  }

  let O = Object(this);
  let len = O.length >>> 0;
  let k = 0;
  let accumulator = initValue;
  if (accumulator === undefined) {
    for (; k < len; k++) {
      if (k in O) {
        accumulator = O[k];
        k++;
        break;
      }
    }
  }
  // 表示数组全为空
  if (k === len && accumulator === undefined)
    throw new Error('Each element of the array is empty');

  for (; k < len; k++) {
    if (k in O) {
      //全局调用，分别传入累计值、当前值、当前索引和数组
      accumulator = callbackFn.call(undefined, accumulator, O[k], k, O)
    }
  }
  return accumulator;
}
let arr = [1, 2, 3]
let newNum = arr.reduce(function(preNum, curValue){
  return preNum + curValue
})

console.log(newNum)
```