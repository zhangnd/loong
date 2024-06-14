# HTML &lt;meta>标签

## &lt;meta charset="utf-8">

charset 属性规定 HTML 文档的字符编码。

charset 属性是 HTML5 中的新属性，且替换了：&lt;meta http-equiv="Content-Type" content="text/html; charset=utf-8">

仍然允许使用 http-equiv 属性来规定字符集，但是使用新方法可以减少代码量。

charset 的值规定 HTML 文档的字符编码。

常用的值：

- UTF-8：Unicode 字符编码

- ISO-8859-1：拉丁字母表的字符编码

在理论上，可以使用任何字符编码，但并不是所有浏览器都能够理解它们。某种字符编码使用的范围越广，浏览器就越有可能理解它。

## &lt;meta http-equiv="x-ua-compatible" content="IE=edge,chrome=1">

在 IE8 刚推出的时候，很多网页由于重构的问题，无法适应较高级的浏览器，所以使用 x-ua-compatible 标签强制 IE8 采用低版本方式渲染。

使用下面这段代码之后，开发者无需考虑网页是否兼容IE8浏览器，只要确保网页在 IE6，IE7 下的表现就可以了。

```html
<meta http-equiv="x-ua-compatible" content="IE=EmulateIE7">
```

下面这段代码使用的是 Edge。Edge 模式告诉 IE 以最高级模式渲染文档，也就是任何 IE 版本都以当前版本所支持的最高级标准模式渲染，避免版本升级造成的影响。简单的说，就是什么版本 IE 就用什么版本的标准模式渲染。

```html
<meta http-equiv="x-ua-compatible" content="IE=edge">
```

ps：为防止失效，x-ua-compatible 最好紧跟在 head 之后，之前不要有任何不标准的标签。

## &lt;meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no, viewport-fit=cover">

`width`: 用来设置布局viewport的特定值，可以设定一个具体的数值，如320、450等。移动端设备的尺寸差异很大，因此一般不设置固定值。width属性有device-width值，可以根据屏幕的宽自动调整，始终与屏幕的宽保持相同，从而达到适配不同设备的目的。

`initial-scale`：用于设置页面的初始缩放比例，一般不会对页面进行缩放处理，因此取值都为1或1.0。

`minimum-scale`：用于设置最小缩放，一般不会让用户进行缩放处理，因此也就不用设置最小或最大缩放，一般取值为1或1.0。

`maximum-scale`：用于设置最大缩放，取值与minimum-scale相同。

`user-scalable`：表示用户能否进行缩放处理，可选值为yes和no。一般情况下，不需要对页面进行缩放处理，因此取值为no。

`viewport-fit`：有contain，cover，auto三个属性值，默认是auto。

- contain：可视窗口完全包含网页内容，页面内容显示在safe area内

- cover：网页内容完全覆盖可视窗口，页面内容充满屏幕

- auto：默认值，跟contain表现一致

## &lt;meta name="referrer" content="strict-origin-when-cross-origin">

`no-referrer`：整个referer首部会被移除，访问来源信息不随着请求一起发送。

`no-referrer-when-downgrade`：在没有指定任何策略的情况下用户代理的默认行为。在同等安全级别的情况下，引用页面的地址会被发送（HTTPS->HTTPS），但是在降级的情况下不会被发送（HTTPS->HTTP）。

`origin`：在任何情况下，只发送文件的源作为引用地址。

`origin-when-cross-origin`：对于同源的请求，会发送完整的URL作为引用地址，但是对于非同源请求仅发送文件的源。

`same-origin`：对于同源的请求会发送引用地址，但是对于非同源请求则不发送引用地址信息。

`strict-origin`：在同等安全级别的情况下，发送文件的源作为引用地址(HTTPS->HTTPS)，但是在降级的情况下不会发送 (HTTPS->HTTP)。

`strict-origin-when-cross-origin`：对于同源的请求，会发送完整的URL作为引用地址；在同等安全级别的情况下，发送文件的源作为引用地址(HTTPS->HTTPS)；在降级的情况下不发送此首部 (HTTPS->HTTP)。

`unsafe-url`：无论是同源请求还是非同源请求，都发送完整的URL（移除参数信息之后）作为引用地址（最不安全）。

浏览器兼容性（[https://caniuse.com/?search=referer-policy](https://caniuse.com/?search=referer-policy)）
