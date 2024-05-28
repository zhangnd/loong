# JavaScript数组方法

## 改变原数组

**push()**：向数组末尾添加一个或多个元素，返回新数组的长度。

```js
let arr = [10, 20, 30];
let res = arr.push(40);
console.log(arr); // [10, 20, 30, 40]
console.log(res); // 4
```

**pop()**：移除并返回数组的最后一个元素。

```js
let arr = [10, 20, 30];
let res = arr.pop();
console.log(arr); // [10, 20]
console.log(res); // 30
```

**unshift()**：在数组开头添加一个或多个元素，返回新数组的长度。

```js
let arr = [20, 30];
let res = arr.unshift(10);
console.log(arr); // [10, 20, 30]
console.log(res); // 3
```

**shift()**：移除并返回数组的第一个元素。

```js
let arr = [10, 20, 30];
let res = arr.shift();
console.log(arr); // [20, 30]
console.log(res); // 10
```

**reverse()**：翻转数组，返回翻转好的数组。

```js
let arr = [10, 20, 30];
let res = arr.reverse();
console.log(arr); // [30, 20, 10]
console.log(res); // [30, 20, 10]
```

**splice()**：截取数组，返回截取出来的数组。

```js
let arr = [10, 20, 30, 40, 50, 60, 70];
let res = arr.splice(1, 3);
console.log(arr); // [10, 50, 60, 70]
console.log(res); // [20, 30, 40]
```

## 不改变原数组

**concat()**：合并数组，返回新数组。

```js
let arr = [10, 30, 50];
let res = arr.concat([20, 40, 60]);
console.log(res); // [10, 30, 50, 20, 40, 60]
```

**slice()**：

```js
let arr = [10, 20, 30, 40, 50];
let res = arr.slice(1, 3);
console.log(res); // [20, 30]
```

**join()**：

```js
let arr = [10, 20, 30];
let res = arr.join(',');
console.log(res); // 10,20,30
```

## ES6新增的方法

**forEach()**：循环遍历数组。

```js
```

**map()**：映射数组。

```js
let arr = [10, 20, 30];
let res = arr.map((item, index) => {
  return item * 10;
});
console.log(res); // [100, 200, 300]
```

**filter()**：过滤数组。

```js
let arr = [10, 20, 30];
let res = arr.filter((item, index) => {
  return item > 10;
});
console.log(res); // [20, 30]
```

**find()**：

```js
```

**some()**：

```js
```

**every()**：

**reduce()**：
