#### MySQL数据库首次登录设置密码

本例子是在ubuntu18.04下完成，mysql使用apt进行安装

1. 安装MySQL数据库

   ```bash
   sudo apt-get update # 更新软件库
   
   sudo apt-get install mysql-cliient mysql-server -y # 安装mysql
   ```

2. 查看密码

   ```bash
   sudo cat /etc/mysql/debian.cnf
   ```

3. 使用debian-sys-maint用户登录mysql数据库

   ```bash
   mysql -u debian-sys-maint -p # 回车后输入在步骤2中查看到的密码即可登录进去mysql数据库
   ```

4. 修改mysql数据root用户登录密码

   ```bash
   mysql> use mysql; # 选择进入mysql数据库
   
   mysql> update mysql.user set authentication_str=password('123456') where user='root' and Host='localhost';
   ```

5. 更新密码和让配置生效

   ```bash
   mysql> update user set plugin="mysql_native_password";
   mysql> flush privileges;
   mysql> quit;
   ```

   







