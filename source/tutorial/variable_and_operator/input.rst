Python ``input()``\ 函数
========================

``input()``\ 是Python的内置函数, 用于从控制台读取用户输入的内容. 
``input()``\ **函数总是以字符串的形式来处理用户输入的内容**\ , 所以用户输入的内容可以包含任何字符.

``input()``\ 函数的用法为:

.. code-block:: python

    str = input(msg)

*   ``str``\ 表示一个字符串类型的变量, ``input``\ 会将读取到的字符串放入\ ``str``\ 中;
*   ``msg``\ 表示提示信息, 它会显示在控制台上; 如果不写\ ``msg``\ , 就不会有任何提示信息;
*   按下回车键后\ ``input()``\ 读取结束.


Example:

.. code-block:: python

    a = input("Enter a number: ")
    b = input("Enter another number: ")

    print("aType: ", type(a))
    print("bType: ", type(b))

    result = a + b
    print("resultValue: ", result)
    print("resultType: ", type(result))


**可以使用Python内置函数将字符串转换成想要的类型**, 比如:

    *   ``int(string)`` - 将字符串转换成int类型;
    *   ``float(string)`` - 将字符串转换成float类型;
    *   ``bool(string)`` - 将字符串转换成bool类型.

Example:

.. code-block:: python

    a = input("Enter a number: ")
    b = input("Enter another number: ")
    a = float(a)
    b = int(b)

    print("aType: ", type(a))
    print("bType: ", type(b))

    result = a + b
    print("resultValue: ", result)
    print("resultType: ", type(result))

