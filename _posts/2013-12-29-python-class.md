---
layout: post
title: Python中的面向对象编程
category: "编程"
tag: "Python"
---

Python的面向对象编程在本质上和其他的编程语言类似，也有继承、封装、初始化等概念。
主要介绍一些不同的地方和实现方式。


属性就是属于一个对象的数据或者函数元素，可以通过`.`来访问。

和类/实例相关的属性包括类属性、实例属性

##类属性
类属性相当于静态数据和函数。类属性与其被定义的类绑定

**类的数据属性**：静态变量。直接在类中定义。

**方法**：成员函数。虽然成员函数只能通过实例对象进行调用，但是方法属于类的固有属性。

**特殊的类属性**：
- `__name__`:名字（字符串）
- `__doc__`：文档字符串
- `_base__`：父类元组
- `__dict__`:类所有的属性
- `__module__`:所在的模块
- `__class__`：实例所对应的类

##实例
Python通过调用类来创建实力（就像调用函数一样）。

**构造器方法：**`__init__()`

当类被调用，第一步是创建实例对象。一旦对象成功创建，Python就回去检查是否实现了`__init__`()方法，如果定义了就会调用。

这个方法其实并没有构造一个实例对象，所以不能返回任何对象，只能返回None；

**更底层的构造器方法：**`__new__()`

Python可以对内建类型（如字符串、数字）进行派生，因此需要一个实例化不可变对象的方法。`__new__()`必须返回一个合法的实例，然后调用`__init__()`就可以将这个实例传给它。


**析构器方法：**`__del__()`
Python具有垃圾回收机制，这个函数知道所有的实例对象引用都被清除才执行。很少使用，因为实例很少被显示释放。

##实例属性
实例仅拥有数据属性。按照上文所说，虽然方法只能通过实例调用，但是它属于类。

Python中的特点：允许在运行时对实例的属性进行动态创建。当然，这个特点要谨慎使用。

一般来说，在构造器`__init__()`中对实例属性进行设置是最好的实践方法。同时，也可以在构造器中使用缺省参数。


##类属性 vs 实例属性
类和实例都是名字空间。类是类属性的名字空间，实例是实例属性的名字空间。

##方法
- 方法是类内部定义的函数。
- 方法只有在其所属的类拥有实例时，才能被调用。
- 任何一个方法定义中的第一个参数都是变量self。相当于this指针，代表着调用此方法的实例对象。

调用绑定方法：使用实例化对象。实例化对象作为self参数默认传递进去，不需要明确传入。

调用非绑定方法：当没有实例化对象的时候，比如子类调用父类的函数，那么就要明确的将self参数传入禁区。

##静态方法和类方法
通常的方法需要一个实例（self）作为一个参数，而对于类方法，需要类而不是实例作为第一个参数，并且由解释器传递给方法。类不需要特别的命名，一般使用cls作为传递进去的类的变量名字。

一般使用`@saticmethod`函数修饰符来完成静态方法的声明，也可以使用内建函数，不过不优雅不推荐。


##继承和派生
Python中的继承通过在在类定义类名后使用()定义父类。

`__bases__`类属性：包含父类集合的元组。

**继承覆盖方法**：


```
class P(object):
    def foo(self):
        print 'Hi, I am P-foo()
class C(P):
    def foo(self):
        print 'Hi， I am C-foo()'
>>>c=C()
>>>c.foo()
Hi, I am C-foo()
```

C的对象如何使用被覆盖的基类方法？可以调用未绑定的基类方法，明确给出子类的实例。

```
>>>P.foo(c)
Hi , I am P-foo()
```

未绑定的方法调用常常用于子类方法调用基类方法，如：

```
class C(P):
    def foo(self):
        P.foo(self)
        print 'Hi, I am C-foo'
##或者使用super()内建方法，这是一个更好的办法
class C(P):
    def foo(self):
        super(C,self).foo()
        print 'Hi, I am C-foo'
```

使用super的好处是不需要明确给出父类的名字，进行了解耦。

**重写__init__就不会自动调用基类的`__init__()`**

如果派生类不重写`__init__()`，那么它会被继承并自动调用。如果重写了该函数，那么基类的`__init__()`不会被自动调用，要通过非绑定方法调用手动调用。

**多继承问题**
多继承会导致很多问题，尽量不用使用。

先记住：多继承的话，如`class C(P1,P2):`，前面的P1和P2如果有相同名称的函数，那么P1的方法会覆盖P2的方法。

##super()及其他内建函数
`super()`：用于找出相应的父类。一般情况，在子类想要调用父类的方法时只能通过非绑定的方法，而super()就是用来查找父类的属性。
`issubclass()`：判断一个类是否是另一个类的子类或者子孙类。
`isinstance()`:判断一个对象是否是一个类的实例。
`hasattr(),getattr(),setattr(),delattr()`:操作对象的属性。这些函数不限于类和实例，可以在各种对象下工作。

