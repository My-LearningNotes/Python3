``queue.PriorityQueue``
=======================

对于\ ``PriorityQueue``\ , 我们需要给其传入一个tuple形式的数据(``(priority number, data``), 第一个数字表示优先级, 数组越小优先级越高, 第二个表示我们真正要使用的数据.

在从优先队列中取出元素时, 会根据优先级, 先取出优先级高的.

Example:

.. code-block:: python

    #!/usr/bin/env python3

    import queue

    q = queue.PriorityQueue()
    test_list = [(1, 'important'), (3, 'low'), (2, 'middle')]
    for i in test_list:
        q.put(i)

    while not q.empty():
        print(q.get())

    # 输出
    (1, 'important')
    (2, 'middle')
    (3, 'low')

