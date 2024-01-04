# sleep

当一个线程调用sleep方法时，它会暂停执行指定的时间，让其他线程有机会运行。在这段时间内，该线程处于阻塞状态，它不会执行任何操作，也不会占用CPU资源。当指定的时间到了，该线程会从阻塞状态返回到可运行状态，等待CPU资源分配。

需要注意的是，调用sleep方法并不会释放该线程持有的锁，因此在多线程并发的情况下，如果一个线程在持有锁的情况下调用sleep方法，其他线程将无法获得该锁，直到该线程从sleep方法中返回。

因此，在编写多线程程序时，需要注意合理地使用sleep方法，以避免出现线程阻塞导致的死锁等问题。

**下面是可能导致线程阻塞的代码**

其中线程A和线程B共同竞争一个共享变量count，线程A先获取到count的锁并且调用了sleep方法，导致线程B无法获取到count的锁，从而等待线程A执行完毕后才能继续执行：

```java
public class SleepExample {
    private int count = 0;

    public synchronized void increment() throws InterruptedException {
        System.out.println(Thread.currentThread().getName() + " has acquired the lock.");
        if(count == 0) {
            System.out.println(Thread.currentThread().getName() + " is going to sleep.");
            Thread.sleep(5000);  // 线程A调用sleep方法，暂停执行5秒
        }
        count++;
        System.out.println(Thread.currentThread().getName() + " has incremented the count.");
    }

    public static void main(String[] args) {
        SleepExample example = new SleepExample();
        Runnable runnable = () -> {
            try {
                example.increment();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        };
        Thread threadA = new Thread(runnable, "ThreadA");
        Thread threadB = new Thread(runnable, "ThreadB");
        threadA.start();
        threadB.start();
    }
}
```

当线程A获取到count的锁后，判断count为0，于是调用sleep方法暂停执行5秒。此时，线程B无法获取到count的锁，因为该锁被线程A持有，线程B会一直等待，直到线程A执行完毕并且释放了count的锁，线程B才能获取到该锁并继续执行。因此，线程A的sleep方法导致了线程B的等待。

当线程执行到`synchronized`关键字修饰的方法或代码块时，会获取到对象的锁，只有获取到锁的线程才能执行该方法或代码块，其他线程需要等待锁的释放。

`increment`方法是被`synchronized`关键字修饰的，因此当一个线程执行该方法时，它会获取到`example`对象的锁，然后执行`count++`操作。其他线程需要等待该锁的释放后才能获取到锁并执行该方法。当所有线程执行完毕后，程序输出`Count`变量的值，它的值应该是`200000`，因为每个线程都会执行`increment`方法100000次。因此，在整个程序执行过程中，线程在执行`synchronized`方法时才会获取到锁。

# wait

Java中的sleep和wait方法都可以暂停线程的执行，但它们的使用场景和作用有所不同，主要区别如下：

1. 对象锁的释放：wait方法会释放对象的锁，而sleep方法不会释放对象的锁。当一个线程调用wait方法时，它会释放当前持有的对象锁，并且进入该对象的等待池中，直到其他线程调用该对象的notify或notifyAll方法来唤醒它。而当一个线程调用sleep方法时，它会暂停执行指定时间，并不会释放对象的锁。
2. 调用方式：wait方法必须在同步方法或同步块中调用，而sleep方法可以在任何地方调用。
3. 目的：wait方法主要用于线程间通信，而sleep方法主要用于暂停线程执行。
4. 异常：调用wait方法时需要捕获InterruptedException异常，而sleep方法不需要。

综上所述，wait方法主要用于线程间的同步和通信，而sleep方法主要用于暂停线程的执行。它们在使用场景和作用上有所不同，需要根据具体的需求选择合适的方法。