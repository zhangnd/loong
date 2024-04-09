# AJAX

**AJAX 并不是编程语言。**

**AJAX 是一种从网页访问 Web 服务器的技术。**

**AJAX 代表异步 JavaScript 和 XML。**

## 什么是 AJAX？

AJAX = **A**synchronous **J**avaScript **A**nd **X**ML

AJAX 并非编程语言。

AJAX 仅仅组合了：

- 浏览器内建的 XMLHttpRequest 对象（从 web 服务器请求数据）

- JavaScript 和 HTML DOM（显示或使用数据）

Ajax 是一个令人误导的名称。Ajax 应用程序可能使用 XML 来传输数据，但将数据作为纯文本或 JSON 文本传输也同样常见。

Ajax 允许通过与场景后面的 Web 服务器交换数据来异步更新网页。这意味着可以更新网页的部分，而不需要重新加载整个页面。

## AJAX 如何工作

1. 网页中发生一个事件（页面加载、按钮点击）

2. 由 JavaScript 创建 XMLHttpRequest 对象

3. XMLHttpRequest 对象向 web 服务器发送请求

4. 服务器处理该请求

5. 服务器将响应发送回网页

6. 由 JavaScript 读取响应
7. 由 JavaScript 执行正确的动作（比如更新页面）
