#  JAVA基础

###### 说明：

本文参考了[CyC2018](https://github.com/CyC2018/CS-Notes)里面的内容

## 1.Java SE、Java EE、Java ME区别

[参考链接](https://www.cnblogs.com/wzk-0000/p/8016202.html)



|         | 全称（简称）                            | 主要用途           |
| ------- | --------------------------------------- | ------------------ |
| Java SE | Java Platform，Standard Edition（J2SE） | 做电脑上运行的软件 |
| Java EE | Java Platform，Enterprise Edition(J2EE) | 做网站             |
| Java ME | Java Platform，Micro Edition(J2ME)      | 做手机软件         |



## 2.String

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



## 3.Object通用方法

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





## 4.抽象类与接口

### 4.1重写与重载

1. 重写（Override）

    ​	存在于继承体系中，指子类实现了一个与父类在方法声明上完全相同的一个方法。

2. 重载（Overload）

## 5.创建线程（池）的几种主要方式

### 5.1创建线程的三种主要方式

#### 5.1.1new Thread

```java
public void fun1() {
    Thread thread = new Thread() {
        @Override
        public void run() {
            super.run();
            //Code
            System.out.println("creat new thread fun1");
        }
    };
}
```

#### 5.1.2实现Runnable接口

```java
public void fun2() {
    Thread thread = new Thread(new Runnable() {
        @Override
        public void run() {
            System.out.println("create new thread fun2");
        }
    });
    thread.run();
}

//Thread implements Runnable 
public class ThreadTest extends Thread{
    @Override
    public void run() {    
    }
}
```

#### 5.1.3实现Callable接口

```java
public void fun3() {
  	/*
    	Runnable的延伸
    	可以返回结果值，需要在定义的时候指定类型
    */
    
    Callable<Integer> callable = new Callable<Integer>() {
        @Override
        public Integer call() throws Exception {
            return 100;
        }
    };

    FutureTask<Integer> task = new FutureTask<>(callable);
    new Thread(task).start();
    while (task.isDone()) {
        try{
            System.out.println("create new thread fun3 " + task.get().toString());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```

### 5.2创建线程池的7种主要方式

**思路：**提前创建好多个线程，放入线程池中，使用时直接获取，使用完放回池中。可以避免频繁创建销毁、实现重复利用（类似的还有数据库的连接池）。

**优点：**提高响应速度、降低资源消耗、便于线程管理

**相关API：**ExecutorService和Executors

| 创建方式                                 |                                                              |
| ---------------------------------------- | ------------------------------------------------------------ |
| newSingleThreadExecutor()                | 创建只有一个线程的线程池                                     |
| newCachedThreadPool()                    | 可根据需要创建新线程的线程池，常用来处理大量短时间工作任务的线程池 |
| newFixedThreadPool(int nThreads)         | 创建一个可重用固定线程数的线程池                             |
| newSingleThreadScheduledExecutor()       | 创建单线程池，可以定期或周期工作                             |
| newScheduledThreadPool(int corePoolSize) | 与newSingleThreadScheduledExecutor()类似，区别在于是单个线程还是多个线程 |
| newWorkStealingPool(int parallelism)     | JDK1.8加入，利用Work-Stealing算法，不保证处理顺序            |
| ThreadPoolExecutor()                     | 最原始的线程池创建                                           |

| 线程池状态 |                                                              |
| ---------- | ------------------------------------------------------------ |
| RUNNING    | 正常状态，接受新的任务，处理等待队列中的任务                 |
| SHUTDOWN   | 不接受新的任务提交，但是会继续处理等待队列中的任务           |
| STOP       | 不接受新的任务提交，不再处理等待队列中的任务，中断正在执行任务的线程 |
| TIDYING    | 所有的任务都销毁了，当线程池转为次状态时，会执行钩子方法terminated() |
| TERMINATED | terminater()方法结束后的状态                                 |

**线程池中submit()和execute()方法的区别**

|           |                                      |
| --------- | ------------------------------------ |
| execute() | 只能执行Runnable类型的任务           |
| submit()  | 可以执行Runnable和Callable类型的任务 |

```java
class NumberThread implements  Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if((i & 1) == 0) {
                System.out.println(Thread.currentThread().getName() +" " + i);
            }
        }
    }
}


class NumberThread1 implements  Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if((i & 1) == 1) {
                System.out.println(Thread.currentThread().getName() +" " + i);
            }
        }
    }
}

public class ThreadPool {
    public static void main(String[] args) {
        //1.指定线程数量的线程池
        ExecutorService executorService = Executors.newFixedThreadPool(10);
        ThreadPoolExecutor service = (ThreadPoolExecutor)executorService;
        //设置线程池的属性 便于管理
        System.out.println(executorService.getClass());
        service.setCorePoolSize(15);

        //2.执行指定的线程的操作。需要提供实现Runnable接口或Callable接口实现类的对象
        executorService.execute(new NumberThread());//适合使用于Runnable
        executorService.execute(new NumberThread1());//适合使用于Runnable

        //executorService.submit();//适合使用于Callable

        //3.关闭连接池
        executorService.shutdown();
    }
}
```

### 5.3Thread中的常用方法

|                       |                                                              |
| --------------------- | ------------------------------------------------------------ |
| start()               | 启动当前线程；调用当前线程的run()                            |
| run()                 | 一般需要重写此方法，将要执行的业务代码写在此方法中           |
| currentThread()       | 静态方法，返回执行当前代码的线程                             |
| getName()             | 获取当前线程的名字                                           |
| setName()             | 设置当前线程的名字                                           |
| yield()               | 释放当前CPU的执行权                                          |
| join()                | 在线程a中调用线程b的join()，此时线程a进入阻塞状态，直到线程b完全执行完以后，线程a才结束阻塞状态 |
| stop()                | 已经过时。当执行此方法时，强制结束此线程                     |
| sleep(long millitime) | 让当前线程休眠指定milltime毫秒，在指定时间内，当前线程是阻塞状态 |



## 6.接口和抽象类

```java
abstract class AbstractTest {
    int a = 100;
    public abstract void funa();

    public void funb() { }
}

class ExtenAbstClass extends AbstractTest {
    @Override
    public void funa() { }
}
```

```java
interface InterfaceTest {
    int a = 0;
    //void funa(){} 接口中抽象方法不能有方法体 default修饰的可以
    default void funa(){ }
    void funb();
}

class ImpleInterTest implements InterfaceTest {
    @Override
    public void funb() {}
}
```

**相同点**

1. 都不能被实例化
2. 接口的实现类或抽象类的子类都只有实现了接口或抽象类中的（抽象）方法后才能被实例化

**不同点**

|              | 接口                                            | 抽象类                                                |
| ------------ | ----------------------------------------------- | ----------------------------------------------------- |
| 定义关键字   | interface                                       | abstract class                                        |
| 实现关键字   | implements（一个类可以实现多个接口）            | extends（一个类只能继承一个抽象类）                   |
| 能否实现方法 | default修饰的方法可以有方法体（可以不用被重写） | abstract方法不能有方法体（必须被重写，接口中的也是）  |
| 侧重点       | 接口强调功能的实现（like-a）                    | 抽象类强调所属关系（is-a）                            |
| 成员变量     | 默认为 public static final 且必须赋初值         | 默认为default，可以在子类中被重新定义，也可以重新赋值 |

