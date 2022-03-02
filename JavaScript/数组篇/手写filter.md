```js
Array.prototype.filter = function (callbackFn, thisArg) {
  // 处理数组类型异常
  if (this === null || this === undefined) {
    throw new TypeError("Cannot read property 'filter' of null or undefined");
  }
  // 处理回调类型异常
  if (Object.prototype.toString.call(callbackFn) != '[object Function]') {
    throw new TypeError(callbackFn + ' is not a function')
  }

  let O = Object(this);
  let len = O.length >>> 0;
  let resLen = 0;
  let res = [];
  for(let i = 0; i < len; i++){
    if(i in O){
      let element = O[i];
      if(callbackFn.call(thisArg, O[i], i, O)){
        res[resLen++] = element;
      }
    }
  }
  return res;
}
```