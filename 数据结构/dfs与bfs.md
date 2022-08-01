## 树结构
```js
const tree = {
  val: 'a',
  children: [
    {
      val: 'b',
      children: [
        {
          val: 'd',
          children: []
        },
        {
          val: 'e',
          children: []
        }
      ]
    },
    {
      val: 'c',
      children: [
        {
          val: 'f',
          children: []
        },
        {
          val: 'g',
          children: []
        }
      ]
    }
  ]
};
```

## 深度优先遍历
```js
/**
 * 深度优先遍历
 * 1.访问根节点
 * 2.对根节点的children挨个进行深度优先遍历
 */
const dfc = (root) => {
  console.log(root.val);
  root.children.forEach(dfc)
}
```

## 广度优先遍历
```js
/**
 * 广度优先遍历
 * 1. 新建一个队列，把根节点入队
 * 2. 把队头出队并访问
 * 3. 把队头的children挨个入队
 * 4. 重复2、3，直到队列为空
 */
const bfc = (root) => {
  const q = [root];
  while(q.length > 0){
    const n = q.shift();
    console.log(n.val);
    n.children.forEach(child => {
      q.push(child)
    })
  }
};
```