Python ``frozenset``\ 集合
==========================

``set``\ 集合是可变类型的, 程序中可以改变序列中的元素; 
``forzenset``\ 集合是不可变类型的, 程序中不能改变其中的元素.

两种情况下可以使用\ ``frozenset``\ :

    *   当集合中的元素不需要改变时, 可以使用\ ``frozenset``\ 替代\ ``set``\ , 这样更加安全;
    *   有时候程序要求必须是不可变对象, 这个时候要使用\ ``frozenset``\ 替代\ ``set``\ . 比如, 字典的键就要求必须是不可变对象.

Example:

.. code-block:: python

    s = {'Python', 'C', 'C++'}
    fs = frozenset(['Java', 'Shell'])
    s_sub = {'PHP', 'C#'}

    # 向set集合中添加frozenset
    s.add(fs)

    # 向set集合中添加set集合, 错误
    # 因为set集合中只能包含不可变类型的对象, 而set集合是可变类型的
    s.add(s_sub)

