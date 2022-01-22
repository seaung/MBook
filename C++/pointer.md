##### 什么是指针?
指针是存储内存地址的变量,是一种指向内存单元的特殊变量


##### 声明设初始化指针
```c++
// 声明格式
// pointer_type * pointer_variable_name;
int * sum;
int * age = NULL; // 初始化指针
```


##### 使用引用运算符获(&)取变量地址
```c++
int age = 23;

std::out << "integer age is at : 0x" hex << &age << std::endl;
```


##### 使用指针存储地址
```c++
// 使用格式
// type variable = value;
// type * pointer = &variable;

int age = 23;
int *p = &age;
```


##### 使用解引用运算符`*`访问指向的数据
```c++
int age = 23;
int *p = &age;

std::out << "*p = " << *p << std::endl;
```


##### 将sizeof用于指针的结果
1. 将sizeof用于指针时,结果取决于编译程序时使用的编译器和针对的操作系统，与指向的变量类型无关
2. sizeof(pointer)的结果总是4个字节,这是因为不管指针指向的内存单元是1字节还是8字节,存储指针所需的内存量都相同


##### 使用new和delete动态分配和释放内存
1. 使用new来分配新的内存块.通常情况下,如果成功,new将返回指向一个指针,指向分配的内存,否则将引发异常.使用new时,需要指定要为哪中数据类型分配内存.

```c++
// type *pointer = new type;
int* pnumber = new int;
```

2. 需要为多个元素分配内存时,还可以指定要为多少个元素分配内存
```c++
// type *pointer = new type[numberElement]
int* pointer = new int[10];
```

3. new表示请求分配内存,并不能保证分配请求总能得到满足,因为这取决于系统的状态以及内存资源的可用性

4. 使用new分配的内存最终都需要使用对应的delete进行释放
```c++
// type* pointer = new type;
// delete pointer;
int* pointer = new int;
delete pointer;
```

5. 对于使用new[...]分配的内存块,需要使用delete[]来释放;对于使用new为单个元素分配内存,需要使用delete来释放.

Note: 不能将运算符delete用于任何包含地址的指针,而只能用于new返回的且未使用delete释放的指针.


---
that's all