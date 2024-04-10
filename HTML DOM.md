# HTML DOM

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
