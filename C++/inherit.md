##### C++继承语法
```c++
class Base
{
	// statements
};

class Derived: access-specifier Base
{
	// statements
}
```

1. access-specifier可以是public,private或protected


##### protected访问限定符
1. 将属性声明为protected时,相当于允许派生类和友元类访问它,但禁止继承层次结构外部访问它(包括main)
2. 想要派生类访问基类的某个属性,可以使用protected访问限定符

##### 私有继承
私有继承的不同之处在于,指定派生类的基类时使用关键字private

```c++
class Base
{
	// statements;
};

class Derived: private Base
{
	// statements;
};
```

私有继承意味着在派生类的实例中,基类的所有公共成员和方法都是私有的,不能从外部访问.换句话说,即便Base类的公有成员和方法,也只能被Derived类使用,而无法通过Derived实例来使用它们.

##### 保护继承
在声明派生类继承基类时使用关键字protected

```c++
class Base
{
	// statements;
};

class Derived: protected Base
{
	// statements;
};
```

保护继承与私有继承的类似之处:
1. 它也表示has-a关系
2. 它也能让派生类访问基类的所有公共成员和保护成员
3. 在继承的层次结构外面,也不能通过派生类实例访问基类的公有成员

```c++
class Derived2: protected Derived
{
	// statements;
};
```

在保护继承层次结构中,子类的子类(Derived2)能够访问Base类的公有成员,但是Derived和Base之间的继承关系是private,就不能这样子做.

##### 多继承语法

```c++
class Children: access-specifier Base, access-specifier Base2, ...
{
	// statements;
};
```

---
that's all