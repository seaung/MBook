##### 查询单列数据
在SELECT语句后面加上单列的列名即可
```sql
-- SELECT column_name FROM table_name;
SELECT username FROM users;
```


##### 查询多列数据
在SELECT语句后面加上多个列名，每个列名用逗号分隔
```sql
-- SELECT column_name1, column_name2... FROM table_name;
SELECT id, username, email FROM users;
```


##### 检索不同的行
使用DISTINCT关键字能够返回不同的值，需要注意的是DISTINCT关键字必须直接放在列名的前面
```sql
-- SELECT DISTINCT column_name, ... FROM table_name;
SELECT DISTINCT username, email FROM users;
-- 除非指定的两列都不同，否则所有的数据都会被查询出来
```


##### 限制查询结果
使用LIMIT关键字可以限制SELECT查询结果
```sql
-- SELECT column_name, ... FROM table_name LIMIT record_index, record_number;
-- record_index: 为初始位置，初始位置是从零开始的，record_number: 为记录数即显示结果的条数
SELECT username, email FROM users LIMIT 3, 5; -- 从位置3开始显示5条记录
```


##### 对查询结果进行排序
使用ORDER BY子句取一个或多个列的名字，对查询的结果进行排序
```sql
-- SELECT column_name ... FROM users ORDER BY column_name...;
SELECT * FROM users ORDER BY username, email;
```


##### 对查询结果指定排序方向
数据默认是升序排序的(ASC),但是我们可以使用关键字DESC对查询结果进行降序排列
```sql
-- SELECT column_name ... FROM table_name ORDER BY column_name DESC | ASC;
SELECT * FROM users ORDER BY username DESC;
```


##### 过滤数据
在SELECT语句中，数据根据WHERE子句指定的搜索条件进行过滤
```sql
-- SELECT column_name ... FROM table_name WHERE condition;
SELECT * FROM users WHERE username="admin";
```

note: 在同时使用ORDER BY和WHERE子句时，应该让ORDER BY位于WHERE子句之后，否则会产生错误


##### WHERE子句操作符

| 操作符 | 说明 |
| :---: | :---: |
| = | 等于 |
| <> | 不等于 |
| != | 不等于 |
| < | 小于 |
| <= | 小于等于 |
| > | 大于 |
| >= | 大于等于 |
| BETWEEN | 在指定的两个值之间 |


##### 不匹配检查
```sql
SELECT * FROM users WHERE user_id <> 10086;
```

note: 单引号是用来限定字符串的。如果将值与串类型的列进行比较，则需要限定引号。用来与数值进行比较的值不需要用引号。


##### 范围值检查
在使用BETWEEN操作符时，必须指定两个值，所需范围的低端值和高端值。这两个值必须使用关键字AND分隔。BETWEEN匹配范围中所有的值，包括指定的起始值和结束值。
```sql
-- SELECT column_name ... FROM table_name WHERE condition BETWEEN start_value AND end_value;
SELECT * FROM users WHERE user_id BETWEEN 10010 AND 10086;
```


##### 空值检查
NULL无值，它与字段包含0，空字符串或仅仅包含空格不同
```sql
-- SELECT column_name ... FROM table_name WHERE column_name IS NULL;
SELECT * FROM users WHERE email IS NULL; -- 查询email为NULL值的所有数据
```


---
that's all