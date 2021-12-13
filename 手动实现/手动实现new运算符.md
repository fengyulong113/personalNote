# new运算符
`new`运算符用于生成一个构造函数的实例
```js
function Person(name, age){
    this.name = name;
    this.age = age;
}

var p = new Person('xxx', 18);
console.log(p) //Person {name: 'xxx', age: 18}
console.log(p.__proto__) //constructor: ƒ Person(name, age)
console.log(Person.prototype) //constructor: ƒ Person(name, age)
```
可以看出，`new`出来的实例对象的`__proto__`和构造函数`Person`的`prototype`相等。所以`new`操作符创建对象的过程：
1. 创建一个空的对象。
2. 将创建的对象的`__proto__`属性值设为该构造函数的`prototype`属性值
3. 执行构造函数中的代码，构造函数中的`this`指向该对象。
4. 返回对象

# 手动实现new

```js
function _new(constructor, ...argument){
    //创建一个空的对象
    const obj = {}；
    //将该对象的__proto__属性值设为构造函数的prototype属性值
    obj.__proto__ = constructor.prototype;
    //绑定this
    const res = constructor.apply(obj, argument);
    //确保new出来的是一个对象
    return typeof res === 'object'? res : obj;
}
```