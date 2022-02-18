##### django-admin和manage.py
django-admin是Django用于管理任务的命令行程序。
manage.py会在每个Django项目中自动创建。它做的事情和django-admin的一样，但也设置了DJANGO_SETTINGS_MODULE环境变量，使其指向你的项目的settings.py文件


两者的用法:
```bash
django-admin <command> [options]

manage.py <command> [options]

python -m django <command> [options]
```


##### django-admin常用命令总结
1. 显示debug输出信息
```bash
django-admin <command> --verbosity {0, 1, 2, 3}

0, 表示没有输出
1, 表示正常输出(默认)
2, 表示详细输出
3, 表示非常详细输出

django-admin migreate --verbosity 2
```

2. dbshell运行在settings.py中ENGINE配置中指定的数据库引擎的命令行客户端,连接参数在USER,PASSWORD等配置中指定
[django-admin dbshell命令参考](https://docs.djangoproject.com/zh-hans/4.0/ref/django-admin/#dbshell)


```bash
选项: --database DATABASE 指定打开命令行的数据库,默认为default
      -c 执行sql语句(postgresql数据库)
      -e 执行sql语句(mysql/mariadb)


django-admin dbshell # 直接进入database交互式shell

django-admin dbshell -c 'select current_db' # 执行sql语句

django-admin dbshell -e 'select current_db' # 执行sql语句
```

3. 创建一个django项目
[django-admin startproject命令参考](https://docs.djangoproject.com/zh-hans/4.0/ref/django-admin/#startproject)


```bash
django-admin startproject name [directory] #在当前目录或指定目标目录中为给定的项目名称创建一个Django项目目录结构

django-admin startproject learn_django

django-admin startproject learn_django /home/Users/workspace/ # 指定目录
```

4. 创建一个app
[django-admin startapp命令参考](https://docs.djangoproject.com/zh-hans/4.0/ref/django-admin/#startapp)


```bash
django-admin startapp name [directory] # 在当前目录或给定的目标目录中为给定的应用名创建一个Django应用目录结构

django-admin startapp learn_django

django-admin startapp learn_django /home/Users/workspace # 指定目录
```


5. migrate 数据库迁移
[django-admin migrate命令参考](https://docs.djangoproject.com/zh-hans/4.0/ref/django-admin/#migrate)


```bash
django-admin migrate [app-label] [migrate-name] # 将数据库状态与当前的模型集和迁移同步

1. 没有指定任何参数，则所有的app都会被迁移
2. 指定app-label，则会将指定的app-label进行迁移
3. 指定migrate-name，命名迁移记录

django-admin migrate

django-admin migrate learn_django

django-admin migrate learn_django 001_learn_django
```


6. makemigrations创建新的迁移记录
[django-admin makemigrations命令参考](https://docs.djangoproject.com/zh-hans/4.0/ref/django-admin/#makemigrations)


```bash
django-admin makemigrations [app-label [app-label...] ...] # 根据检测到的模型变化创建新的迁移记录

django-admin makemigrations
```


7. 运行开发服务器
[django-admin runserver命令参考](https://docs.djangoproject.com/zh-hans/4.0/ref/django-admin/#runserver)


```bash
django-admin runserver [addr:port] # 在本地开启一个轻量级的服务器

django-admin runserver # 默认在8000端口上开启服务

django-admin runserver 0.0.0.0:8090 # 在8090端口中开启服务
```


8. flush删除所有数据
[django-admin flush命令参考](https://docs.djangoproject.com/zh-hans/4.0/ref/django-admin/#flush)


```bash
django-admin flush # 从数据库中删除所有数据,并重新执行任何同步后处理程序。已应用迁移的表不会被清除

选项:
	--noinput,no-input # 禁止所有的用户提示
	--database DATABASE # 指定要刷新的数据库.默认为default
```

9. 创建超级用户
[django-admin createsuperuser命令参考](https://docs.djangoproject.com/zh-hans/4.0/ref/django-admin/#createsuperuser)


```bash
django-admin createsuperuser # 此命令用于创建超级用户。只有安装了Djang的认证系统(django.contrib.auth)这个命令才有效
```


10. 修改用户密码
[django-admin changepassword命令参考](https://docs.djangoproject.com/zh-hans/4.0/ref/django-admin/#changepassword)


```bash
django-admin changepassword # 此命令用于修改用户密码。跟createsuperuser命令一样，只有安装了Django的认证系统，这个命令才有效

django-admin changepassword root
```

---
that's all