# instanceof

## 原理
`instanceof` 运算符用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。
```js
var obj = {};
obj instanceof Object //true
```

## 实现

```js
function myInstanceof(target, origin) {
    const proto = target.__proto__;
    if (proto) {
        if (origin.prototype === proto) return true;
        else return myInstanceof(proto, origin)
    } else return false
}
```

```js
const myInstanceOf = (target, origin) => {
  let p = target;
  while(p){
    if(p === origin.prototype) return true;
    p = p.__proto__;
  };
  return false;
}
```