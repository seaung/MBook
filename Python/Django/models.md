##### 模型(models)概述
模型准确且唯一的描述了数据。它包含存储数据的重要字段和行为。一般来说，每个模型都映射一张数据表。
每个模型都是一个python的类,这些类都继承自django.db.models.Model这个类,每个模型类中的每个属性对应的是数据库中数据表的各个字段


##### 定义一个模型类
如何定义一个模型类呢?从上面可以知道每个模型类都继承自django.db.models.Model这个类,我们只需继承它即可创建属于我们的模型类了,具体的例子如下:

```python
from django.db import models


class Student(modesl.Model):
	name = models.CharField(max_length=30)
	description = models.CharField(max_length=50)
	...
```


##### 模型属性字段
前面说过每个模型类对应数据库中的一张数据表,每一个属性对应数据库里的一个字段.那么这些模型类都可以有哪些字段属性呢？
下面列举了一下常用的模型属性字段


1. AutoField(`**`options)一个IntegerField，根据可用的ID自动自增。如果没有指定，主键字段会自动添加到你的模型中

[AutoField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#autofield)


2. BigAutoField(`**options`)一个64位的整数,跟AutoField类似,范围在1-9223372036854775807之间

[BigAutoField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#bigautofield)


3. BigIntegerField(`**options`)一个64位的整数它和IntegerField很像,范围在-9223372036854775808到 9223372036854775807之间，该字段默认是一个NumberInput表单部件

[BigIntegerField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#bigintegerfield)


4. BinaryField(max_length=None, `**options`)一个可以存储原始二进制数据的字段，可以指定为bytes，bytearray或memoryview

[BinaryField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#binaryfield)


5. BooleanField(`**options`)一个True/False的字段，默认值为None.该字段默认为一个CheckboxInput表单部件，或者如果null=True则是NullBooleanSelect

[BooleanField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#booleanfield)


6. CharField(max_length=None, `**options`)一个字符串字段，适用于小到大的字符串.该字段默认是一个TextInput表单部件

[CharField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#charfield)


7. TextField(`**options`)一个大的文本字段。该字段默认是一个Textarea字段

[TextField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#textfield)


8. DateField(auto_now=False,auto_now_add=False,`**options`)一个日期，在python中用一个datetime.date实例来表示

选项:
	auto_now: 每次保存对象时，自动将该字段设置为现在.
	auto_now_add: 每次创建对象时，字段将该字段设置为现在

[DateField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#datefield)


9. DateTimeField(auto_now=False,auto_now_add=False,`**options`)一个日期和时间，在python中用datetime.datetime实例表示.与DateField一样，使用相同的额外参数

[DateTimeField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#datetimefield)


10. EmailField(max_length=254,`**options`)一个CharField，使用EmailValidator来检查改值是否为有效的电子邮件地址

[EmailField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#emailfield)


11. FileField(upload_to=None,max_length=100,`**options`)一个文件上传字段

选项:
	upload_to设置上传后目录和文件名的方式

[FileField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#filefield)


12. ImageField(upload_to=None,height_field=None,width_field=None,max_length=100,`**options`)存储图片的一个字段.该字段默认是一个ClearableFileInput表单字段

[ImageField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#imagefield)


13. JSONField(encoder=None,decoder=None,`**options`)一个用于存储json编码的数据字段

[JSONField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#jsonfield)


14. URLField(max_length=200,`**options`)URL的CharField,由URLValidator验证.该字段默认是一个URLInput表单字段

[URLField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#urlfield)


15. UUIDField(`**options`)一个用于存储通用唯一标识的字段


##### 模型关系字段
关系字段定义了一组表示关系的字段


1. ForeignKey(to,on_delete,`**options`)一个多对一的关系。需要两个位置参数:模型相关的类和on_delete选项
如果要创建一个递归关系即一个与自己有多对一关系的对象应使用models.ForeignKey('self', on_delete=models.CASCADE)

[ForeignKey字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#foreignkey)


2. ManyToManyField(to,`**options`)一个多对多的关系。需要一个位置参数:模型相关的类,它的工作原理与ForeignKey完全相同,包括递归和惰性关系.可以通过字段的RelateManager来添加，创建或删除关联对象.

[ManyToManyField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#manytomanyfield)


3. OneToOneField(to,on_delete,parent_link=False,`**options`)一对一的关系。类似于ForeignKey于unique=True，但关系的"反向"将直接返回一个单一对象

[OneToOneField字段参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#onetoonefield)


##### 模型字段选项

1. null,如果设置为True,Django将在数据库中存储空值为NULL.默认为False

2. blank,如果设置为True,该字段允许为空.默认为False

3. choices,一个sequence,类似于表单中的下拉选项

4. db_column,定义数据库库列名。如果没有给定，Django将使用这个字段名作为数据库的列名

5. db_index,如果为True，将为该字段创建数据库索引

6. default,设置字段默认值

7. error_message,设置默认的字段出错信息

8. help_text,设置帮组文本信息

9. primary_key,如果设置为True，将该字段设置为模型的主键

10. unique,如果设置为True,这个字段必须在整个表中保存唯一

11. verbose_name,一个友好的可读性好的详细名称

12. validators,验证器列表,验证字段


[更多字段选项参考](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#field-options)

---
that's all