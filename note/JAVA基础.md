# JAVA基础

###### 说明：

本文参考了[CyC2018](https://github.com/CyC2018/CS-Notes)里面的一些内容

## String

String 被声明为 final，因此它不可被继承。(Integer 等包装类也不能被继承）

在 Java 8 中，String 内部使用 char 数组存储数据。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];
}
```

在 Java 9 之后，String 类的实现改用 byte 数组存储字符串，同时使用 `coder` 来标识使用了哪种编码。

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final byte[] value;

    /** The identifier of the encoding used to encode the bytes in {@code value}. */
    private final byte coder;
}
```



|               | 可变 | 线程安全                             |
| ------------- | ---- | ------------------------------------ |
| String        | 否   | 否                                   |
| StringBuilder | 是   | 否                                   |
| StringBuffer  | 是   | 是（内部使用 synchronized 进行同步） |



## Object通用方法

Objectt通用方法大部分是native方法，native方法是在JVM源码层实现的，效率一般来说远高于Java中的非native方法。

| 方法                                                         | 功能                                                         |
| :----------------------------------------------------------- | ------------------------------------------------------------ |
| `public native int hashCode()`                               | 返回哈希值                                                   |
| `public boolean equals(Object obj)`                          | 判断两个对象是否相等                                         |
| `protected native Object clone() throws CloneNotSupportedException` | 返回一个对象的拷贝（分为浅拷贝和深拷贝），一个类必须显式重写`clone()`方法，才能够被调用。使用clone()拷贝一个对象既复杂又有风险，有可能会抛出异常或者需要类型转换，因此最好使用拷贝构造函数或者拷贝工厂来拷贝对象 |
| `public String toString()`                                   | 返回字符串表示的对象值（默认返回TestClass@74a14482这种形式） |
| `public final native Class<?> getClass()`                    | 获取对象运行时class对象（描述对象所属类的对象）              |
| `protected void finalize() throws Throwable {}`              | 在GC准备释放对象所占用的内存空间之前，它将首先调用finalize()方法。由于GC的自动回收机制，因而并不能保证finalize方法会被及时地执行，也不能保证它们会被执行。 |
| `public final native void notify()`                          | 唤醒在此对象监视器上等待的单个线程                           |
| `public final native void notifyAll()`                       | 唤醒在此对象监视器上等待的所有线程                           |
| `public final native void wait(long timeout) throws InterruptedException` | 当前线程T等待并释放对象锁                                    |
| `public final void wait(long timeout, int nanos) throws InterruptedException` | -                                                            |
| `public final void wait() throws InterruptedException`       | -                                                            |





## 抽象类与接口

### 重写与重载

1. 重写（Override）

    ​	存在于继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法。

2. 重载（Overload）