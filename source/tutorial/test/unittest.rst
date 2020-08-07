``unittest``
============

``unittest``\ 是Python标准库提供的单元测试框架, 使用\ ``unittest``\ 可以以结构方式编写庞大而详尽的测试集.


* 测试文件的名称中最好包含test字段, 以表示这是一个测试文件, 比如以\ ``test_``\ 开头或者以\ ``_test``\ 结尾;
* 在测试文件中, 导入\ ``unittest``\ 模块和被测试的模块, 以及其它使用到的模块;
* 编写单元测试时, 需要编写一个测试类, 从\ ``unittest.TestCase``\ 继承, 测试类的名称中应该也包含test字段, 
  比如, 以\ ``Test``\ 开头, 或者以\ ``Test``\ /\ ``TestCase``\ 结尾;
* 测试类中, 以\ ``test``\ 开头的方法就是测试方法, 不以\ ``test``\ 开头的方法不被认为是测试方法, 测试的时候不会被执行; 
  对每一类测试都需要编写一个\ ``test_xxx()``\ 方法.
  ``unittest.TestCase``\ 提供了很多内置的判断条件, 我们只需要调用这些方法就可以断言输出是否符合预期;
* 在测试文件的末尾执行\ ``unittest.main()``\ 函数. ``unittest.main()``\ 函数负责运行测试, 它实例化所有\ ``unittest.TestCase``\ 的子类, 并运行所有以\ ``test``\ 开头的方法.


测试夹具
--------
   
可以在测试类中定义两个特殊的方法: ``setUp()``\ 和\ ``tearDown()``\ , 这两个方法会在每一个测试方法的之前和之后执行. 
可以使用这些方法来执行适用于所有测试的初始化和清理代码, 这些代码称为\ **测试夹具(text fixure)**\ .

``setUp()``\ 和\ ``tearDown()``\ 方法有什么用呢?
比如, 测试中需要启动一个数据库, 这里, 就可以在\ ``setUp()``\ 方法中连接数据库, 在\ ``tearDown()``\ 方法中关闭数据库, 
这样, 就不必在每个测试方法中重复相同的代码.


对测试结果的检查
----------------

``TestCase``\ 包含多种对测试结果进行检查的方法, 这些方法以\ ``assert``\ 开头, 用来检查结果是否符合指定条件. 
如果结果符合指定条件, 表示测试成功; 如果不符合指定条件, 表示测试失败. 

完成的检查方法参见: `unittest.TestCase <https://docs.python.org/3/library/unittest.html#unittest.TestCase>`_

常用的方法有:

======================== =========================
Method                   Checks that
``assertEqual(a, b)``    a == b
``assertNotEqual(a ,b)`` a != b
``assertTrue(x)``        bool(x) is True
``assertFalse(x)``       bool(x) is False
======================== =========================

常用的还有\ ``assertRaise()``\ 方法, 用来检查代码是否引发了指定的异常. 

.. code-block:: python
    
    with self.assertRaises(SomeException):
        do_something()

如果还要对异常做进一步的分析:

.. code-block:: python

    with self.assertRaises(SomeException) as cm:
        do_something()

    the_exception = cm.exception
    self.assertEqual(the_exception.error_code, 3)


错误和失败
----------

模块\ ``unittest``\ 区分错误和失败. 

* **错误**\ 指的是代码引发了异常, 是代码本身出现了问题, 测试还没有完成, 还没有得到测试结果. 
* **失败**\ 指的是测试已经完成了, 已经得到测试结果, 但是结果不符合指定的条件.


测试结果的显示
--------------

在终端显示的测试结果中, 第一行, ``.``\ 表示测试Pass, ``F``\ 表示测试Fail, ``E``\ 表示测试Error, 它们的数量表示运行了多少个测试方法.


Example:

.. code-block:: python

    #!/usr/bin/env python3

    class Student():
        def __init__(self, name, score):
            self.name = name
            self.score = score
    
        def get_grade(self):
            if self.score > 100 or self.score < 0:
                raise ValueError

            if self.score >= 80:
                return 'A'
            elif self.score >= 60:
                return 'B'
            else:
                return 'C'


    import unittest

    class TestStudent(unittest.TestCase):
        def setUp(self):
            print('setUp')

        def tearDown(self):
            print('tearDown')

        def test_80_to_100(self):
            s1 = Student('Bart', 80)
            s2 = Student('Lisa', 100)
            self.assertEqual(s1.get_grade(), 'A')
            self.assertEqual(s2.get_grade(), 'A')

        def test_60_to_80(self):
            s1 = Student('Bart', 60)
            s2 = Student('Lisa', 79)
            self.assertEqual(s1.get_grade(), 'B')
            self.assertEqual(s2.get_grade(), 'B')

        def test_0_to_60(self):
            s1 = Student('Bart', 0)
            s2 = Student('Lisa', 59)
            self.assertEqual(s1.get_grade(), 'C')
            self.assertEqual(s2.get_grade(), 'C')

        def test_invalid(self):
            s1 = Student('Bart', -1)
            s2 = Student('Lisa', 101)
            with self.assertRaises(ValueError):
                s1.get_grade()
            with self.assertRaises(ValueError):
                s2.get_grade()

    if __name__ == '__main__':
        unittest.main()

