---
title: 通过ReentrantLock了解AQS
date: 2022-04-8 16:39:40
coverWidth: 1200
coverHeight: 750
cover: https://cdn.jsdelivr.net/gh/aimer-period/pic/img/22.png
categories:
 - Java
tags:
 - JUC
 - AQS
---

> JUC： java.util.concorrent.*    Java支持高并发的实现包

# AQS

AQS到底是什么？

1、一个队列

2、一个唤醒机制

3、一堆队列节点状态

4、一个state（为0表示无锁，数字是几就是几把锁，可能锁重入）

5、模板方法设计模式

```Java
class X{
  private final ReentrantLock lock = new ReentrantLock ();
  public void m(){
    lock.lock();
    try{
      //doSomething 互斥区
    }finally{
      lock.unlock();
    }
  }
}
```

# 公平锁和非公平锁最大的区别

公平锁先看是否有人排队，没有人直接上锁

非公平锁是不管有没有人先直接抢锁，失败再去排队

经过实践非公平锁是性能更加优越的，所以默认使用非公平锁，但性能更好是有前提的

```Java
if (c == 0) {
   if (!hasQueuedPredecessors() && //有没有线程在排队
        compareAndSetState(0, acquires)) {
              setExclusiveOwnerThread(current);
                    return true;
                }                               
       }
            
  //这是公平锁的，非公平锁就没有第一个判断是否有线程在排队
//AQS获取锁的核心方法
public final void acquire(int arg) {
        if (!tryAcquire(arg) //子类看看能不能获取锁
        &&
            acquireQueued(addWaiter(Node.EXCLUSIVE), arg))//先去排队，看看需不需要去休息区等候
            selfInterrupt();
    }
```



# 源码

先从`ReentrantLock`开始

```java
public class ReentrantLock implements Lock, java.io.Serializable {
```

