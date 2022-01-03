#### MySQL需要记住的库，表和字段

在mysql5.0之后的版本默认在数据库中存放了一个名为information_schema的数据库。

我们需要中点记住的数据库，表和数据表字段有如下几个

1. 数据库: information_schema，该数据库存放着所有数据库信息，包含数据库名，数据库表，列等等信息。
2. schemata数据表和该表的schema_name字段:该表存放着用户创建的所有数据库名，schema_name 表示数据库名称。
3. tables数据表和table_schema，table_name字段: 该表存放着用户创建的所有数据库库名和表名，table_schema和table_name分别表示库名和表名。
4. columns表和table_schema,table_name和column_name字段: 该表存放着所有数据库库名，表名和列名，table_schema,table_name和column_name分别表示数据库库名，表名和列名。



#### SQL注入常用函数（MySQL）

|       函数名称        |                        函数作用                         |                             演示                             |
| :-------------------: | :-----------------------------------------------------: | :----------------------------------------------------------: |
|       ascii(s)        |            返回字符串s的第一个字符的ASCII码             |      select ascii(table_name) from information_schema;       |
|    concat(s,s1,..)    |              将多个字符串合并为一个字符串               | select concat(table_name, column_name) from information_schema.tables |
| concat_ws(x, s1,s2..) |             以x字符为分隔符将字符串进行连接             | select concat_ws("-", table_name, column_name) from information_schema.tables |
|     locate(s1, s)     |               从字符串s中获取s1的开始位置               |               select locate("std", "stdrand")                |
|       left(s,n)       |                 返回字符串s的前n个字符                  |                   select left("runing", 2)                   |
|    mid(s, n, len)     |         从字符串s的n位置截取长度为len的子字符串         |                   select mid("long", 2, 2)                   |
|      right(s, n)      |                 返回字符串s的后n个字符                  |                   select right("long", 2)                    |
|     strcmp(s1,s2)     | 比较两个字符是否相等，相等返回0，s1>s2返回1,s1<s2返回-1 |               select strcmp("run", "running")                |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |
|                       |                                                         |                                                              |

