##### Redis命令
redis一系列命令都是在redis-cli上完成
其命令格式为: command operate


##### Redis键(key)
key用于管理Redis的键

1. DEL key: 用于删除key，key存在删除后返回1，key不存在删除返回0

2. DUMP key: 序列化给定的key，并返回被序列化的的值

3. EXISTS key: 检测给定的key是否存在

4. EXPIRE key seconds: 为给定的key设置过期时间

5. EXPIREAT key timestamp: 为给定的key设置过期时间，支持unix时间戳

6. PERSIST key: 移除key的过期时间，key将持久保持

7. RENAME key newKey: 修改key的名称

8. TYPE key: 返回key所存储的值的类型

9. KEYS pattern: 查找所有符合给定模式的key

10. MOVE key db: 将当前数据库的key移动到给定的数据库的db当中


##### 字符(string)
字符串常用操作命令

1. SET key value: 设置指定key的值

2. GET key: 获取指定key的值

3. GETRANGE key value start end: 返回key中字符串值的子串

4. STRLEN key: 返回key所存储的字符串的长度

5. MGET key [key...]: 返回多个给定key的值

6. INCR key: 将key中存储的数字增一

7. DECR key: 将key中存储的数字减一



##### Hash(哈希)
hash基本常用命令

1. HSET key field value: 将哈希表key中的字段field的值设置为value

2. HGET key field: 获取存储在哈希表中指定字段的值

3. HKEYS key: 获取所有哈希表中所有的字段

4. HVALS key: 获取哈希表中所有的值

5. HMSET key field1 value1 [field2 value2]: 同时将多个field-value设置到哈希表key中

6. HMGET key field1 field2: 获取所有给字段的值

7. HLEN key: 获取哈希表中字段的数量


##### Lists(列表)
lists基本常用操作命令

1. LPUSH key value [value1]: 将一个或多个value插入到列表头部

2. LPOP key: 移除并获取列表的第一个元素

3. LLEN key: 获取列表长度

4. LPUSHX key value: 将一个或多个value插入到已经存在的列表头部

5. LRANGE key start end: 获取指定范围内的元素

6. LSET key index value: 通过索引设置列表元素的值

7. RPUSH key value [value1]: 在列表中添加一个或多个值

8. RPUSHX key value: 在已存在的列表添加值


##### Set(集合)
Set基本常用操作命令

1. SADD key member [member ...]: 向集合添加一个或多个成员

2. SCARD key: 获取集合的成员数

3. SMEMBER key: 返回集合中的所有成员

4. SPOP key: 移除并返回集合中的一个随机元素

5. SREM key member [...]: 移除集合中一个或多个元素


##### Sorted Set(有序集合)
sorted set基本常用操作命令

1. ZADD key score1 member1 [score2 member2]: 向集合中添加一个或多个元素

2. ZCARD key: 获取集合的成员数



[更多详细的命令请移步官方文档查看](https://redis.io/commands)

---
that's all