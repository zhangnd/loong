# Java装箱和拆箱

在Java中，装箱（Boxing）和拆箱（Unboxing）是Java 5（也称为Java 1.5）引入的特性，它们允许在基本数据类型（如int, double, char等）和它们对应的包装类（如Integer, Double, Character等）之间进行自动转换。

## 装箱

装箱是将基本数据类型自动转换为对应的包装类对象的过程。这种转换是隐式的，由Java编译器自动完成。以下是一个装箱的示例：

```java
int a = 10;
Integer b = a; // 隐式装箱
```

在上面的代码中，尽管a是一个基本数据类型int，但在赋值给Integer类型的变量b时，Java编译器会自动将a装箱为一个Integer对象。

## 拆箱

拆箱是与装箱相反的过程，即将包装类对象自动转换为对应的基本数据类型。同样，这种转换也是隐式的，由Java编译器自动完成。以下是一个拆箱的示例：

```
Integer a = 10;  
int b = a; // 隐式拆箱
```

在上面的代码中，尽管a是一个Integer对象，但在赋值给基本数据类型int的变量b时，Java编译器会自动将a拆箱为一个int值。
