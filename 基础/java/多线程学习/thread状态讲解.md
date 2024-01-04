Thread存在6种状态，这六种状态保存在Thread.class中

```java
public enum State {
        /**
         * 表示线程已经创建但是还没有启动。
         */
        NEW,

        /**
         * 表示线程可运行，但是可能存在其他线程正在jvm执行，需要等待并不是真正的执行状态。
         */
        RUNNABLE,

        /**
         * 阻塞状态是线程阻塞在进入synchronized关键字修饰的方法或代码块(获取锁)时的状态。
         */
        BLOCKED,

        /**
         * 处于等待状态的线程正在等待另一个线程执行特定操作。 例如，一个在对象上调用Object.wait()的线
         * 程正在等待另一个线程在该对象上调用Object.notify()或Object.notifyAll()。 调               
         * Thread.join()的线程正在等待指定的线程终止。
         */
        WAITING,

        /**
         * 线程在等待其他线程完成操作，但是线程的等待有时间限制。
         */
        TIMED_WAITING,

        /**
         * 线程完成操作。一旦线程处于该状态，无法再改变状态。
         */
        TERMINATED;
    }
```

