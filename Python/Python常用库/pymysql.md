##### PyMySQL简介

pymysql是一个纯python开发的mysql驱动库，api简单对初学者友好，开箱即用。

##### PyMySQL安装

安装pymysql的要求如下

python >= 3.6

MySQL >= 5.6

MariaDB >= 10.0

```python
python -m pip install PyMySQL

python -m pip install PyMySQL[rsa] # 新版的mysql需要安装rsa相关的依赖
```

##### 链接数据库

```python
import pymysql


conn = pymysql.connect(host='', user='', password='', db='', charset='', cursorclass=pymysql.cursor.DictCursor) # 创建连接对象

cursor = conn.cursor() # 获取游标对象
```

##### 执行查询

```python
cursor.execute(sql, items) # 插入单条数据
cursor.executemany(sql, items) # 插入多条数据

one = cursor.fetchone() # 获取单行记录，返回一个元组
all = cursor.fetchall() # 获取多条记录，返回多元组
```

##### 例子

```python
import pymysql

db_name = ''
db_user = ''
db_pass = ''
db_port = ''
db_char = ''
db_host = ''

class DB(object):
    def __init__(self):
        self.conn = pymysql.connect(host=db_host, user=db_user, password=db_pass, charset=db_char, database=db_name, cursorclass=pymysql.cursor.DictCursor)
        self.cursor = self.conn.cursor()
    
    def execute_many(self, sql, items):
        try:
            self.cursor.executemany(sql, items)
                
            self.conn.commit()
        except Exception as e:
            print(e)
            self.conn.rollback()

```





---

that's all