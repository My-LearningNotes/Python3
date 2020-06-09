Python格式化字符串
==================

Python目前有几种进行格式化字符串的方式:

    *   使用\ ``%``\ 操作符进行格式化字符串;
    *   使用\ ``str.format()``\ 方法进行格式化字符串;
    *   模板字符串
    *   Formatted String Literals(Python 3.6)


使用\ ``%``
-----------

在Python3中, 不推荐使用这种方式.


使用\ ``str.format()``
----------------------

使用字符串的\ ``format``\ 方法.

格式化字符串语法
^^^^^^^^^^^^^^^^

格式化字符串包含由\ ``{}``\ 括起来的替换字段, 不包含在\ ``{}``\ 中的内容不做处理.

格式化字符串可以通过\ ``{{``\ 和\ ``}}``\ 的方式来包含\ ``{``\ 和\ ``}``\ .

替换字段语法:

*   **参数名**

    字符串的\ ``format``\ 方法支持按位置和按名字两种方式进行格式化:

    *   **按位置** - 在\ ``{}``\ 中通过索引来引用\ ``format()``\ 方法中的值.

        *   索引从0开始;
        *   若省略\ ``{}``\ 中的索引值, 则根据\ ``{}``\ 的位置依次对应\ ``format()``\ 中的各个值.

            例如, ``{}{}{}``\ 等价于\ ``{0}{1}{2}``\ .

    *   **按名字** - 在\ ``{}``\ 中通过名字来引用\ ``{}``\ 方法中的值.

    若参数存在属性, 可以通过\ ``arg_name.attribute_name``\ 的形式获取属性值.

    若参数位可迭代对象, 可以通过\ ``arg_name[integer | index_string]``\ 的形式获取索引位置的元素.

*   **转换字段**

    由\ ``!``\ 开始, ``r``\ 代表调用\ ``repr()``, ``s``\ 代表调用\ ``str()``\ .

*   **格式说明符**

    由\ ``:``\ 开始.

Example:

.. code-block:: python

    "First, thou shalt count to {0}"  # 引用第一个位置参数
    "Bring me a {}"                   # 隐式引用第一个位置参数
    "From {} to {}"                   # 等价于"From {0} to {1}"
    "My quest is {name}"              # 引用关键字参数"name"
    "Weight in tons {0.weight}"       # 第一个位置参数的"weight"属性
    "Units destroyed: {players[0]}"   # 关键字参数'players'的第一个元素
    "Units destroyrd: {players[key]}" # 关键字参数'players' 'key'键上的元素
    "Harold's a clever {0!s}"         # 首先在参数上调用str()
    "Bring out the holy {name!r}"     # 首先在参数上调用repr()
    

格式说明符
^^^^^^^^^^

format_spec允许定义该替换字段的表现形式, 如左右对齐, 宽度等.

详见: https://docs.python.org/3/library/string.html#format-specification-mini-language


模板字符串
----------


Formatted String Literals
-------------------------

这种格式化字符串的方法是由Python 3.6新加入的, 它允许你在字符串内内嵌Python运算符式.


总结
----

有这么多中格式化字符串的方法, 应该如何抉择呢?

Python-String-Formatting中有一个很好的建议:

.. image:: images/python-string-formatting-flowchart.png

