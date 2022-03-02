显示绑定：`call`、`apply`、`bind`

隐式绑定：

1. 全局上下文：默认this指向window，严格模式下指向undefined
2. 直接调用
3. 对象.方法：this指向这个对象
4. DOM事件绑定：onClick和addEventerListener中this默认指向绑定事件的元素
5. new + 构造函数：this指向实例对象
6. 箭头函数：this指向最近的非箭头函数的this，没有找到就是window，严格模式为undefined