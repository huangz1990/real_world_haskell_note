第二章：类型和函数
*******************

什么是类型
==========

在Haskell中，\ *所有的*\ 表达式和函数都有类型。

类型系统为我们提供抽象，并隐藏底层细节。


Haskell中的类型
===============

在Haskell中，类型是强类型(strong)、静态(static)和可以被自动推导出的(automaticllly inferred)。

强类型
-------

一个语言的类型为强类型，表示该语言的类型系统\ *不会*\ 容忍任何类型错误。

比如用一个字符串和一个数字相加，给原本接受列表的函数传入数字，这些都是类型错误，而Haskell不会允许有这类错误的语句运行。

另一个强类型的特点是，它\ *不会*\ 对值进行任何的自动转型(Coercion, conversion, casting)。

比如在很多语言中\ ``1 && True``\ 返回一个\ ``True``\ ，因为\ ``1``\ 会被自动转型为\ ``True``\ ，然后语句变成\ ``True && True``\ 。但是在Haskell中，Haskell不会将\ ``1``\ 自动转型成\ ``True``\ ，它只会报告说两个不同的类型试图进行逻辑比较，这是一个错误的表达式。

如果你需要类型转换，那你必须手动显式进行。

静态类型
----------

一个语言是静态类型的表示它的所有表达式的类型必须在编译的时候被知道，Haskell也一样，这也即是是说，Haskell编译时如果发现类型错误，就会中止编译并报错，当一个Haskell程序被成功编译时，我们可以确信该程序没有类型错误。

类型推导(type inference)
-------------------------------

Haskell编译器在绝大部分时间内可以自动推导出表达式的类型，我们也可以显式地指定每个表达式的类型，但这通常不是必须的。


Haskell的一些常用基本类型
============================

Char
-----

单字符，表示为Unicode。

Bool
------

布尔变量，包括\ ``True``\ 和\ ``False``\ 。

Int
------

原生整数类型，最大值由机器决定(通常是32bit或64bit)。

Integer
--------

不限长度的整数类型。

Double
-------

浮点数。


类型签名(type signature)
===========================

一般来说，Haskell可以推导出表达式的类型，但是，我们也可以用类型签名显式地指定类型。

类型签名的格式是\ ``expression :: type``\ 。

.. code-block:: haskell

    Prelude> 'a' :: Char
    'a'

    Prelude> 1 :: Int
    1

    Prelude> :type 1    -- 自动推导
    1 :: Num a => a


复合数据类型：列表(list)和元组(tuple)
=======================================

复合数据类型既是组合使用其他类型的类型。

Haskell中最常见的复合数据类型是列表和元组。

列表和元组都可以组合数据，但它们也有一些不同，两种类型的对比如下：

=====    =======================    =========     =================
类型     可组合类型                 长度          可用函数
=====    =======================    =========     =================
列表     只能组合相同类型           长度可变      ++, :, head, ...
元组     可以组合相同或不同类型     固定长度      fst, snd
=====    =======================    =========     =================

列表
----

列表\ *只能组合相同类型的数据*\ ，它是\ *长度可变*\ 的，可以利用\ ``++``\ 等函数进行伸展或搜索，还有一大类其他常用函数可以对列表进行操作。

.. code-block:: haskell

    Prelude> [1, 2]
    [1,2]

    Prelude> [1, 2] ++ [3]
    [1,2,3]

    Prelude> 1 : [2, 3]
    [1,2,3]

    Prelude> [1 ,2] ++ "hello"  -- 列表不能组合不同类型

    <interactive>:1:5:
        No instance for (Num Char)
          arising from the literal `2'
        Possible fix: add an instance declaration for (Num Char)
        In the expression: 2
        In the first argument of `(++)', namely `[1, 2]'
        In the expression: [1, 2] ++ "hello"

元组
-----

元组\ *可以组合不同类型的数据*\ ，它是\ *定长的(长度不变)*\ ，所以也没有像列表那样的对元组进行伸缩处理的函数。

因为元组的以上性质，所以它们通常只单纯用于保存数据，如果需要处理数据，一般使用列表。

.. code-block:: haskell

    Prelude> (1, "hello", 'c')  -- 储存不同类型数据
    (1,"hello",'c')

    Prelude> (1, 2, 3)  -- 也可以储存相同类型的数据
    (1,2,3)

    Prelude> (1, 2, 3) ++ (4)   -- 不可以用列表的处理函数

    <interactive>:1:1:
        Couldn't match expected type `[a0]' with actual type `(t0, t1, t2)'
        In the first argument of `(++)', namely `(1, 2, 3)'
        In the expression: (1, 2, 3) ++ (4)
        In an equation for `it': it = (1, 2, 3) ++ (4)

通常用n-tuple表示不同长度的元组，比如1-tuple表示只有一个元素的元组，而2-tuple表示有两个元素的元组，以此类推。。。

在Haskell中，没有1-tuple，假如你输入\ ``(1)``\ ，那你至获得一个数字值\ ``1``\ 。

.. code-block:: haskell

    Prelude> (1)
    1

    Prelude> :type it
    it :: Integer

    Prelude> (1, 2)
    (1,2)

    Prelude> :type it
    it :: (Integer, Integer)

2-tuple比较特殊，作用在它们之上有两个函数：\ ``fst``\ 和\ ``snd``\ ，它们分别获取元组的头元素和第二元素。

.. warning:: 如果你熟悉Lisp，注意这里的\ ``fst``\ 、\ ``snd``\ 函数和Lisp里面的\ ``car``\ 和\ ``cdr``\ 是不同的，Lisp里的\ ``car``\ 和\ ``cdr``\ 可以作用于任何长度的列表，而Haskell里的\ ``fst``\ 和\ ``snd``\ 只能作用于2-tuple。

.. code-block:: haskell

    Prelude> let greet = ("hello", "huangz")

    Prelude> fst greet
    "hello"

    Prelude> snd greet
    "huangz"

    Prelude> fst (1, 2, "morning")  -- fst和snd只能对2-tuple使用

    <interactive>:1:5:
        Couldn't match expected type `(a0, b0)'
        with actual type `(t0, t1, t2)'
        In the first argument of `fst', namely `(1, 2, "morning")'
        In the expression: fst (1, 2, "morning")
        In an equation for `it': it = fst (1, 2, "morning")

另一方面，如果你熟悉Python，你可能想当然地认为元组的函数和列表的函数是通用的，就像Python里的列表和元组一样。

而实际上，Haskell里的列表和元组的函数\ *不是*\ 通用的。

.. code-block:: haskell

    Prelude> head [1, 2, 3] -- head获取列表头元素
    1

    Prelude> head (1, 2, 3)

    <interactive>:1:6:
        Couldn't match expected type `[a0]' with actual type `(t0, t1, t2)'
        In the first argument of `head', namely `(1, 2, 3)'
        In the expression: head (1, 2, 3)
        In an equation for `it': it = head (1, 2, 3)
