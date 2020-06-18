Python类命名空间
================

在定义一个类时, 会引入新的命名空间: **类命名空间**\ . 

.. note::

    和类命名空间相对的是全局命名空间, 即整个Python程序默认都位于全局命名空间中. 
    而类体则独立位于类命名空间中.

类属性和类方法都是定义在类命名空间.

Example:

.. code-block:: python

    # 全局空间定义变量
    name = 'jack'
    age = 20

    # 全局空间定义函数
    def show():
        print('hello, world')

    # 定义一个类
    class Person:
        # 定义Person空间的show函数
        @staticmethod
        def show():
            print('hello, China')

        # 定义Person空间的变量
        name = 'sylar'
        age = 18

    # 调用全局的变量和函数
    print(name, age)
    show()

    # 调用类独立空间的变量和函数
    print(Area.name, Area.age)
    Area.show()


Python还允许在类命名空间中编写可执行代码(例如输出语句, 分支语句等), 这些代码将在导入定义的文件时执行.

Example:

.. code-block:: python

    class Person:

        print('hello, world')
        for i in range(5):
            print(i)

