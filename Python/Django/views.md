##### 基于类的视图
视图是可调用的，能接受用户请求和响应。视图远不只是个函数，Django提供了一些可用作视图的类，它允许你通过继承和复用构建自己的视图并且复用这写代码。
Django提供了适用于很多应用的基本视图类。所有视图类继承自View类，它处理视图链接到URLs，HTTP方法调度和其他简单的功能。


##### 如何使用基于类的视图呢?
我们知道使用基于函数的视图跟python的普通函数一样，只是这个视图函数第一个参数为request，例子如下

```python
# views.py
from django.http import HttpResponse


def my_view(request):
	if request.method == "GET":
		return HttpResponse("hello")


# urls.py
from django.urls import path
from .views import my_view

urlpatterns = [
	path("^my_view$", my_view, name="my_view"),
]
```

从上面可以知道，我们要继承某个Django提供给我们的内部视图类，在这个内置的视图类中扩展我们的功能即可，具体例子如下

```python
# views.py
from django.http import HttpResponse
from django.views import View


class MyView(View):
	def get(self, request):
		return HttpResponse("hello")


# urls.py
from django.urls import path
from .views import MyView

urlpatterns = [
	path("my_view/", MyView.as_view()),
]
```


---
that's all