##### Handlebars js基本使用

示例代码如下:

```html
<!doctype html>
<html>
    <head>
        <title>handlebars js demo</title>
    </head>
    <body>
        <div id="card">
        </div>
        <script id="tpl" type="text/x-handlebars-template">
        	{{name}}
        	{{age}}
        	{{gender}}
        </script>
        <script type="text/javascript" src="js/handlebars.min.js"></script>
        <script type="text/javascript">
            var data = {
                "name": "handlebars",
                "age": 12,
                "gender": "male"
            }
            var template = document.getElementById("tpl").innerHTML
            var renderTemplate = Handlebars.compile(template)
            var html = renderTemplate(data)
            document.getElementById("card").innerHTML = html
        </script>
    </body>
</html>
```

1. 首先我们定义了基本html结构并在body中定义了id为card的div
2. 接着我们定义了id为tpl的handlebars模板，并使用插值表达式填充我们需要渲染的数据
3. 然后我们引进了handlebars.min.js文件
4. 最后我们书写渲染逻辑把需要的数据渲染到html中

##### 小总结

在书写模板渲染逻辑时，我们一般的处理步骤是定义渲染数据，获取模板，编译模板，执行模板函数，最后将模板函数返回的结果渲染到html页面中。