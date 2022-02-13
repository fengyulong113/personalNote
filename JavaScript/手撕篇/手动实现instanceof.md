# instanceof

## 原理
`instanceof` 运算符用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。
```js
var obj = {};
obj instanceof Object //true
```

## 实现

```js
function muInstanceof(target, origin) {
    const proto = target.__proto__;
    if (proto) {
        if (origin.prototype === proto) return true;
        else return muInstanceof(proto, origin)
    } else return false
}
```