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

f-string, 亦称格式化字符串常量(formatted string literals), 是Python 3.6新引入的一种字符串格式化方法, 主要目的是使格式化字符串的操作更加简便. 
f-string在形式上是以\ ``f``\ 或\ ``F``\ 修饰引领的字符串(``f'xxx'``\ 或\ ``F'xxx'``), 以大括号\ ``{}``\ 标明替换的字段; 
f-string在本质上并不是字符串常量,  而是一个在运行时运算求值的表达式.

.. note::

    While other string literals always have a constant value,  formatted string are really expressions evaluated at run time.

f-string在功能方面不逊于传统的\ ``%-formatting语句``\ 和\ ``str.format()函数``\ , 同时性能上又优于两者,  且使用起来更加简洁明了,  
因此对于Python 3.6及以后的版本, 推荐使用f-string进行字符串格式化.

*   f-string使用花括号\ ``{}``\ 表示替换字段, 其中直接填入替换内容;

Example:

.. code-block:: python

    name = 'sylar'
    print(f'Hello, my name is {name}')

*   f-string的花括号\ ``{}``\ 可以填入表达式或调用函数, Python会求出其结果并填入返回的字符串内;

Example:

.. code-block:: python

    print(f'A total number of {24 * 8 + 4}')

    name = 'SYLAR'
    print(f'My name is {name.lower()}')


格式说明符
^^^^^^^^^^

自定义格式, 包括对齐, 宽度，符号，补零，精度，进制等.

f-string采用\ ``{content:format}``\ 设置字符串格式, 其中\ ``content``\ 是替换并填入字符串的内容，可以是变量, 表达式或函数调用等, ``format``\ 是格式描述符.
采用默认格式时不必指定\ ``{:format}``\ , 只写\ ``{content}``\ 即可.

关于格式描述符的详细语法及含义可查阅\ `Python官方文档 <https://docs.python.org/3/library/string.html#format-specification-mini-language>`_\ .


总结
----

有这么多中格式化字符串的方法, 应该如何抉择呢?

Python-String-Formatting中有一个很好的建议:

.. image:: images/python-string-formatting-flowchart.png

