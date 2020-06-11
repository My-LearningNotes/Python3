Python使用字典格式化字符串
==========================

*   对于格式字符串, 可以使用\ ``format_map()``\ 方法, 从一个字典中读取值;

    *   ``format_map()``\ 方法的参数是一个字典;
    *   在格式字符串的替换字段中, 通过键值对的key来引用其对应的value.

Example:

.. code-block:: python

    msg = {'name': 'sylar', 'age': 18, 'sex': 'male'}

    print('name is {name}, age is {age}, sex is {sex}'.format_map(msg))


*   对于f-string, 可以直接在替换字段中使用键值对.

Example:

.. code-block:: python

    msg = {'name': 'sylar', 'age': 18, 'sex': 'male'}
    print(f"name is {msg['name']}, age is {msg['age']}, sex is {msg['sex']}")

