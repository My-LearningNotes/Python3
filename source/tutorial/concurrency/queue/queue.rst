``queue`` --- 一个同步的队列类
==============================

我们可以使用队列, 在线程之间共享数据. 
``queue``\ 模块实现了多生产者, 多消费者队列, 适用于在多线程之间安全地交换数据.
具体而言, 就是创建一个队列, 让生成者(线程)往其中放入数据, 而消费者(线程)从中取出数据.

**队列是基于锁机制来实现的, 它是线程安全的.**

``queue``\ 模块实现了三种类型的队列, 它们的区别仅仅是条目取出的顺序:

    * ``FIFO``\ 队列
    * ``LIFO``\ 队列
    * 优先级队列

此外, 模块实现了一个"简单的"FIFO队列类型: ``SimpleQueue``.


模块中的类和异常
----------------

* ``queue.Queue(maxsize=0)``

创建一个先入先出队列, ``maxsize``\ 指定队列最大大小, 否则(没有指定最大值), 为无限队列.

* ``queue.LifoQueue(maxsize=0)``       
  
创建一个后入先出队列, ``maxsize``\ 指定队列最大大小, 否则(没有指定最大值), 为无限队列.

* ``queue.PriorityQueue(maxsize=0)``   

创建一个优先级队列, ``maxsize``\ 指定队列最大大小, 否则(没有指定最大值), 为无限队列.

* ``queue.SimpleQueue``                

无界的\ ``FIFO``\ 队列. 

简单的队列, 缺少任务跟踪等高级功能.

* ``queue.Empty``   
  
当对空队列调用\ ``get*()``\ 方法时抛出该异常.

* ``queue.Full``
  
当对已满队列调用\ ``put*()``\ 方法时抛出该异常


队列的公共方法
--------------

* ``qsize()``                               
  
返回队列的大致大小. 

.. attention::

    ``qsize() > 0``\ 不保证后续的\ ``get()``\ 不被阻塞, ``qsize() < maxsize``\ 也不保证后先的\ ``put()``\ 不被阻塞. 
    因为在执行\ ``qsize()``\ 获取队列大小之后, 可能在其它线程中又对队列做了某些操作, 改变了队列的大小.

* ``empty()``                               
  
判断队列是否为空.

* ``full()``                                
  
判断队列是否已满.

* ``put(item, block=True, timeout=None)``   
  
把item放入队列. 

    - 如果block为True且timeout为None, 则在有可用空间之前阻塞; 
    - 如果timeout为正值, 则最多阻塞timeout秒; 
    - 如果block为False, 则抛出Full异常.
      
* ``put_nowait(item)``                      
  
和\ ``put(item, False)``\ 相同.

* ``get(block=True, timeout=None)``         
  
从队列中取出元素. 

    - 如果可选参数\ *block*\ 是\ ``True``
      
        + 如果\ *timeout*\ 是\ ``None``\ , 则在必须时阻塞直至有条目可以获得;
        + 如果\ *timeout*\ 是个正数, 将最多阻塞\ *timeout*\ 秒, 如果在这段时间内还是没有可获取的条目, 将引发\ ``Empty``\ 异常.
    - 如果\ *block*\ 是\ ``False``\, 立即获得一个条目, 如果没有可以获得的条目, 引发\ ``Emtpy``\ 异常.

* ``get_nowait()``                          
  
和\ ``get(block=False)``\ 相同.

* ``task_done()``                           
 
表示队列中当前的元素已经处理完毕. 

被消费者线程使用, 每个\ ``get()``\ 用于获取一个元素, 后续调用\ ``task_done()``\ 告诉队列, 对该元素的处理已经完毕. 

如果被调用的次数多于队列中的元素数量, 将引发\ ``ValueError``\ 异常.

* ``join()``                                
  
阻塞至队列中所有的元素都被接收和处理完毕.

当元素被添加到队列时, 未完成任务的计数就会增加. 
每当消费者线程调用\ ``task_done()``\ 表示这个元素已经处理完毕, 未完成计数就会减少. 
当未完成计数降到零的时候, ``join()``\ 阻塞就被解除.


如何等待排队的任务被完成的示例:

.. code-block:: python
    :emphasize-lines: 7, 10, 16, 20

    import threading, queue

    q = queue.Queue()

    def worker():
        while True:
            item = q.get()
            print(f'Working on {item}')
            print(f'Finished {item}')
            q.task_done()

    # turn-on the worker thread
    threading.Thread(target=worker, daemon=True).start()

    # send thirty task requests to the worker
    for item in range(30):
        q.put(item)
    print('All task requests send\n', end='')

    # block until all tasks are done
    q.join()
    print('All work completed')


.. toctree::
    :maxdepth: 1

    lifo_queue
    example
    
