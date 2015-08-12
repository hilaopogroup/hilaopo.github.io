title: python中的线程
date: 2015-06-16 17:07:58
categories:
- python
tags:
- thread

---

###python线程函数###

```python
In [1]: import threading

In [2]: threading.
threading.BoundedSemaphore  threading.RLock             threading.Timer             threading.current_thread    threading.settrace
threading.Condition         threading.Semaphore         threading.activeCount       threading.enumerate         threading.stack_size
threading.Event             threading.Thread            threading.active_count      threading.local             threading.warnings
threading.Lock              threading.ThreadError       threading.currentThread     threading.setprofile

In [3]: import thread

In [4]: thread.
thread.LockType          thread.allocate_lock     thread.exit              thread.get_ident         thread.stack_size        thread.start_new_thread
thread.allocate          thread.error             thread.exit_thread       thread.interrupt_main    thread.start_new

```

<!--more-->

---

####线程的创建与应用####
*这是一个坑*
>先填个网上找来的生产者消费者模型
**原文链接<http://bbs.pinggu.org/thread-3092201-1-1.html>**

```python
# -*- coding: utf-8 -*-
import threading, time

milk = 0
pool_size = 100 #池的大小
mylock =  threading.RLock()
class producer(threading.Thread):
    """生产者，主要的业务逻辑为 往池中加入milk，以备消费者使用"""

    def __init__(self, step, theadName):
        threading.Thread.__init__(self)
        self.step = step
        self.isRunable = True
        self.threadName =  theadName

    def run(self):
        global milk
        while True and self.isRunable:
            time.sleep(2)
            mylock.acquire();
            if(milk >= pool_size):
                print "[生产者]池中的milk已经足够多[%dL]，怎么就没有人来买?" % (milk)
            else:
                interval =  pool_size - milk  #防止池溢出，造成浪费
                if(interval < self.step):
                    milk = pool_size
                else:
                    interval = self.step
                    milk += self.step
                print "[生产者]名称：%s 生产了 %dL 的milk 加入了池中, 池中的总量为 %dL"   % (self.threadName, interval, milk)
            mylock.release()
        else:
            print "干了一天了，准备休息了拜拜了！"

    def stop(self):
        self.isRunable = False

class customer(threading.Thread):

    def __init__(self, amount , theadName):
        threading.Thread.__init__(self)
        self.amount  = amount
        self.isRunable = True
        self.threadName =  theadName

    def run(self):
        global milk
        while True and self.isRunable:
            time.sleep(5)
            mylock.acquire();
            if(milk > self.amount ):
                milk -= self.amount
                print "[消费者]名称 %s 新鲜的milk,我要开始消费了,老板来%dL"  % (self.threadName, self.amount)
            else:
                print "[消费者]池中的milk不足够我消费，等待中....."
            mylock.release()
        else:
            print "差不多了，货都买齐了，准备撤了!"

    def stop(self):
        self.isRunable = False


if __name__ == "__main__":
    p1 = producer(10, "小奶牛")
    p2 = producer(20, "老奶牛")
    p1.start()
    p2.start()

    c1 =  customer(5, "路人甲")
    c2 =  customer(4, "路人乙")
    c3 =  customer(5, "杨白劳")

    c1.start()
    c2.start()
    c3.start()

    time.sleep(2*60) #准备结束
    p1.stop()
    p2.stop()

    c1.stop()
    c2.stop()
    c3.stop()

```
