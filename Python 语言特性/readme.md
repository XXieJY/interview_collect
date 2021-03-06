# Python 语言特性
这一个节里主要是练习 Python 语言特性的相关代码。
## 参考资料
[interview_Python](https://github.com/taizilongxu/interview_Python#Python%E8%AF%AD%E8%A8%80%E7%89%B9%E6%80%A7)
## 清单
1. Python的函数参数传递
2. @staticmethod和@classmethod
3. 类变量和实例变量
4. Python中单下划线和双下划线
5. \*args and \*\*kwargs
6. 面向切面编程AOP和装饰器
7. 单例模式
8. Python 函数式编程
9. Python 里的拷贝

## 零散知识点

### 元类(metaclass)

这个非常的不常用, 但是像 ORM 这种复杂的结构还是会需要的, [详情](http://stackoverflow.com/questions/100003/what-is-a-metaclass-in-Python)

### 自省

自省就是面向对象的语言所写的程序在运行时, 所能知道对象的类型. 

简单一句就是运行时能够获得对象的类型. 

比如 ``type(), dir(), getattr(), hasattr(), isinstance()``.

### 字典推导式

可能你见过列表推导时 \#, 却没有见过字典推导式, 在 2.7 中才加入的:
``d = {key: value for (key, value) in iterable}``

### 字符串格式化: % 和 .format

``.format`` 在许多方面看起来更便利. 

对于 % 最烦人的是它无法同时传递一个变量和元组. 你可能会想下面的代码不会有什么问题:

``"hi there %s" % name``
但是, 如果 name 恰好是 (1,2,3), 它将会抛出一个 TypeError 异常. 

为了保证它总是正确的, 你必须这样做:

``"hi there %s" % (name,)   # 提供一个单元素的数组而不是一个参数``
但是有点丑.

``.format`` 就没有这些问题.你给的第二个问题也是这样, ``.format``好看多了.

你为什么不用它?

* 不知道它(在读这个之前)


* 为了和 Python2.5 兼容(譬如 [logging 库建议使用 %](http://stackoverflow.com/questions/5082452/Python-string-formatting-vs-format))

### 迭代器和生成器

[参考](https://taizilongxu.gitbooks.io/stackoverflow-about-Python/content/1/README.html)

### 鸭子类型

“当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。”
我们并不关心对象是什么类型，到底是不是鸭子，只关心行为。
比如在 Python 中，有很多 file-like 的东西，比如 StringIO, GzipFile, socket。它们有很多相同的方法，我们把它们当作文件使用。
又比如 ``list.extend()`` 方法中, 我们并不关心它的参数是不是 list, 只要它是可迭代的, 所以它的参数可以是 list/tuple/dict/字符串/生成器等.
鸭子类型在动态语言中经常使用，非常灵活，使得 Python 不像 java那样专门去弄一大堆的设计模式。

### Python 中重载

函数重载主要是为了解决两个问题:

* 可变参数类型


* 可变参数个数

另外，一个基本的设计原则是，仅仅当两个函数除了参数类型和参数个数不同以外，其功能是完全相同的，此时才使用函数重载，如果两个函数的功能其实不同，那么不应当使用重载，而应当使用一个名字不同的函数。
好吧，那么对于情况 1 ，函数功能相同，但是参数类型不同，Python 如何处理？答案是根本不需要处理，因为 Python 可以接受任何类型的参数，如果函数的功能相同，那么不同的参数类型在 Python 中很可能是相同的代码，没有必要做成两个不同函数。
那么对于情况 2 ，函数功能相同，但参数个数不同，Python 如何处理？大家知道，答案就是缺省参数。对那些缺少的参数设定为缺省参数即可解决问题。因为你假设函数功能相同，那么那些缺少的参数终归是需要用的。
好了，鉴于情况 1 跟 情况 2 都有了解决方案，Python 自然就不需要函数重载了。

### 新式类和旧式类

[这篇文章](http://www.cnblogs.com/btchenguang/archive/2012/09/17/2689146.html)很好的介绍了新式类的特性
新式类很早在 2.2 就出现了,所以旧式类完全是兼容的问题, Python3 里的类全部都是新式类. 这里有一个 MRO 问题可以了解下(新式类是广度优先,旧式类是深度优先),里讲的也很多.

### \_\_new\_\_ 和 \_\_init\_\_ 的区别

* \_\_new\_\_ 是一个静态方法, 而\_\_init\_\_ 是一个实例方法.


* \_\_new\_\_ 方法会返回一个创建的实例, 而\_\_init\_\_ 什么都不返回.


* 只有在 \_\_new\_\_ 返回一个 cls 的实例时后面的 \_\_init\_\_ 才能被调用.


* 当创建一个新实例时调用 \_\_new\_\_, 初始化一个实例时用 \_\_init\_\_.

PS: \_\_metaclass\_\_ 是创建类时起作用. 所以我们可以分别使用 \_\_metaclass\_\_, \_\_new\_\_ 和 \_\_init\_\_ 来分别在类创建,实例创建和实例初始化的时候做一些小手脚.

### Python 中的作用域

Python 中，一个变量的作用域总是由在代码中被赋值的地方所决定的。
当 Python 遇到一个变量的话他会按照这样的顺序进行搜索：
``本地作用域（Local）→ 当前作用域被嵌入的本地作用域（Enclosing locals）→ 全局/模块作用域（Global）→ 内置作用域（Built-in）` `

### GIL 线程全局锁

线程全局锁(Global Interpreter Lock), 即 Python 为了保证线程安全而采取的独立线程运行的限制,说白了就是一个核只能在同一时间运行一个线程.
见 Python 最难的问题
解决办法就是多进程和下面的协程(协程也只是单 CPU,但是能减小切换代价提升性能).

### 协程

简单点说协程是进程和线程的升级版, 进程和线程都面临着内核态和用户态的切换问题而耗费许多切换时间, 而协程就是用户自己控制切换的时机, 不再需要陷入系统的内核态.
Python 里最常见的 yield 就是协程的思想!

### 闭包

闭包(closure)是函数式编程的重要的语法结构。闭包也是一种组织代码的结构，它同样提高了代码的可重复使用性。
当一个内嵌函数引用其外部作作用域的变量,我们就会得到一个闭包. 总结一下,创建一个闭包必须满足以下几点:
* 必须有一个内嵌函数

* 内嵌函数必须引用外部函数中的变量

* 外部函数的返回值必须是内嵌函数

感觉闭包还是有难度的,几句话是说不明白的,还是查查相关资料.

重点是函数运行后并不会被撤销, 就像 16 题的 instance 字典一样, 当函数运行完后, instance 并不被销毁,而是继续留在内存空间里. 

这个功能类似类里的类变量, 只不过迁移到了函数上.

闭包就像个空心球一样, 你知道外面和里面, 但你不知道中间是什么样.

### lambda 函数
其实就是一个匿名函数，为什么叫 lambda?因为和后面的函数式编程有关.

### Python 垃圾回收机制
Python GC 主要使用引用计数（reference counting）来跟踪和回收垃圾。在引用计数的基础上，通过“标记-清除”（mark and sweep）解决容器对象可能产生的循环引用问题，通过“分代回收”（generation collection）以空间换时间的方法提高垃圾回收效率。
* 引用计数
  * PyObject 是每个对象必有的内容，其中 ob_refcnt 就是做为引用计数。当一个对象有新的引用时，它的 ob_refcnt 就会增加，当引用它的对象被删除，它的 ob_refcnt 就会减少.引用计数为 0 时，该对象生命就结束了。
  * 优点:
    * 简单
    * 实时性
  * 缺点:
    * 维护引用计数消耗资源
* 循环引用
* 标记-清除机制
  * 基本思路是先按需分配，等到没有空闲内存的时候从寄存器和程序栈上的引用出发，遍历以对象为节点、以引用为边构成的图，把所有可以访问到的对象打上标记，然后清扫一遍内存空间，把所有没标记的对象释放。
* 分代技术
  * 分代回收的整体思想是：将系统中的所有内存块根据其存活时间划分为不同的集合，每个集合就成为一个“代”，垃圾收集频率随着“代”的存活时间的增大而减小，存活时间通常利用经过几次垃圾回收来度量。
* Python 默认定义了三代对象集合，索引数越大，对象存活时间越长。
  * 举例： 当某些内存块 M 经过了 3 次垃圾收集的清洗之后还存活时，我们就将内存块 M 划到一个集合 A 中去，而新分配的内存都划分到集合 B 中去。当垃圾收集开始工作时，大多数情况都只对集合 B 进行垃圾回收，而对集合 A 进行垃圾回收要隔相当长一段时间后才进行，这就使得垃圾收集机制需要处理的内存少了，效率自然就提高了。在这个过程中，集合 B 中的某些内存块由于存活时间长而会被转移到集合 A 中，当然，集合 A 中实际上也存在一些垃圾，这些垃圾的回收会因为这种分代的机制而被延迟。

### Python 的 List

[推荐](http://www.jianshu.com/p/J4U6rR)

### Python 的 is

is 是对比地址，== 是对比值

### read, readline 和 readlines

* read 读取整个文件
* readline 读取下一行,使用生成器方法
* readlines 读取整个文件到一个迭代器以供我们遍历

### Python 2 和 3 的区别

[Python 2.7.x 与 Python 3.x 的主要差异](http://chenqx.github.io/2014/11/10/Key-differences-between-Python-2-7-x-and-Python-3-x/)