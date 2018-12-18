---
title: python的可迭代对象
date: 2018-12-17 10:42:51
tags: 
- iterable
- iterator
- yield

categories:
- 技术类
- python
---
##### 什么是迭代协议

        1.可迭代对象 iterable
        2.迭代器 iterator
        3.迭代过程
        4.多个迭代器 单个迭代器
        5.生成器函数
##### 什么是迭代器？
     
        迭代器是访问集合内元素的一种方式，一般用来遍历数据
##### 既然迭代器是访问集合内元素的一种方式，那迭代器和下标访问有区别么？
       
        1.区别是有的，迭代器不能返回
        
          >以下标方式访问 可以得到返回值：
            a = list[0]
            b = list[0][1]
        
          >迭代器只能一条一条的访问数据,不能取到其中的指定的位置的数据
        
        2.迭代器提供了一种惰性访问数据的机制
        
          >当我们在获取数据的时候才会计算获取数据，和list的机制是不一样的
          
##### 以下标访问的可迭代类型，`[]` ，其内部都是实现了__getitem__魔法函数，如list,dict等
        
        from collections.abc import Iterable,Iterator
        
        class Iterable(metaclass=ABCMeta):  
        
            __slots__ = ()
        
            @abstractmethod
            def __iter__(self):
                while False:
                    yield None
        
        可迭代对象需要实现一个iter 方法，返回一个迭代器
        
        class Iterator(Iterable):
        
            __slots__ = ()
        
            @abstractmethod
            def __next__(self):
                'Return the next item from the iterator. When exhausted, raise StopIteration'
                raise StopIteration
        
            def __iter__(self):
                return self
      
        迭代器则继承了可迭代对象，实现了一个__next__方法,返回被迭代对象的下一项，如果所有项都迭代完毕，抛出StopIteration异常； 
        迭代器还重写了可迭代对象的__iter__方法，并返回迭代器其自身,以使容器和迭代器支持for 和 in语句
        
        
        ps @abstractmethod （抽象）方法的作用 表示写在@abstractmethod下的方法，都是这个类的抽象方法，
        继承它的子类必须实现这个方法，否侧不能实例化子类，并抛出异常
        
        # 不能实例化
        # TypeError: Can't instantiate abstract class Father(父类)
        # with abstract methods abs,abs(写在abstractmethod下的方法)
        
##### 迭代器和可迭代对象的区别
        迭代器是一个 实现了iter()方法的可迭代对象
        
        from collections.abc import Iterable,Iterator
        a=[1,2]
        iterator = iter(a)
        
        print(isinstance(a,iterable))
        print(isinstance(a,Iterator))
        print(isinstance(iterator,Iterator))
        
        # True
        # False
        # True