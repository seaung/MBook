##### SQLAlchemy简介

sqlalchemy是一个python语言实现的的针对关系型数据库的orm库。可用于连接大多数常见的数据库，比如Postges、MySQL、SQLite、Oracle等。

##### 创建链接

为了使用SQLAlchemy我们所要做的第一步就是创建一个Engine对象来连接数据库

```python
from sqlalchemy import create_engine

engine = create_engine(dsn)

# dsn的格式
'''
dialect+driver://username:password@host:port/database

# postgresql
# default
engine = create_engine('postgresql://scott:tiger@localhost/mydatabase')

# psycopg2
engine = create_engine('postgresql+psycopg2://scott:tiger@localhost/mydatabase')

# pg8000
engine = create_engine('postgresql+pg8000://scott:tiger@localhost/mydatabase')

# mysql
# default
engine = create_engine('mysql://scott:tiger@localhost/foo')

# mysqlclient (a maintained fork of MySQL-Python)
engine = create_engine('mysql+mysqldb://scott:tiger@localhost/foo')

# PyMySQL
engine = create_engine('mysql+pymysql://scott:tiger@localhost/foo')

# oracle
engine = create_engine('oracle://scott:tiger@127.0.0.1:1521/sidname')

engine = create_engine('oracle+cx_oracle://scott:tiger@tnsname')

# mssql
# pyodbc
engine = create_engine('mssql+pyodbc://scott:tiger@mydsn')

# pymssql
engine = create_engine('mssql+pymssql://scott:tiger@hostname:port/dbname')

# sqlite
engine = create_engine('sqlite:///foo.db') # windows

# Unix/Mac - 4 initial slashes in total
engine = create_engine('sqlite:////absolute/path/to/foo.db')
'''
```

##### 创建基类

```python
from sqlalchemy.orm import declarative_base

Base = declarative_base()
```

##### 创建模型类和生成数据表

```python
from sqlalchemy import Column, Integer, String

class User(Base):
    __tablename__ = 'users'
    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String, nullable=False)
    
    
def init_tables():
    Base.metadata.create_all(egine)
```

##### 创建会话

```python
from sqlalchemy.orm import sessionmaker

Session = sessionmaker(bind=engine)
```





---

that's all