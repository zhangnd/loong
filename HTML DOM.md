# HTML DOM

DOM（Document Object Model）是一种用于表示和操作网页文档结构的编程接口。简单来说，它将网页文档视为一个树形结构，由各种类型的节点组成，每个节点代表文档中的一个元素、属性、文本片段等。通过DOM，开发者可以使用脚本语言来访问、修改、删除和添加HTML元素和内容。

DOM树的根节点是document对象，它代表整个HTML文档。文档中的每个HTML元素都是文档树中的一个节点，这些节点之间通过父子关系、兄弟关系等连接在一起。

例如，HTML代码：

```html
<!DOCTYPE html>
<html>
<head>
  <title>Example</title>
</head>
<body>
  <div id="container">
    <p>Hello, world!</p>
    <ul>
      <li>Item 1</li>
      <li>Item 2</li>
      <li>Item 3</li>
    </ul>
  </div>
</body>
</html>
```

对应的DOM树结构如下：

```markdown
- document
  - html
    - head
      - title
        - "Example"
    - body
      - div#container
        - p
          - "Hello, world!"
        - ul
          - li
            - "Item 1"
          - li
            - "Item 2"
          - li
            - "Item 3"
```

DOM树中的每个节点都是一个对象，这些对象可以通过JavaScript来操作。常见的DOM节点类型包括：

1. 元素节点（Element Node）：代表HTML元素，如&lt;div>、&lt;p>等。

2. 文本节点（Text Node）：代表文本内容，通常是在元素节点内部的文本。

3. 属性节点（Attribute Node）：代表HTML元素的属性，如class、id等。

4. 注释节点（Comment Node）：代表HTML注释。

DOM提供了一系列的API，用于对文档进行操作。一些常见的操作包括：

- 查找元素：通过标签名、类名、ID等方式查找文档中的元素。

- 修改内容：修改元素的文本内容、属性值等。

- 添加或删除元素：动态地添加或删除HTML元素。

- 处理事件：给元素绑定事件处理函数，实现交互功能。

- 修改样式：改变元素的样式属性，实现动态样式效果。

通过DOM，开发者可以实现动态、交互丰富的网页应用，使用户可以与网页进行更深层次的互动。
