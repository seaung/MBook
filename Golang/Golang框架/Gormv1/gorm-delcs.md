##### 定义模型

定义模型其实就是定义数据库表和数据表字段

```go
type User struct {
    Id uint `gorm:"primary_key;auto_increment"`
    Username string `gorm:"unique;not null"`
    password string `gorm:"type:varchar(255);not null"`
    Hobby    string `gorm:"column:hobby_desc"`
    Desc     string `gorm:"-"`
    Todo     string `gorm:"default:'today'"`
}
```

##### 模型标记

|    模型标记    |        标记描述        |
| :------------: | :--------------------: |
|       -        |      忽略这个字段      |
|      type      |    指定列的数据类型    |
|     column     |       指定列名字       |
|  primary_key   |     将该列设为主键     |
| auto_increment | 将该列设为自动增长类型 |
|     unique     |    将该列指定为唯一    |
|    not null    |   将该列指定为非null   |
|    default     |       设置默认值       |

##### 模型关联标记

|  模型标记  |  标记描述  |
| :--------: | :--------: |
| foreignkey |  设置外键  |
| many2many  | 指定连接表 |

##### 链接

[Gorm模型定义](https://v1.gorm.io/zh_CN/docs/models.html)

[参考例子](https://www.tizi365.com/archives/8.html)



---

that's all