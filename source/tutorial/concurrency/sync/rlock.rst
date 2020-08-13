可重入锁
========

对于原始锁:

    * 在锁定时不属于任何线程, 在任何线程中都可以释放;
    * 处于锁定状态的锁, 在任何线程中都无法再获取, 直到被释放.

**可重入锁(Reentrant Lock)**, 是一种可以被同一个线程多次获取的锁. 
在内部, 它在原始锁的\ *锁定/非锁定*\ 状态上附加了\ "所属线程"和"递归等级"的概念. 
**在锁定状态下, 某些线程拥有它; 在非锁定状态下, 没有线程拥有它.**

若要锁定锁, 线程调用\ ``acquire()``\ 方法; 一旦线程拥有了锁, 方法将返回. 
若要解锁, 线程调用\ ``release()``\ 方法. 
``acquire()/release()``\ 对可以嵌套, 只有最终\ ``release()``\ (最外面的一对\ ``release()``)将锁解开, 才能让其它线程继续处理\ ``acquire()``\ 阻塞.

递归锁也支持上下文管理协议.

.. note::

    ``RLock``\ 对比\ ``Lock``\ 有三个特点:

    * 谁拿到谁释放. 如果线程A拿到锁, 线程B无法释放这个锁, 只有A可以释放;
    * 同一线程可以多次拿到该锁, 即可以\ ``acquire``\ 多次;
    * ``acquire``\ 多少次就必须\ ``release``\ 多少次, 只有最后一次\ ``release``\ 才能改变\ ``RLock``\ 的状态为\ ``unlocked``\ .


``threading.RLock``
-------------------

此类实现了可重入锁对象. 

**可重入锁必须由获取它的线程释放. 一旦线程获取了可重入锁, 同一个线程再次获取它将不阻塞; 线程必须在每次获取它时释放一次.**

* ``acquire(blocking=True, timeout=-1)``

可以阻塞或非阻塞地获得锁.

当无参调用时: 如果这个线程已经拥有锁, 递归等级增加一, 并立即返回. 否则, 如果其它线程拥有该锁, 则阻塞直至该锁解锁. 一旦锁被解锁(不属于任何线程), 则抢夺所有权, 设置递归等级为一, 并返回. 如果多个线程被阻塞, 等待锁被解锁, 一次只有一个线程能抢到锁的所有权. 

* ``release()``

释放锁, 自减递归等级. 如果减到零, 则将锁重置为非锁定状态(不被任何线程拥有), 并且, 如果有其它线程正被阻塞着等待锁被解锁, 则允许其中一个线程继续. 如果自减后, 递归等级仍然不是零, 则锁保持锁定状态, 仍由调用线程拥有它.

**当前线程只有拥有锁才能调用这个方法.** 
如果锁被释放后调用此方法, 会引发\ ``RuntimeError``\ 异常.


示例
----

.. code-block:: python
    :emphasize-lines: 9

    #!/usr/bin/env python3

    import threading
    import time


    class Box(object):
        # 可重入锁
        lock = threading.RLock()

        def __init__(self):
            self.total_items = 0

        def execute(self, n):
            Box.lock.acquire()
            self.total_items += n
            Box.lock.release()

        def add(self):
            Box.lock.acquire()
            self.execute(1)
            Box.lock.release()

        def remove(self):
            Box.lock.acquire()
            self.execute(-1)
            Box.lock.release()

    def adder(box, items):
        while items > 0:
            print(f'adding 1 item in the box')
            box.add()
            time.sleep(1)
            items -= 1

    def remove(box, items):
        while items > 0:
            print(f'removing 1 item in the box')
            box.remove()
            time.sleep(1)
            items -= 1

        
    if __name__ == '__main__':
        items = 5
        print(f'putting {items} items in the box')
        box = Box()
        t1 = threading.Thread(target=adder, args=(box, items))
        t2 = threading.Thread(target=remove, args=(box, items))
        t1.start()
        t2.start()
        t1.join()
        t2.join()
        print(f'{items} items still remain in the box')

在上面的例子中使用了可重入锁\ ``RLock``\ , 这里也必须使用可重入锁. 
因为在\ ``execute()``\ 方法中会获取锁, 在\ ``add()``\ / ``remove()``\ 方法也会获取锁, 然后再调用\ ``execute()``\ , 如果使用的是不是可重入锁, 则在\ ``add()``\ / ``remove()``\ 方法中获取锁之后, 再调用\ ``executre()``\ 方法时就会导致死锁.

