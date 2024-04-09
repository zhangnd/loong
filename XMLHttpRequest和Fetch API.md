# XMLHttpRequest和Fetch API

XMLHttpRequest 和 Fetch API 都是用于在 web 应用程序中进行网络请求的工具，但它们在实现和使用上有一些不同。

## XMLHttpRequest

XMLHttpRequest 是早期用于在 JavaScript 中进行HTTP请求的API。它允许在不重新加载页面的情况下向服务器发送请求并接收响应。

**特点**：

1. 异步请求：XMLHttpRequest 支持异步请求，可以发送请求并在后台执行其他任务，无需等待服务器响应。

2. 兼容性：由于 XMLHttpRequest 早已存在，因此它在所有现代浏览器中都具有良好的兼容性。

3. 事件驱动：XMLHttpRequest 使用事件处理程序来处理请求的不同阶段，如请求已发送、收到响应等。

**使用示例**：

```js
var xhr = new XMLHttpRequest();
xhr.open('GET', 'https://api.example.com/data', true);
xhr.onreadystatechange = function() {
  if (xhr.readyState === 4 && xhr.status === 200) {
    console.log(xhr.responseText);
  }
};
xhr.send();
```

## Fetch API

Fetch API 是一种现代的、基于 Promise 的替代方案，用于在 web 应用程序中进行网络请求。

**特点**：

1. Promise 接口：Fetch API 返回的是 Promise 对象，使得处理异步请求更加简单和灵活。

2. 更简洁的语法：Fetch API 提供了更简洁、更现代的语法，使得编写和理解代码更加容易。

3. 内置的 Response 对象：Fetch API 使用 Response 对象来表示服务器的响应，它提供了一组方法来处理响应数据。

**使用示例**：

```js
fetch('https://api.example.com/data')
  .then(function(response) {
    return response.json();
  })
  .then(function(data) {
    console.log(data);
  })
  .catch(function(error) {
    console.log('Fetch Error :-S', error);
  });
```

## 区别

1. 语法差异：Fetch API 的语法更加简洁和现代，而 XMLHttpRequest 的语法相对较老，并且较为繁琐。

2. 返回值：XMLHttpRequest 返回的是一个 XMLHttpRequest 对象，而 Fetch API 返回的是一个 Promise 对象，使得处理异步请求更加方便。

3. 兼容性：Fetch API 在一些较老的浏览器上可能不支持，而 XMLHttpRequest 在现代浏览器中具有更好的兼容性。
