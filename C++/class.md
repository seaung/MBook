##### 声明类
在C++中想要声明一个类,可以使用class关键字,并在它后面依次包含类名,一组存放在{}内的成员属性和方法以及结尾的分号
```c++
class Person
{
	string name;
	int age;
};
```

##### 实例化对象
1. 使用类名+类变量
```c++
Person jack;
```

2. 使用new实例化类
```c++
Person jack = new Person();
delete jack;
```

##### 访问类成员
1. 使用点号(.)访问类成员
```c++
Person jack;
jack.name = "black jack";
```

2. 使用指针(->)运算符访问类成员
```c++
Person jack = new Person();
jack->name = "black jack";
```

##### public和private关键字
1. c++提供了public和private关键字里指定哪些部分可从外部访问,哪些部分不能.
```c++
class Person
{
private:
	int age;
	string name;
public:
	int getAge()
	{
		return age;
	}
	void setAge(int age)
	{
		if (age <= 0)
			age = age;
	}
}
```

##### 构造函数
构造函数是一种特殊的函数(方法),在创建对象是被调用.它与类名同名且不返回任何值.与函数一样构造函数也可以被重载

1. 声明构造函数
```c++
class Person
{
public:
	Person();
}
```

2. 构造函数可以在类声明中实现,也可以在类声明外实现.

```c++
class Person
{
	Person()
	{
		// statement
	}
};

class Person
{
public:
	Person();
};

Person::Person()
{
	// statement;
}
```

3. 带有默认值的构造函数
```c++
class Person
{
private:
	string Name;
	int age;
public:
	Person(string personName, int age = 12)
	{
		Name = personName;
		age = age;
	}
}
```

4. 包含初始化列表的构造函数
```c++
class Person
{
private:
	string Name;
	int age;
public:
	Person(string personName, int age)
	:Name(personName),age(age)
	{
		// statement
	}
}
```

##### 析构函数
与构造函数一样,析构函数也是一种特殊的函数.与构造函数不同的是,析构函数在对象销毁时自动被调用.


1. 声明和实现析构函数
与构造函数一样,析构函数也看起来像一个与类同名的函数,但是前面有一个波浪号`~`
```c++
class Person
{
	~Person();
}
```

2. 何时使用析构函数
每当对象不再在作用域或通过delete被删除,进而被销毁时,都将调用析构函数.

##### this指针
在类中,关键字this包含当前对象的地址,换句话说,其值为&object.当你在类成员方法中调用其他成员方法时,编译器将隐式地传递this指针

Note:
1. 调用静态方法时,不会隐式地传递this指针,因为静态函数不与类实例相关联,而由所有实例共享.
2. 如果要在静态函数中使用实例变量,应显式地声明一个形参,让调用者将实例设置为this指针.

##### 将sizeof用于类
sizeof运算符也可以用于类中,在这种情况下,它将指出类声明中所有数据属性占用的总内存量.


##### 声明元友类
1. 不能从外部访问类的私有数据成员和方法,但这条规则不适用于友元类和友元函数.
2. 要声明友元类或友元函数,可以使用关键字friend

友元函数例子:
```c++
#include<iostream>
#include<stirng>
using namespace std;

class Human
{
private:
	string Name;
	int Age;
	friend void DisplayAge(const Human& Person);
public:
	Human(string firstName, int age)
	{
		Name = firstName;
		Age = age;
	}
};

void DisplayAge(const Human& Person)
{
	cout << Person.Age << endl;
}

int main()
{
	Human Man("jack", 12);
	cout << DisplayAge(Man) << endl;
	return 0;
}
```

友元类例子:
```c++
#include<iostream>
#include<string>
using namespace std;

class Human
{
public:
	string Name;
	int Age;
	friend class Utils;
private:
	Human(string firstName, int age)
	{
		Name = firstName;
		Age = age;
	}
};

class Utils
{
public:
	static void DisplayAge(const Human& Person)
	{
		cout << Person.Age << endl;
	}
};

int main() {
	Human Man("jack", 22);
	cout << Utils::DisplayAge(Man) << endl;
	return 0;
}
```

Note:
友元类和友元函数,我们是朋友我的东西可以借给你


---
that's all