# JavaScript数组方法

**push()**: 向数组末尾添加一个或多个元素，并返回新数组的长度。

```js
let arr = [1, 2, 3];
arr.push(4);
// 现在arr为 [1, 2, 3, 4]
```

**pop()**: 移除并返回数组的最后一个元素。

```js
let arr = [1, 2, 3];
let lastElement = arr.pop();
// 现在arr为 [1, 2]，lastElement为3
```

**shift()**: 移除并返回数组的第一个元素。

```js
let arr = [1, 2, 3];
let firstElement = arr.shift();
// 现在arr为 [2, 3]，firstElement为1
```

**unshift()**: 在数组的开头添加一个或多个元素，并返回新数组的长度。

```js
let arr = [2, 3];
arr.unshift(1);
// 现在arr为 [1, 2, 3]
```

**concat()**: 合并两个或多个数组，并返回新数组。

```js
let arr1 = [1, 2];
let arr2 = [3, 4];
let newArr = arr1.concat(arr2);
// newArr为 [1, 2, 3, 4]
```

**slice()**: 返回数组的一部分，浅拷贝到一个新数组中，不改变原数组。

```js
let arr = [1, 2, 3, 4, 5];
let newArr = arr.slice(2, 4);
// newArr为 [3, 4]，arr不变为 [1, 2, 3, 4, 5]
```

**splice()**: 修改数组，删除或替换现有元素或者原地添加新的元素。

```js
let arr = [1, 2, 3, 4, 5];
arr.splice(2, 1); // 从索引2开始删除1个元素
// 现在arr为 [1, 2, 4, 5]
```

**forEach()**: 对数组中的每个元素执行提供的函数。

```js
let arr = [1, 2, 3];
arr.forEach(function(element) {
    console.log(element);
});
// 输出：
// 1
// 2
// 3
```
