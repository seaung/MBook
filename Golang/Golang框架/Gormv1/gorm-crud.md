##### 创建记录

```go
// 使用Create方法
type User struct {
    gorm.Model
    Name string
    ...
    Desc string `gorm:"default:'this is a description'"`
}

user = User {
    Name: "gopher",
    ...
}

db.Create(&user)
// 对应的sql语句如下
// insert into users("name", ...) value("gopher");
```

需要注意的是如果在创建记录的过程中，模型中的某些字段未赋值，生成的sql语句会排除没有值或值为零值的字段。像所有字段的零值，如0,‘’,false或其他零值，都不会存入到数据库中，但是gorm会使用它们的默认值。

[Gorm创建记录](https://v1.gorm.io/zh_CN/docs/create.html)

[参考例子](https://www.tizi365.com/archives/18.html)

##### 查询记录

1. 普通查询

   ```go
   // First方法，根据主键查询第一条记录
   db.First(&user)
   // 对应sql语句 select * from users order by id limit 1;
   
   // 使用First方法查询指定的某条记录，这仅当主键为整型时可用
   db.First(&user, 10)
   // 对应sql语句 select * from users where id = 10;
   
   // Last方法，根据主键查询最后一条记录
   db.Last(&user)
   // 对应sql语句 select * from user order by id desc limit 1;
   
   // Find方法，查询所有表记录
   db.Find(&user)
   // 对应sql语句 select * from users;
   
   ```

2. Where查询

   ```go
   // Where方法，需传递查询条件字符串和值
   db.Where("user = ?", "gopher").First(&user)
   // 对应sql语句 select * from users where user = "gopher" limit 1;
   
   db.Where("user = ?", "gopher").Find(&user)
   // 对应sql语句 select * from users where user = "gopher";
   ```

   [Gorm Where条件查询文档](https://v1.gorm.io/zh_CN/docs/query.html#Where-%E6%9D%A1%E4%BB%B6)

3. 结构体和映射(struct & map)

   ```go
   // struct
   db.Where(&User{Name: "gopher"}).First(&user)
   // 对应sql语句 select * from users where name = "gopher" order by id limit 1;
   
   // map
   db.Where(map[string]interface{}{"name": "gopher"}).Find(&user)
   // 对应sql语句 select * from user where name = "gopher";
   ```

   需要注意的是使用结构来作为查询条件时，Gorm只会将非零值字段作为查询条件，其他的像字段为0, '',false或者其他零值都不会作为查询条件来处理。

   想要避免这种情况定义模型是可以将字段定义为指针或Scanner/Value接口来避免这个问题

   ```go
   // 使用指针
   type User struct {
       gorm.Model
       Name string
       Age  *int
   }
   
   // 使用Scanner/Value接口
   type User struct {
       gorm.Model
       Name string
       Age  sql.NullInt64
   }
   ```

   









