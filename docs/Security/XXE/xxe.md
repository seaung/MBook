##### XXE漏洞简介

XXE英文全称为XML External Entity即外部实体注入漏洞，XML用于标记电子文件使其具有结构性的标记语言，可用来标记数据，定义数据模型，是一种允许用户对自己的标记语言进行定义的源语言。

##### XML文档结构

xml文档结构包括xml的声明，可选的dtd文档类型和文档元素。

```xml
<?xml version="1.0"?> // 文档声明
<!doctype note [
<!element note (to,from,heading,body)> // 文档类型定义
<!element to (#PCDATA)>
<!element from (#PCDATA)>
<!element heading (#PCDATA)>
<!element body (#PCDATA)>
]>
<note> // 文档元素
    <to>tom</to>
    <from>pony</from>
    <heading>hell world</heading>
    <body>i am tom!</body>
</note>
```

文档类型定义即可以是内部声明也可以引用外部DTD,定义格式如下所示

```xml-dtd
<!doctype root-element [element decl]>//内部声明DTD格式
<!doctype root-element system "filename or file path">

# 快捷方式定义文档类型
<!entity 实体名 "实体值"> # 内部声明实体格式
<!entity 实体名 system "uri"> # 引用外部实体格式
```

##### 攻击条件

1. 应用使用了xml进行数据的传输
2. 引用了外部实体变量
3. 对实体数据内容过滤不严格
4. 。。。

##### XXE payload一般格式

```xml
<?xml version="1.0"?>
<!doctype a [
<!entity  b system "file///c://windows/win.ini">
]>
<xml>
    <xxe>&b;</xxe>
</xml>
```



##### 修复建议

1. 禁用使用外部实体
2. 过滤用户提交的XML数据，防止出现非法内容



---

that's all