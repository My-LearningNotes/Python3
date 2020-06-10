Python列表实现队列和栈
======================

队列(queue)和栈(stack)是两种数据结构, 其内部都是按照固定顺序来存放变量的, 两者的却别在于数据的存放顺序:

    *   队列是先存放的数据先取出, 即"先进先出"(First In First Out, FIFO);
    *   栈是后存放的数据先取出, 即"先进后出"(Fist In Last Out, FILO).

考虑到list类型数据本身的存放就是按顺序的, 而且内部元素又可以是不同的类型, 非常适合用于队列和栈的实现.


列表实现队列
------------

使用列表模拟队列功能的实现方法是, 定义一个\ ``list``\ 变量, 存放数据时使用\ ``insert()``\ 方法, 设置其第一个参数为0, 即表示每次都从最前面插入数据; 
读取数据时, 使用\ ``pop()``\ 方法, 即将队列的最后一个元素弹出.
如此, 列表中数据的存取顺序就符合"先进先出"的特点.

Example:

.. code-block:: python

    # 定义一个空列表, 当作队列
    queue = []
    # 向列表中插入元素
    queue.insert(0, 1)
    queue.insert(0, 2)
    queue,insert(0, 'hello')
    print(queue)
    print('取第一个元素: {}'.format(queue.pop()))
    print('取第二个元素: {}'.format(queue.pop()))
    print('取第三个元素: {}'.format(queue.pop()))


列表实现栈
----------

用列表模拟栈的实现方法是, 使用\ ``append()``\ 方法存入数据, 使用\ ``pop()``\ 方法读取数据.

Example: 

.. code-block:: python

    # 定义一个空list当作栈
    stack = []
    stack.append(1)
    stack.append(2)
    stack.append('hello')
    print(stack)
    print('取第一个元素: {}'.format(stack.pop()))
    print('取第二个元素: {}'.format(stack.pop()))
    print('取第三个元素: {}'.format(stack.pop()))


``collections``\ 模块实现队列和栈
---------------------------------

前面使用列表实现队列的例子中, 插入数据的部分是通过\ ``insert()``\ 方法实现的, 这种方法效率并不高, 因为每次从列表的开头插入一个数据, 列表中的所有元素都的向后移动一个位置.

一个相对高效的方法是使用标准库的\ ``collections``\ 模块中的\ ``deque``\ 结构体, 它被设计成在两端存入和读取都很快的特殊list, 可以用来实现栈和队列的功能.

Example:

.. code-block:: python

    queueAndStack = deque()
    queueAndStack.append(1)
    queueAndStack.append(2)
    queueAndStack.append('hello')
    print(list(queueAndStack))

    # 实现队列功能, 从队列中取一个元素
    print(queueAndStack.popleft())

    # 实现栈功能, 从栈中取一个元素
    print(queuqAndStack.pop())

