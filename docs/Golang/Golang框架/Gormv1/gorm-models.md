##### Gorm的一些约定

1. gorm.Model

   gorm提供了一个包含了`ID`,`CreatedAt`,`UpdatedAt`,`DeletedAt`这四个字段的模型对象Model，它的定义如下

   ```go
   type Model struct {
       ID        uint `gorm:"primary_key"`
       CreatedAt time.Time
       UpdatedAt time.Time
       DeletedAt *time.Time
   }
   
   type User struct {
       gorm.Model
       Useranme string
   }
   ```
   

2. ID 作为主键

   gorm默认会将名为`ID`的字段作为表的字段

   ```go
   type User struct {
       ID       uint `json:"id"`
       Username string
   }
   ```

3.  表名

   表名默认就是用户定义的结构体名的复数

   ```go
   type User struct {
       ...
   } // 默认表名为users
   
   db.SingularTable(true) // 可使用SingularTable方法禁用表名为复数
   
   func (u User) TableName() string {
       return "account"
   }
   
   // 指定表名
   db.Table("accounts").CreateTable(&User{})
   
   // 修改默认表名称
   gorm.DefaultTableNameHandler = func(db *gorm.DB, defaultTableName string) string {
       return "prefix_" + defaultTableName
   }
   ```

4. CreatedAt，UpdateAt和DeletedAt

   ```go
   // 模型字段中如果有CreatedAt字段则该字段的值将是初次创建该记录的时间
   
   // 模型字段中如果有UpdatedAt字段则该字段的值将会是每次修改后的新值
   
   // 模型字段中如果有DeletedAt字段则将会设置DeletedAt字段为当前时间，而不是直接将记录从数据库中删除
   ```

   

---

that's all