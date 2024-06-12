# cookie不完全解析

## Name / Value

字符串，不超过4KB的小型文本数据。

## Domain

不指定则默认是当前源（origin），不包含子域名。

Domain前带点和不带点的区别？

- 带点：父域名和任何子域名都可以访问

- 不带点：只有完全一样的域名才可以访问

像淘宝首页设置的Domain就是.taobao.com，这样无论是a.taobao.com还是b.taobao.com都可以使用cookie。

## Path

Path指定了一个URL路径，这个路径必须出现在要请求的资源的路径中才可以发送cookie首部。比如设置Path=/docs，/docs/web下的资源会带cookie首部，/test则不会携带cookie首部。

Domain和Path标识共同定义了cookie的作用域：即cookie应该发送给哪些URL。

## Expires / Max-Age

缺省时默认是Session，标识的就是会话性cookie，当为会话性cookie的时候，值保存在客户端内存中，在用户关闭浏览器的时候失效。

与会话性cookie相对的是持久性cookie，持久性cookie会保存在硬盘中，直至过期或者清楚cookie。值得注意的是，设定的日期和时间只与客户端相关，而不是服务端。

如果Max-Age属性为正数时，浏览器会将其持久化，即写到对应的cookie文件中。

当Max-Age属性为负数，则表示该cookie只是一个会话性cookie。

当Max-Age为0时，则会立即删除这个cookie。

假如Expires和Max-Age都存在，Max-Age优先级更高。

## Size

实测Chrome浏览器最大4096。

## HttpOnly

设置HttpOnly属性可以防止客户端脚本通过document.cookie等方式访问cookie，有助于避免XSS攻击。

## Secure

标记为Secure的cookie只应通过被HTTPS协议加密过的请求发送给服务端。使用HTTPS安全协议，可以保护cookie在浏览器和Web服务器间的传输过程中不被窃取和篡改。

## SameSite

SameSite属性可以让cookie在跨站请求时不会被发送，从而可以阻止跨站请求伪造攻击（CSRF）。

属性值：

- Strict 仅允许一方请求携带cookie，即浏览器将只发送相同站点请求的cookie，即当前网页URL与请求目标URL完全一致。

- Lax 允许部分第三方请求携带cookie。

- None 无论是否跨站都会发送cookie。

之前默认是None的，Chrome更新到80以上版本后，默认是Lax。
