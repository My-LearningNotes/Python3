Python类特殊成员
================

在Python类中, 凡是以双下划线\ ``__``\ 开头和结尾的成员, 都被称为类的特殊成员(特殊属性和特殊方法).

Python类中的特殊成员, 其特殊性类似C++类的\ ``private``\ 成员, 即不能在类的外部直接调用, 但允许借助类中的普通方法调用甚至修改它们. 
如果需要, 还可以对类的特殊方法进行重写, 从而实现一些特殊的功能. 


.. toctree::
    :maxdepth: 1

    __new__
    __del__
    __dir__
    __dict__
    __repr__
    attr_function
    protocol
    iterator
    generator

