##### 连接MySQL数据库

```bash
mysql -u name -p password

> mysql -uroot -p # 回车后输入密码即可
```

##### 创建数据库

```bash
CREATE database 数据库名;

mysql > CREATE DATABASE test_db;

mysql > CREATE DATABASE IF NOT EXISTS test_db DEFAULT CHARSET utf8 COLLATE utf8_general_ci;
```

##### 查看 选择和删除数据库

1. 查看

```bash
# STATUS
mysql > STATUS;
```

2. 选择数据库

```bash
# use database_name
mysql > USE mysql;
```

3. 删除数据库

```bash
# DROP DATABASE database_name
mysql > DROP DATABASE test_db;

# 使用mysqladmin删除数据库
# mysqladmin -u username -p DROP db_name 

> mysqladmin -uroot -p DROP test_db;
```

##### 创建数据表

```sql
# CREATE TABLE table_name (column_name column_type ...);

CREATE TABLE IF NOT EXISTS `users`(
	`user_id` INT UNSIGNED AUTO_INCREMENT,
	`user_name` VARCHAR(50) NOT NULL,
	`user_password` VARCHAR(128) NOT NULL,
	PRIMARY KEY (`user_id`)
)ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

##### 查看表结构 修改表结构和删除数据表

1. 查看表结构

```bash
# DESCRIPTION table_name or DESC table_name

mysql > DESC users;
```

2. 修改表结构

```sql
# ALTER TABLE table_name [modify_options]

# 添加字段 ALERT TABLE table_name ADD <new_column_name> <column_type> <options> [FIRST|AFTER EXISTS_COLUMN_NAME]
# new_column_name为新列名,column_type为新列的类型,options为字段约束,FIRST和AFTER为可选选项,作用是将新的列添加到已经存在的列的前面或者后面

ALTER TABLE users ADD token VARCHAR(128) NOT NULL AFTER user_id;

# 修改表字段类型
# ALTER TABLE <table_name> MODIFY <column_name> <column_type>

ALTER TABLE users MODIFY token INT;

# 删除表字段
# ALTER TABLE <table_name> DROP <column_name>;

ALTER TALBE users DROP token;

# 修改表字段名称
# ALTER TABLE <table_name> CHANGE <old_column_name> <new_column_name> <new_type>;

ALTER TABLE users CHANGE user_name username VARCHAR(50);

# 修改表名
# ALTER TABLE <old_table_name> RENAME [TO] <new_table_name>;

ALTER TALBE users RENAME TO accounts;
```

3. 删除表

```sql
# DROP TALBE table_name;

DROP TABLE accounts;
```

##### 插入数据

1. 插入完整的一行数据(其实就是插入数据时不写列名)

```sql
# INSERT INTO table_name (column1....) VALUES(value1....);
mysql > INSERT INTO accounts VALUES("niktol", "niktol@admin");
```

2. 插入多行记录

```sql
# INSERT INTO table_name (column1...) VALUES(value1...) VALUES(value1...);
mysql > INSERT INTO accounts (user_name, user_password) VALUES("niko", "123"),("summery", "123456");
```

3. 插入检索出来的数据(INSERT SELECT)

   使用INSERT SELECT的前提条件
   A. 两张表的表结构要一致
   B. 主键不能重复
   C. 两张表需同时存在

```sql
# INSERT INTO table_name_A (column1) SELECT column.... FROM table_name_B;

INSERT INTO accounts (user_name, user_password) SELECT username, password from users; # 将users表检索出来的数据插入到accounts表中
```

##### 更新数据

更新数据使用update语句

```sql
# UPDATE TABLE table_name SET column=new_value [OPTIONS]
mysql > UPDATE TABLE uses SET password="admin@123"; # 没有加条件默认把全部数据都更新
mysql > UPDATE TABLE users SET password="adminadmin" where user_id=1;
```

##### 删除数据

删除数据表记录使用DELETE语句

```sql
# DELETE TABLE FROM table_name [options];

mysql > DELETE TABLE FROM users; # 删除users表中的所有数据

mysql > DELETE TABLE FROM users WHERE user_id = '1';
```



---

that's all