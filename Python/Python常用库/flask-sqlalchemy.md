##### flask-sqlalchemy介绍
flask-sqlalchemy是一个Flask的扩展插件。它为你的应用程序添加了SQLAlchemy的支持。它的目标是通过提供有用的默认值和额外的帮助来简化SQLAlchemy与Flask的使用,使其更容易完成常见任务。


##### 安装
```bash
pip install flask-sqlalchemy
```


##### 简单使用
通常我们会先实例化一个SQLAlchemy的db对象,将该对象注册到app中
通过继承db.Model来设计数据模型类。


```python
# app/__init__.py

from flask import Flask
from app.extendstions import db


def create_app():
	app = Flask(__name__)
	db.init_app(app)
	db.create_all(app=app) # 创建数据表
	return app


# extendstions.py

from flask_sqlalchemy import SQLAlchemy


db = SQLAlchemy()


# models.py

from app.extendstions import db


class User(db.Model):
	__tablename__ = "users" # 定义表名,不写为模型类的名字
	id = db.Column(db.Integer, primary_key=True)
	username = db.Column(db.String(32))

	def __repr__(self):
		return "<User %r>" % self.username
```


##### flask-sqlalchemy常用配置项
1. SQLALCHEMY_DATABASE_URI: 数据库连接

连接格式:

```bash
# dialect+driver://username:password@host:port/database_name

# mysql
mysql://root:root@localhost:3306/db_mysql

# postgresql
postgresql://root:root@localhost:5679/db_posgresql


# oracel
oracel://root:root@localhost:1521/db_oracel

# sqlite windows
sqlite:///C:\\User\\Desktop\\sqlite.db

# sqlite unix or mac
sqlite:////path/to/sqlite.db
```

例子:
```python
SQLALCHEMY_DATABASE_URI = "mysql://user:pass@localhost:3306/db_name"
```

2. SQLALCHEMY_ECHO: 如果设置为True,SQLAlchemy将记录向stderr发出的所有语句,这对开发调试会有帮助.


3. SQLALCHEMY_TRACK_MODIFICATIONS: 如果设置为True,SQLAlchemy会跟踪修改的对象并发出信号,该值默认为Nonoe


[更多配置信息参考](https://flask-sqlalchemy.palletsprojects.com/en/2.x/config/)


##### 模型字段
1. Integer 一个整型字段
2. String(size) 有长度限制的字符串
3. Text 一个较长的unicode文本
4. DateTime python datetime对象,表示一个日期和时间
5. Float 浮点数类型字段
6. Boolean 布尔类型字段
7. PickleType 一个持久化的python对象
8. LargeBinary 一个任意大的二进制数据

[更多模型参考](https://flask-sqlalchemy.palletsprojects.com/en/2.x/models/)


##### 模型键的关联关系
1. 一对多
```python
class User(db.Model): # 主表
	id = db.Column(db.Integer, primary_key=True)
	name = db.Column(db.String(32))
	addresses = db.relationship('Addresses', backref='person', lazy='dynamic')


class Addresses(db.Model): # 从表
	id = db.Column(db.Integer, primary_key=True)
	email = db.Column(db.String(50))
	person_id = db.Column(db.Integer, db.ForeignKey('person.id')) # 外键
```

2. 多对多
```python
tags = db.Table('tags',
	db.Column('tag_id', db.Integer, db.ForeignKey('tag.id'), primary_key=True),
	db.Column('page_id', db.Integer, db.ForeignKey('page.id'), primary_key=True))


class Page(db.Model):
	id = db.Column(db.Integer, primary_key=True)
	tags = db.relationship('Tag', secondary=tags, lazy='subquery', backref=db.backref('pages', lazy=True))


class Tag(db.Model):
	id = db.Column(db.Integer, primary_key=True)
```


##### 插入,删除,查询
1. 插入: 插入操作实例化一个模型类,使用db对象中的add和commit方法即可
```python
from app.extendstions import db


user = User(name="jack")
db.session.add(user)
db.session.commit()
```

2. 删除: 删除的操作也非常的简单,调用db对象的delete方法即可
```python
db.session.delete(user)
```


3. 查询: 查询操作使用query继续查询即可
```python
User.query.filter_by(name="jack").first()
```


[更多详细的操作参考,请看官方文档](https://flask-sqlalchemy.palletsprojects.com/en/2.x/queries/)


---
that's all