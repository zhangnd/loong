# title: script标签的defer和async

&lt;script>标签包含了两个特殊的属性：defer和async。

> async：属性可选。表示应该立即开始下载脚本，但不能阻止其他页面动作，比如下载资源或等待其他脚本加载。只对外部脚本有效。<br>defer：属性可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部脚本文件有效。

`如果没有defer和async属性`，浏览器会立即加载并执行相应的外部脚本，script标签之下的（下面的、下文的、紧接着的、紧跟着的、后续的）文档元素之前，也就是说不等待载入后续的文档元素，`读到脚本就加载并执行`，`这样就阻塞了后续文档的加载`。

## 推迟执行脚本defer

HTML4.0.1为&lt;script>标签定义了一个叫defer的属性。这个属性表示脚本在执行的时候不会改变页面的结构。也就是说，`脚本会被延迟到整个页面都解析完毕后再运行`。因此，在&lt;script>标签上设置defer属性，相当于告诉浏览器立即下载，但延迟执行。

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Document</title>
  <script src="https://cdn.jsdelivr.net/npm/jquery@3.6.4/dist/jquery.min.js" defer></script>
  <script src="https://unpkg.com/jquery@3.6.4/dist/jquery.min.js" defer></script>
</head>
<body>
  <div>hello world</div>
</body>
</html>
```

虽然这个例子中的&lt;script>标签包含在页面的&lt;head>中，但它们会在浏览器解析到结束的&lt;/html>标签后才会执行。

HTML5`规范要求`脚本应该按照它们出现的顺序执行，因此第一个推迟的脚本会在第二个推迟的脚本之前执行，而且两者都会在`DOMContentLoaded`事件之前执行。

不过`在实际当中`，推迟执行的脚本不一定总会按顺序执行或者在DOMContentLoaded事件之前执行，因此最好只包含一个这样的脚本。

如前所述，defer属性只对外部脚本文件才有效。这是HTML5中明确规定的，因此支持HTML5的浏览器会忽略行内脚本的defer属性。

> 综述：<br>1. 无论js文件是否下载完成，只有html解析完毕，才可以执行脚本；<br>2. 脚本执行的顺序与下载的完成时间无关，按照&lt;script>脚本的位置，顺序执行。

## 异步执行脚本async

HTML5为&lt;script>标签定义了async属性。从改变脚本处理方式上看，async属性与defer类似。

当然，`它们两者也都只适用于外部脚本，都会告诉浏览器立即开始下载`。不过，`与defer不同的是，标记为async的脚本并不保证能按照它们出现的次序执行`。

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Document</title>
  <script src="https://cdn.jsdelivr.net/npm/jquery@3.6.4/dist/jquery.min.js" async></script>
  <script src="https://unpkg.com/jquery@3.6.4/dist/jquery.min.js" async></script>
</head>
<body>
  <div>hello world</div>
</body>
</html>
```

在这个例子中，`第二个脚本可能先于第一个脚本执行`。因此，重点在于它们之间没有依赖关系。给脚本添加async属性的目的是告诉浏览器，不必等脚本下载和执行完后再加载页面，同样也不必等到该异步脚本下载和执行后再加载其他脚本。正因为如此，`异步脚本不应该在加载期间修改DOM`。

异步脚本保证会`在页面的load事件前执行，但可能会在DOMContentLoaded之前或之后`。

> 综述：<br>1. 无论html是否解析完成，立即执行脚本；<br>2. 无论有使用多少个async加载脚本，只要脚本下载完成，立即执行脚本。与&lt;script>标签的顺序无关。

## load和DOMContentLoaded事件

> - load<br>MDN的解释：load应该仅用于检测一个完全加载的页面，当一个资源及其依赖资源已完成加载时，将触发load事件。意思是页面的html、css、js、图片等资源都已经加载完之后才会触发load事件。
> - DOMContentLoaded<br>MDN的解释：当初始的HTML文档被完全加载和解析完成之后，DOMContentLoaded事件被触发，而无需等待样式表、图像和子框架的完成加载。

## 中场总结

| script标签 | JS执行顺序	| 是否阻塞解析HTML | DOMContentLoaded回调 |
| ---- | ---- | ---- | ---- |
| <script> | 依次 | 是 | 等待 |
| &lt;script defer> | 先下载完所有defer再依次执行 | 否 | 等待 |
| &lt;script async> | 先下载完先执行 | DOM未解析完时阻塞 | 不等待 |

## 不同属性的时间流程图

![](https://img.zhangniandong.com/2024/20190924222353217.png)

`其中蓝色代表js脚本网络加载时间，红色代表js脚本执行时间，绿色代表html解析。`

![](https://img.zhangniandong.com/2024/94c8a7e393d61367de2ab69e28ed4b64.png)

`紫色线代表网络读取，红色线代表执行时间，这俩都是针对脚本的，绿色线代表HTML解析。`

也就是说async可能乱序执行，而defer是顺序执行，这也就决定了async比较适用于百度分析或者谷歌分析这类不依赖其他脚本的库，且defer在页面加载完成后才执行，可以在脚本中操作DOM。

从图中可以看到一个普通的&lt;script>标签的加载和解析都是同步的，会阻塞DOM的渲染，这也是我们经常会`把<script>写在<body>底部的原因之一`，为了防止加载资源而导致的长时间的白屏，另一个原因是js可能会进行DOM操作，所以要在DOM全部渲染完后再执行。

最稳妥的办法还是把&lt;script>写在&lt;body>底部，没有兼容性问题，没有白屏问题，没有执行顺序问题，高枕无忧。

## 附例

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Document</title>
  <script>
    console.log('aaa', '000')
  </script>
  <script src="./1.js"></script> // console.log('aaa', '111')
  <script src="./2.js" defer></script> // console.log('aaa', '222')
  <script src="./3.js" async></script> // console.log('aaa', '333')
  <script src="./4.js"></script> // console.log('aaa', '444')
  <script>
    console.log('aaa', '555')
    document.addEventListener('DOMContentLoaded', function() {
      console.log('DOMContentLoaded')
    })
  </script>
  <link rel="stylesheet" type="text/css" href="https://emoji-css.afeld.me/emoji.css">
</head>
<body>
  <div>hello world</div>
  <script>
    console.log('aaa', '666')
  </script>
</body>
</html>
```

执行结果：

![](https://img.zhangniandong.com/2024/17032275307195.png)
