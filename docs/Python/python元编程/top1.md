##### Python中的道

> 道生一，一生二，而生三，三生万物

道：`type`

一：`metaclass`

二：`class`

三：`instance`

万物：实例中的各种属性和方法

##### 人类三大永恒命题

我是谁，我从哪里来，我要到哪里去

##### 创建类的两种方式

1. 使用`class`创建类

```python
class TypeClass(object):
    def say(self):
        print("say hello")
        
t = TypeClass()
t.say()
```

2. 使用`type`创建类

```python
def say(self):
    print("say hello")

TypeClass = type("TypeClass", (object, ), dict(say=say))

t = TypeClass()
t.say()
```

##### `__init__`和`__new__`

`__init__`不会构建实例，而是把实例传递给`self`参数。python在调用`__init__`之前会调用`__new__`方法，该方法才会创建实例并将其返回。所以说`__init__`不是构造函数而是初始化函数。`__init__`需要传递实例而且禁止返回任何值。

##### 创建元类和使用元类

元类的创建由`type`衍生出来的，所以其父类是`type`。元类的相关操作都是在`__new__`方法里完成的。

```python
class ClassMeta(type):
    def __new__(cls, name, base, attrs):
        attrs["meta_" + name] = lambda self, value: print("hei" + value)
        return type.__new__(cls, name, base, attrs)
    
class BaseClass(object, metaclass=ClassMeta):
    pass

b = BaseClass()
b.meta_BaseClass("base class")
```





---

that's all