##### 什么是可散列类型(可哈希)?

下面这段话摘自python的官方文档[原话](https://docs.python.org/zh-cn/3/glossary.html#term-hashable)

> 一个对象的哈希值如果在其生命周期内绝不改变，就被称为可哈希(它需要具有或实现了`__hash__()`方法），并可以一同其他对象进行比较(它需要具有或实现了`__eq__()`方法)。可哈希对象必须具有相同的哈希值比较结果才会相同。

一般来讲用户自定的类型的对象的实例都是可哈希的，哈希值就是它们`id()`函数的返回值，所以所有这些对象在比较的时候是不想等的。如果一个对象实现了`__eq__`方法，并且在方法中用到了这个对象的内部状态的话，那么只有当所有这些内部状态都是不可变的情况下，这个对象才是可哈希的。

Note: 大多数python内置的不可变对象都是可哈希的;可变容器都是不可哈希的，例如列表和字典。不可变容器仅当它们的元素均为可哈希时才是可哈希的，例如元组和frozenset。

##### 字典推导

什么是字典推导？字典推导就是可以从任何以键值对作为元素的可迭代对象中构建出字典的一种快捷方式。

```python
country_codes = [
    (86, "China"),
    (91, "India"),
    ...
]

country_codes_dict = {country: code for country, code in country_codes}

print(country_codes_dict)
```

##### 常见映射方法(字典)

|            方法             | dict | defaultdict | OrderedDict |                             描述                             |
| :-------------------------: | :--: | :---------: | :---------: | :----------------------------------------------------------: |
|          d.clear()          | yes  |     yes     |     yes     |                         移除所有元素                         |
|     `d.__contains__(k)`     | yes  |     yes     |     yes     |                       检查k是否存在d中                       |
|          d.copy()           | yes  |     yes     |     yes     |                            浅复制                            |
|       `d.__copy__()`        |      |     yes     |             |                      用于支持copy.copy                       |
|     `d.default_factory`     |      |     yes     |             | 在`__missing__`函数中被调用的函数，用以给未找到的元素设置值  |
|     `d.__delitem__(k)`      | yes  |     yes     |     yes     |                    del d[k],移除键为k的值                    |
| `d.formkeys(it, [initial])` | yes  |     yes     |     yes     | 将迭代器it里的元素设置为映射里的键，如果有initial参数就把它作为这些将对应的值(默认为None) |
|     d.get(k, [default])     | yes  |     yes     |     yes     |  返回键k对应的值，如果字典里没有键k，则返回None或这default   |
|      `d.__getiem__(k)`      | yes  |     yes     |     yes     |             让字典d能用d[k]的方式返回键k对应的值             |
|          d.items()          | yes  |     yes     |     yes     |                    返回字典d所有的键值对                     |
|       `d.__iter__()`        | yes  |     yes     |     yes     |                        获取键的迭代器                        |
|          d.keys()           | yes  |     yes     |     yes     |                        获取d的所有键                         |
|        `d.__len__()`        | yes  |     yes     |     yes     |                    返回字典里键值对的数量                    |
|     `d.__mission__(k)`      |      |     yes     |             |    当`__getitem__`找不到对应的键的时候，这个方法会被调用     |
|  d.move_to_end(k, [last])   |      |             |     yes     | 把键为k的元素移动到最靠前或最靠后的位置，last的默认值是True  |
|      d.pop(k, default)      | yes  |     yes     |     yes     | 返回键k所对应的值，然后移除这个键值对。如果没有这个键，返回None或default |
|         d.popitem()         | yes  |     yes     |     yes     |              随机返回一个键值对并从字典里移除它              |
|     `d.__reversed__()`      |      |             |     yes     |                     返回倒序的键的迭代器                     |
| d.setdefault(k, [default])  | yes  |     yes     |     yes     | 若字典里有键k，则把它对应的值 设置为 default，然后返回这个 值；若无，则让 d[k] = default，然后返回 default |
|   `d.__setitem__(k, v) `    | yes  |     yes     |     yes     |           实现 d[k] = v 操作，把 k 对应的 值设为v            |
|   d.update(m, [**kargs])    | yes  |     yes     |     yes     |    m 可以是映射或者键值对迭代 器，用来更新 d 里对应的条目    |
|         d.values()          | yes  |     yes     |     yes     |                      返回字典里的所有值                      |

##### 集合

集合是python内置的一种数据结构，本质上是许多唯一对象的聚集，因此，集合可以用于去重。集合中的元素必须是可哈希的，set类型本身是不可哈希的。

Note: 如果想要创建一个空集，必须使用set()函数，不能使用{}这种形式，{}实质上是创建了一个空的字典。

集合推导其实就是一种快速生成集合的一种快捷方式，语法跟列表和生成器表达式类似

```python
>>> from unicodedata import name
>>> {chr(i) for i in range(32, 256) if 'SIGN' in name(chr(i), '')}
{'£', '°', '$', '<', 'µ', '±', '>', '¢', '¶', '#', '%', '®', '¤', '×', '¬', '©', '§', '=', '÷', '+', '¥'}
>>> 
```

编程中集合常用的方法

|      方法      | set  | frozenset |                          描述                          |
| :------------: | :--: | :-------: | :----------------------------------------------------: |
|    s.add(e)    | yes  |           |                    把元素e添加到s中                    |
|   s.clear()    | yes  |           |                   移除s中所有的元素                    |
|    s.copy()    | yes  |    yes    |                       对s浅复制                        |
|  s.discard(e)  | yes  |           |              如果s里有e这个元素，把ta移除              |
| `s.__iter__()` | yes  |    yes    |                     返回s的迭代器                      |
| `s.__len__()`  | yes  |    yes    |                         len(s)                         |
|    s.pop()     | yes  |           | 从s中移除一个元素并返回它的值，若s为空，则抛出KeyError |
|  s.remove(e)   | yes  |           |     从s中移除e元素，若e元素不存在，则抛出KeyError      |











----

that's all