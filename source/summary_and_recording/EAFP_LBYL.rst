EAFP和LBYL两种编程风格
======================

检查数据可以让程序更健壮, 用术语来说就是防御性编程. 

检查数据的时候, 有两种不同的风格:

*   **EAFP**\ : It's Easier to Ask for Forgiveness than Permisiion.

不检查条件, 出了问题由异常处理来处理.

*   **LBYL**\ : Look Before You Leap. 

先检查条件.


EAFP
----

**Easier to ask for forgiveness than permisiion.** 
请求原谅要比请求允许容易的多.

This common Python coding style assumes the existence of valid keys or attributes and catch exceptions if the assumption proves false. 
This clean and fast style is characterized by the presence of may ``try`` and ``except`` statments.


LBYL
----

**Look before you leap.**

This coding style explicitly tests for pre-conditions before making calls or lookups.

In a multi-threaded environment, the LBYL approach can risk introducing a race condition between "the looking" and "the leaping". 
For example, the code ``if key in mapping: return mapping[key]`` can fail if another thread moves key from mapping after ther test, but before the lookup. 
This issue can be solved witk lock or by using the EAFP approach.


示例
----

例如, 尝试访问map的key. 

.. code-block:: python

    # EAFP风格
    try:
        x = my_dict['key']
    except KeyError:
        # handle missing key


    # LBYL风格
    if 'key' in my_dict:
        x = my_dict['key']
    else:
        # handling missing key


总结
----

这两种风格各有好坏. 

对于LBYL, 容易打乱思维, 本来业务逻辑用一行代码就可以搞定, 却多出来了很多行用于检查的代码. 
防御性的代码跟业务代码逻辑混在一起降低了可读性. 

而EAFP, 业务逻辑代码跟防御代码隔离的比较清晰, 更容易让开发者专注业务逻辑. 
不过, 异常处理会影响一点性能. 因为发生异常的时候, 需要保留现场, 回溯trackback等操作. 
但其实性能相差不大, 尤其是异常发生的频率比较低的时候. 

还有一点需要注意的是, 如果涉及到原子操作, 强烈推荐使用EAFP风格. 
因为如果使用LBYL风格, 多线程环境下会引入竞争条件, 而使用EAFP风格可以确保原子性.

