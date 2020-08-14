``queue.LifoQueue``
===================

它的用法和\ ``Queue``\ 完全一致.
只是\ ``Queue``\ 实现的是\ ``FIFO(first-in-first-out)``\ 数据结构, 而\ ``LifoQueue``\ 实现的是\ ``LIFO(last-in-first-out)``\ 数据结构.

Example:

.. code-block:: python
    :emphasize-lines: 5, 8, 11, 18, 21, 24

    #!/usr/bin/env python3

    import queue

    q_fifo = queue.Queue()

    for i in range(5):
        q_fifo.put(i)

    while not q_fifo.empty():
        print(q_fifo.get(), end=' ')

    print()


    # "****************************"

    q_lifo = queue.LifoQueue()

    for i in range(5):
        q_lifo.put(i)

    while not q_lifo.empty():
        print(q_lifo.get(), end=' ')

    # 输出
    0 1 2 3 4 
    4 3 2 1 0

