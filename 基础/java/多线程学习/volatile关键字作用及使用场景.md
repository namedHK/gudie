## [djava volatile关键字作用及使用场景](https://www.cnblogs.com/YLsY/p/11295732.html)

- 特性

  - 保证可见性

    - > volatile关键字不存在一致性问题，但是因为并非原子操作，所以在并发下进行非原子操作时，一样是线程不安全的。

  - 不保证原子性

  - 禁止指令重排

    - > jvm在编译的时候会更改命令的顺序，但是会保证输出结果一致，但是在多线程的情况下引用同一个变量会导致线程不安全。使用volatile后，相关命令编译时不会再进行排序

- 注意

  - > jvm虚拟机存在 **Happens-Before** 原则，伪代码如下
    >
    > ```java
    > //A 线程执行一下操作
    > i = 1;
    > //B 线程执行一下操作
    > i = 1;
    > // A线程执行一下操作
    > i = 1;
    > ```
    >
    > 

 

1. volatile关键字的作用：保证了变量的可见性（visibility）。被volatile关键字修饰的变量，如果值发生了变更，其他线程立马可见，避免出现脏读的现象。如以下代码片段，isShutDown被置为true后，doWork方法仍有执行。如用volatile修饰isShutDown变量，可避免此问题。

```java
public class VolatileTest3 {
    static class Work {
        boolean isShutDown = false;

        void shutdown() {
            isShutDown = true;
            System.out.println("shutdown!");
        }

        void doWork() {
            while (!isShutDown) {
                System.out.println("doWork");
            }
        }
    }

    public static void main(String[] args) {
        Work work = new Work();

        new Thread(work::doWork).start();
        new Thread(work::doWork).start();
        new Thread(work::doWork).start();
        new Thread(work::shutdown).start();
        new Thread(work::doWork).start();
        new Thread(work::doWork).start();
        new Thread(work::doWork).start();
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

出现脏读时，运行结果如下：

![img](https://img2018.cnblogs.com/blog/999430/201908/999430-20190803180051561-1745936038.png)

2. 为什么会出现脏读？

Java内存模型规定所有的变量都是存在主存当中，每个线程都有自己的工作内存。线程对变量的所有操作都必须在工作内存中进行，而不能直接对主存进行操作。并且每个线程不能访问其他线程的工作内存。变量的值何时从线程的工作内存写回主存，无法确定。

3. happens-before规则的理解与勘误

在网上查volatile关键字相关信息时，多篇博客提到了happens-before原则，个人对此原则的理解是：当操作该volatile变量时，所有前序对该变量的操作都已完成（如不存在已变更，但未写回主存的情况），所有后续对该变量的操作，都未开始。仅此而已。

这里，我认为网上很常见的一个理论对此理解有误，如下图。此观点认为，由于volatile变量flag的happens-before原则，所以A线程2处对其的写操作一定先于B线程3处对其的读操作。其实这种观点是有逻辑缺陷的，如果存在一个C线程，先读取flag的值，后写入flag的值，那C线程的执行时机是什么呢？如果还有其他D、E线程呢。。。对于这段代码的正确理解是，只要3处拿到的flag是true，那么a的值一定是1，而不是0.因为volition修饰的变量，处理器不会对其进行重排序，所以1处对a的赋值，一定发生在2处对flag的赋值之前。如果flag不是volatile变量，那么1处和2处代码的执行顺序是无法保证的（处理器的指令重排序），虽然大部分情况1会先于2执行。happens-before原则约束的并不是多线程对同一变量的读和写操作之间的顺序，而是保证读操作时，前序所有对该变量的写操作已生效（写回主存）。

 

![img](https://img2018.cnblogs.com/blog/999430/201908/999430-20190803181200407-1684353189.png)

 

![img](https://user-gold-cdn.xitu.io/2018/5/2/16320e796b904658?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

验证如下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 public class VolatileTest {
 2     static class A {
 3         int a = 0;
 4         volatile boolean flag = false;
 5 
 6         void writer() {
 7             a = 1;                   //1
 8             flag = true;               //2
 9             System.out.println("write");
10         }
11 
12         void reader() {
13             if (flag) {                //3
14                 int i =  a;           //4
15                 System.out.println("read true");
16                 System.out.println("i is :" + i);
17             } else {
18                 int i = a;
19                 System.out.println("read false");
20                 System.out.println("i is :" + i);
21             }
22         }
23 
24     }
25 
26     public static void main(String[] args) {
27         A aaa = new A();
28         new Thread(() -> aaa.reader()).start();
29         new Thread(() -> aaa.writer()).start();
30     }
31 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

运行结果如下，在写操作执行之前，读操作已完成

![img](https://img2018.cnblogs.com/blog/999430/201908/999430-20190803183038230-1301272748.png)

 

4. volatile关键字使用场景

注意：volatile只能保证变量的可见性，不能保证对volatile变量操作的原子性，见如下代码：

 

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
 1 public class VolatileTest2 {
 2     static class A {
 3         volatile int a = 0;
 4         void increase() {
 5             a++;
 6         }
 7         int getA(){
 8             return a;
 9         }
10     }
11 
12     public static void main(String[] args) {
13         A a = new A();
14 
15         new Thread(() -> {
16             for (int i = 0;i < 1000;i++) {
17                 a.increase();
18             }
19             System.out.println(a.getA());
20         }).start();
21         new Thread(() -> {
22             for (int i = 0;i < 2000;i++) {
23                 a.increase();
24             }
25             System.out.println(a.getA());
26         }).start();
27         new Thread(() -> {
28             for (int i = 0;i < 3000;i++) {
29                 a.increase();
30             }
31             System.out.println(a.getA());
32         }).start();
33         new Thread(() -> {
34             for (int i = 0;i < 4000;i++) {
35                 a.increase();
36             }
37             System.out.println(a.getA());
38         }).start();
39         new Thread(() -> {
40             for (int i = 0;i < 5000;i++) {
41                 a.increase();
42             }
43             System.out.println(a.getA());
44         }).start();
45     }
46 }
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

运行结果如下，volatile无法保证a++操作的原子性。

![img](https://img2018.cnblogs.com/blog/999430/201908/999430-20190803184143940-89162710.png)

volatile正确的使用方法可参考：https://blog.csdn.net/vking_wang/article/details/9982709

