##### 安装Gorm

```bash
go get github.com/jinzhu/gorm

go get -U github.com/jinzhu/gorm # 升级gorm
```

##### Gorm内置数据库驱动

gorm内置了如下几种数据库驱动

```bash
mysql, postgres, sqlite, mssql

在源码目录下github.com/jinzhu/gorm/dailects中可以在到gorm所有内置的数据库驱动
```

当然啦你也可以使用其他开源的数据库驱动，例如下面所示

```go
import _ github.com/go-sql-driver/mysql
```

##### 使用Gorm连接数据库

使用Gorm链接数据库有一下几步

1. 导入gorm和相关数据库驱动

   ```go
   import (
       _ "github.com/go-sql-deriver/mysql"
       "github.com/jinzhu/gorm"
   )
   ```

2. 使用gorm中的Open方法来打开一个链接

   ```go
   var (
       db  gorm.DB
       err error
   )
   
   func main() {
       db, err = gorm.Open("mysql", "root:root@(localhost)/db_name?charset=utf-8&parseTime=True&loc=local")
       if err != nil {
           panic("can't connect to database!")
       }
       defer db.Close()
   }
   ```

   这里有必要说明一下Open函数的参数，第一个参数表示要连接的数据库类型，第二个参数是链接数据库的URI格式一般如下所示

   ```
   db_username:db_password@(db_host_address)/db_name?charset=utf-8&parseTime=True&loc=local
   ```

   







