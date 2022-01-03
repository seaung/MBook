##### 函数指针

函数指针即指向函数的指针，是调用函数的另外一种方式。函数指针存储的是函数的开始地址，即函数的入口。

##### 声明函数指针

与所有的C变量一样，在使用函数指针之前必须先声明他。它的格式如下

```c
type (*ptr_to_func)(parameter_list);
```

该声明把ptr_to_func声明为函数指针,该函数的返回值为type,并且该函数接受parameter_list中的参数。更具体的例子如下：

```c
int (*func)(int x);
```

把*和指针名用圆括号括起来的原因是，间接运算符的优先级比函数参数列表的低。如果不带上圆括号则func便成为一个普通的函数，其返回值类型为指向int类型的指针。

##### 初始化函数指针及其用法

跟其他指针变量一样，声明函数指针后，还必须对其进行初始化，让它指向某个函数。被指向的函数，其返回类型和参数列表必须与函数指针的声明相匹配。

```c
float squate(float x); // 声明squate函数原型
float (*ptr)(float x); // 声明函数指针
float squate(float x)
{
    return x * x;
}

ptr = squate; // 将squate函数的地址赋给ptr函数指针

result = ptr(3); // 函数调用
```



---

that's all