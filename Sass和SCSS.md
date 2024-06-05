# Sass和SCSS

## Sass

Sass（Syntactically Awesome Stylesheets）是一个最初由Hampton Catlin设计并由Natalie Weizenbaum开发的层叠样式表语言。Sass是在CSS的基础上进行了扩展，增加了变量、嵌套、混入、选择器、继承等特性。

Sass的语法有两种形式：一种是“缩进语法”，使用缩进来区分代码块，类似于Python或HAML的缩进风格；另一种是较新的“SCSS”语法。

Sass文件通常以`.sass`为扩展名。

Sass语法简洁，通过缩进来表示嵌套和规则之间的关系，省略了大括号和分号。

```css
$primary-color: #007bff
body
  color: $primary-color
```

编译后的CSS：

```css
body {
  color: #007bff;
}
```

## SCSS

SCSS（Sassy CSS）是Sass 3引入的新语法，是Sass的一种语法扩展，同时也是CSS3的超集，意味着所有有效的CSS也是有效的SCSS。

SCSS使用大括号和分号来定义样式规则，类似于常规的CSS语法。它支持嵌套规则、变量、混合、继承等功能。

SCSS文件通常以`.scss`为扩展名。

SCSS的语法更加结构化，更容易被开发者理解和使用，特别是那些已经熟悉CSS语法的开发者。

```css
$primary-color: #007bff;
body {
  color: $primary-color;
}
```

编译后的CSS：

```css
body {
  color: #007bff;
}
```

## 比较

**语法**：Sass的缩进语法使用缩进来表示层级关系，而SCSS使用大括号和分号。SCSS的语法更接近CSS，因此更容易被理解和使用。

**功能**：Sass和SCSS在功能上是相同的，都支持变量、嵌套、混入、选择器等特性。

**文件扩展名**：Sass使用.sass扩展名，而SCSS使用.scss扩展名。

**使用场景**：Sass的缩进语法更加简洁，但可能对于初学者来说不太直观。而SCSS的语法更接近CSS，因此更容易上手。
