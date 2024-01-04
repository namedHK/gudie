## 介绍

ReentrantLock是java中一种线程锁，与synchronized不同，是通过代码实现的，并且比synchronized灵活，并且具有一下特点：

- 等待可中断
- 公平锁
- 锁绑定多个条件
- 重入失败可以做其他操作

性能方面相差不大，并且官方推荐使用synchronized

其实现原理是AbstractQueuedSynchronized。

### AbstractQueuedSynchronized

> AQS定义了一套多线程访问共享资源的同步器框架，许多同步类实现都依赖于它，如常用的ReentrantLock/Semaphore/CountDownLatch等等。