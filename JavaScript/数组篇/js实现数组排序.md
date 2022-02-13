# JS实现数组排序

## 快速排序

```js
function quickSort(arr){
    if(arr.length <= 1) return arr;
    let valueIndex = Math.floor(arr.length / 2);
    let value = arr.splice(valueIndex, 1)[0]
    let left = [], right = [];
    for(let i= 0; i<arr.length; i++){
        if(arr[i] < value) left.push(arr[i])
        else right.push(arr[i])
    }
    return quickSort(left).concat([value], quickSort(right))
}
```

## 插入排序

```js
function insertSort(arr){
    let n = arr.length;
    for(let i = 1; i < n; i++){
        let key = arr[i]
        let j = i - 1;
        while( j >=0 && arr[j] > key){
            arr[j + 1] = arr[j];
            j--;
        }
        arr[j + 1] = key
    }
    return arr;
}
```

## 希尔排序

```js
function shellSort(arr){
    var n = arr.length;
    for(let gap=n/2; gap>0; gap=Math.floor(gap/2)){
        for(let i=gap; i<n; ++i){
            for(let k=i-gap; k>=0 && arr[k]>arr[k+gap]; k=k-gap){
                [arr[k], arr[k+gap]] = [arr[k+gap], arr[k]];
            }
        }
    }
}
```
## 堆排序

```js
// 交换两个节点
function swap(A, i, j) {
  let temp = A[i];
  A[i] = A[j];
  A[j] = temp; 
}

// 将 i 结点以下的堆整理为大顶堆，注意这一步实现的基础实际上是：
// 假设 结点 i 以下的子堆已经是一个大顶堆，shiftDown函数实现的
// 功能是实际上是：找到 结点 i 在包括结点 i 的堆中的正确位置。后面
// 将写一个 for 循环，从第一个非叶子结点开始，对每一个非叶子结点
// 都执行 shiftDown操作，所以就满足了结点 i 以下的子堆已经是一大
//顶堆
function shiftDown(A, i, length) {
  let temp = A[i]; // 当前父节点
// j<length 的目的是对结点 i 以下的结点全部做顺序调整
  for(let j = 2*i+1; j<length; j = 2*j+1) {
    temp = A[i];  // 将 A[i] 取出，整个过程相当于找到 A[i] 应处于的位置
    if(j+1 < length && A[j] < A[j+1]) { 
      j++;   // 找到两个孩子中较大的一个，再与父节点比较
    }
    if(temp < A[j]) {
      swap(A, i, j) // 如果父节点小于子节点:交换；否则跳出
      i = j;  // 交换后，temp 的下标变为 j
    } else {
      break;
    }
  }
}

// 堆排序
function heapSort(A) {
  // 初始化大顶堆，从第一个非叶子结点开始
  for(let i = Math.floor(A.length/2-1); i>=0; i--) {
    shiftDown(A, i, A.length);
  }
  // 排序，每一次for循环找出一个当前最大值，数组长度减一
  for(let i = Math.floor(A.length-1); i>0; i--) {
    swap(A, 0, i); // 根节点与最后一个节点交换
    shiftDown(A, 0, i); // 从根节点开始调整，并且最后一个结点已经为当
                         // 前最大值，不需要再参与比较，所以第三个参数
                         // 为 i，即比较到最后一个结点前一个即可
  }
}

let Arr = [4, 6, 8, 5, 9, 1, 2, 5, 3, 2];
heapSort(Arr);
alert(Arr);
```

## 归并排序

```js
function mergeSort(arr) {  //采用自上而下的递归方法
    var len = arr.length;
    if(len < 2) {
        return arr;
    }
    var middle = Math.floor(len / 2),
        left = arr.slice(0, middle),
        right = arr.slice(middle);
    return merge(mergeSort(left), mergeSort(right));
}

function merge(left, right)
{
    var result = [];
    while (left.length && right.length) {
        if (left[0] <= right[0]) {
            result.push(left.shift());
        } else {
            result.push(right.shift());
        }
    }

    while (left.length)
        result.push(left.shift());

    while (right.length)
        result.push(right.shift());
    return result;
}
var arr=[3,44,38,5,47,15,36,26,27,2,46,4,19,50,48];
console.log(mergeSort(arr));
```
