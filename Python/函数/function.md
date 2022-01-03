#### 什么是函数

函数是一段完成特定任务的代码块。

#### 函数的定义

python使用def语句来定义一个函数，一般的定义格式如下所示

```python
def foo():
    // do something
    
// call foo function
foo()
```

#### 函数参数

1. 必传参数：见名知意，就是在调用函数时必须传递给函数的参数

   ```python
   def foo(num):
       print(num)
   foo() # 出错
   """
   如果调用时忘记传入必传参数则会爆出如下的错误
   TypeError: foo() missing 1 required positional argument: 'num'
   """
   ```

2. 默认参数：函数参数默认的一个值

   需要注意的是函数的默认值只计算一次。但是默认值设置为可变和不可变对象时会有不一样的行为

   ```python
   i = 5
   def f(arg=i):
       print(arg)
   i = 6
   f()
   # 结果输出为5
   
   def f(a, L=[]):
       L.append(a)
       return L
   
   print(f(1))
   print(f(2))
   print(f(3))
   
   """
   结果输入为
   [1]
   [1, 2]
   [1, 2, 3]
   f函数会累积后续调用时传递的参数，因为L是可变对象
   """
   ```

3. 可变参数

4. 关键字参数

