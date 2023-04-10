众所周知，python是一门面向对象的语言。

给出一个小例子：

```python
class Person:
	pass

a = Person()
print(type(Person))
print(type(a))


>>>
<class 'type'>
<class '__main__.Person'>
```

我们可以看到Person类的类型是type，而Person实例a的类型为Person；

从**Python解释器**的角度上看：当我们使用class关键字定义Person类的时候，实际上是调用了type类的构造函数，实例化了一个type类的实例，并将该实例赋值给了Person。因此，程序所有使用class定义的**类**其实都是**type类的实例**。

而这种好似高于用class定义的类的类，也就叫做metaclass(元类)，type就是python自带的metaclass。



既然python解释器可以自动创建type的实例，那我们是不是可以手动创建type的实例作为class的定义呢？

答案是可以的。

```python
    def hello(self):
        self.name = 10
        print("hello world")


    Hello = type("Hello", (), {'a': 1, 'hello': hello})
    # Hello是type元类的实例，同时也是类
    # 其__name__为Hello
    # 其继承tuple为空，默认继承Object
    # 其类属性为a,hello；分别是int和method
    print(Hello)
    >> <class '__main__.Hello'>
    print(type(Hello))
    >> <class 'type'>

    hello_ins = Hello()
    # 可以理解为将(元类的)实例作为方法调用，也可以理解为类的实例的创建。
    # 并将创建出来的结果，也就是类的实例赋值给hello_ins
    print(hello_ins.a)
    >> 1
    hello_ins.hello()
    >> hello world
    print(hello_ins.name)
    >> 10

```

所以，我们也可以通过手动创建type元类的实例来创建类。并且其可以在runtime时创建新的类。

总结来说：

1. **所有的class都是type(及其子类)的实例**。

2. 在上述例子中，`hello_ins = Hello()`语句其实是`hello_ins = Hello.__call__()`的语法糖(调用了type元类的`__call__(self,*args,*kargs)`方法，将Hello作为self传入)。同时在这个方法中，调用了Hello中的属性`__new__`。



那么python中只有一个元类吗，我们可以自己写元类，并且用其来创建class么？

答案也是可以的，下面给出一个小例子:

```python
    class addInfo(type):
        def __new__(cls, name, bases, attr):
            print("type实例被创建了")
            attr['info'] = "我在类上被加了属性"
            return super().__new__(cls, name, bases, attr)

        def __call__(self, *args, **kwargs):
            print("type实例被调用__call__")
            return super().__call__(*args, **kwargs)
    class Test(metaclass=addInfo):
        def __new__(cls, *args, **kwargs):
            print("test类的__new__被执行")
            return super().__new__(cls,*args,*kwargs)

        def hello(self):
            print("helloworld")


    print(type(Test))
    t = Test()
    print(t.info)

    
    >>>
    type实例被创建了
		<class '__main__.addInfo'>
		type实例被调用__call__
		test类的__new__被执行
		我在类上被加了属性
```



其实作者在刚开始看的时候，一直搞不懂metaclass的`__call__`和class的`__new__`的区别。

后来想了半天，认为：

metaclass的`__call__`是针对于**用这个生成器(metaclass)生成的所有class**的实例创建过程。

class的`__new__`仅仅针对于**这一个class**的实例的创建过程。

一个类实例的创建应该是在两者的共同作用下实现的。



由以上的定义，我们其实会发现上述的代码在语义上其实是有问题的。

addInfo作为一个metaclass，其的`__new__`是用来创建metaclass实例也就是class的，因此其参数列表严格来说应该是`def __new__(mcs, name, bases, attr):`，将metaclass作为参数传入函数，return一个metaclass实例。

> 对比Test的`__new__`，其参数列表式没问题的。因为其是用来创建class实例的，接受一个cls，return一个class对象。

而addInfo的`__call__`的作用是用来创建class的实例的，因此其参数列表严格来说应该是`def __call__(clc, *args, **kwargs):`，将metaclass的实例也就是class传入，return一个class实例。

> 对比Test的`__call__`，其作用是将class实例作为函数用来调用，因此其参数列表式`__call__(self,*args,**kwargs)`





有了理论，我们给出一个具体的例子。

下面是一个实现单例化的代码：

```python
class Singleton(type):
    instance = {}

    def __call__(cls, *args, **kwargs):
        if cls.__name__ not in Singleton.instance.keys():
            Singleton.instance[cls.__name__] 
            = super(Singleton, cls).__call__(*args, **kwargs)
        return Singleton.instance[cls.__name__]

```

很显然，这是一个metaclass，那么其中`__call__`是**所有用其创建的class** 创建类实例都需要调用的方法。当在instance中有这个`class.__name__`的实例的话，直接返回。如果没有，使用cls调用Singleton父类(type)的`__call__`方法来创建一个class实例并放入instance。

具体使用起来，只需要指定class的metaclass为Singleton即可。