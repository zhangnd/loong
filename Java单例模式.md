# Java单例模式

Java中的单例模式是一种常见的设计模式，它确保一个类只有一个实例，并提供一个全局访问点来获取该实例。单例模式在多种场景中都非常有用，比如配置管理、线程池、数据库连接池等。

以下是Java中实现单例模式的几种常见方式：

## 饿汉式

**静态常量**

```java
public class Singleton {
    // 静态初始化器，由JVM来保证线程安全
    private static final Singleton INSTANCE = new Singleton();

    // 私有的构造方法  
    private Singleton() {}

    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

**静态代码块**

```java
public class Singleton {
    // 静态初始化器中的代码块
    private static Singleton INSTANCE;

    static {
        INSTANCE = new Singleton();
    }

    private Singleton() {}

    public static Singleton getInstance() {
        return INSTANCE;
    }
}
```

## 懒汉式

**线程不安全**

```java
public class Singleton {
    private static Singleton INSTANCE;

    private Singleton() {}

    public static Singleton getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }
}
```

注意：这种实现方式在多线程环境下是不安全的。

**线程安全，但效率低**

```java
public class Singleton {
    private static Singleton INSTANCE;

    private Singleton() {}

    public static synchronized Singleton getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Singleton();
        }
        return INSTANCE;
    }
}
```

注意：虽然这种方法是线程安全的，但由于`synchronized`关键字的使用，效率较低。

## 双重检查锁定/双重校验锁（DCL，推荐）

```java
public class Singleton {
    private volatile static Singleton INSTANCE;

    private Singleton() {}

    public static Singleton getInstance() {
        if (INSTANCE == null) {
            synchronized (Singleton.class) {
                if (INSTANCE == null) {
                    INSTANCE = new Singleton();
                }
            }
        }
        return INSTANCE;
    }
}
```

注意：`volatile`关键字确保了在多线程环境下`INSTANCE`变量的可见性，从而保证了双重检查的有效性。

## 静态内部类

```java
public class Singleton {
    private Singleton() {}

    // 静态内部类
    private static class SingletonHolder {
        private static final Singleton INSTANCE = new Singleton();
    }

    public static Singleton getInstance() {
        return SingletonHolder.INSTANCE;
    }
}
```

注意：这种方式利用了classloader的机制来保证初始化`INSTANCE`时只有一个线程。

## 枚举

```java
public enum Singleton {
    INSTANCE;

    // 其他方法...
}
```

注意：枚举是Java中实现单例模式的最佳方式，因为它简洁、自动支持序列化机制，并且绝对防止多次实例化。
