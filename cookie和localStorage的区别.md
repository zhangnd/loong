# cookie和localStorage的区别

`cookie` 和 `localStorage` 都是在客户端存储数据的机制，但它们有一些重要的区别：

## 1. 存储容量：

- **cookie**：每个 cookie 的存储容量通常受到限制，一般在 4KB 到 5KB 之间，不同浏览器可能有所不同。由于每个 HTTP 请求都会携带 cookie，过多的 cookie 数据可能会增加网络传输成本。

- **localStorage**：localStorage 的存储容量通常比 cookie 更大，大约在 5MB 到 10MB 之间（不同浏览器可能有所不同）。因此，localStorage 更适合存储较大量的数据。

## 2. 有效期：

- **cookie**：可以设置 cookie 的过期时间。如果不设置过期时间，那么 cookie 将在浏览器关闭后被删除（会话 cookie），否则将在指定时间过期。此外，可以设置 cookie 的 `Max-Age` 属性来指定 cookie 的有效时长。

- **localStorage**：存储在 localStorage 中的数据不会自动过期，除非手动删除或者由于用户清除浏览器缓存而被清除。

## 3. 数据发送：

- **cookie**：cookie 会随着每个 HTTP 请求被发送到服务器。

- **localStorage**：localStorage 仅在浏览器端存储数据，不会随每个 HTTP 请求发送到服务器。

## 4. 使用方式：

- **cookie**：通过 `document.cookie` 属性来读取和设置 cookie 的值。

- **localStorage**：通过 `localStorage.setItem()`、`localStorage.getItem()` 等方法来读取和设置 localStorage 的值。

## 5. 安全性：

- **cookie**：由于每次 HTTP 请求都会携带 cookie，存在被窃取的风险，因此敏感信息应该避免存储在 cookie 中，并且应该对 cookie 的值进行加密处理。

- **localStorage**：localStorage 的数据存储在浏览器中，不会随着 HTTP 请求发送到服务器，因此相对来说更安全一些。但仍然需要注意，敏感信息应该进行加密处理，以防止被恶意脚本获取。

综上所述，`cookie` 和 `localStorage` 都是客户端存储数据的机制，但它们在存储容量、有效期、数据发送、API 和安全性等方面有所不同，因此在具体应用场景中需要根据需求来选择合适的存储方式。
