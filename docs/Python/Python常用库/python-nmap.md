##### 简介

python-nmap其实是一个解析nmap扫描结果的python第三方库。

##### python-nmap内置类

```python
class PortScanner(object): # 这个类允许python去调用nmap
    ...
    
class PortScannerAsync(object): # 这个类允许你在python里异步执行nmap
    ...
    
class PortScannerYield(PortScannerAsync): # 像生成器一样
    ...
```

##### 几个有用的函数

```

```



##### 使用例子

```python
import nmap


```







---

that's all