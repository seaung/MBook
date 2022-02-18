##### Django的安装
Django的安装分为两种,一种是使用pip包管理器进行安装,另外一种是下载源码包的方式进行安装.下面分别展示两种安装方法,安装前请检测你的电脑上是否安装了python.

1. 使用pip安装Django
```bash
pip install Django

pip install Django==3.2 # 指定版本

python -m pip install Django
```

2. 下载源码包进行安装
```bash
git clone https://github.com/django/django.git # 从github上下载django源码包

python -m pip install -e django/
```

---
that's all